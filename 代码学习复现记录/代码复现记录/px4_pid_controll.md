
# 绕圈
## takeoff
```c++
void flightBase::takeoff(){
geometry_msgs::PoseStamped ps;//设定一个几何点作为目标
ps.header.frame_id = "map";
ps.header.stamp = ros::Time::now();
ps.pose.position.x = this->odom_.pose.pose.position.x;
ps.pose.position.y = this->odom_.pose.pose.position.y;
ps.pose.position.z = this->takeoffHgt_;
ps.pose.orientation = this->odom_.pose.pose.orientation;
this->updateTarget(ps);//更新位置给发布器
cout << "[AutoFlight]: Start taking off..." << endl;
ros::Rate r (30);
while (ros::ok() and
	   std::abs(this->odom_.pose.pose.position.z - this->takeoffHgt_) >= 0.1){
//ｚ轴高度误差０.1 之后开始飞行
ros::spinOnce();
r.sleep();
}
ros::Time startTime = ros::Time::now();
while (ros::ok()){
ros::Time currTime = ros::Time::now();
if ((currTime - startTime).toSec() >= 3){//三秒后销毁
break;
}
ros::spinOnce();
r.sleep();
}
cout << "[AutoFlight]: Takeoff succeed!" << endl;
}
```
- **this->updateTarget(ps);**
```c++
void flightBase::updateTarget(const geometry_msgs::PoseStamped& ps){
this->poseTgt_ = ps;// 这里的位置是给publishTarget使用的，进行位置发布。
this->poseTgt_.header.frame_id = "map";
this->poseControl_ = true;
}
```
## circle
- 获得各种飞行参数
- //初始化数值
- //计算姿态并上传
- 第一步逐步扩大，第二部绕圈，第三部逐步缩小。
## stop
-　发布位置点结束


# 控制

## 初始化
```c++
trackingController::trackingController(const ros::NodeHandle& nh) : nh_(nh){
this->initParam();  //读参数　设定控制参数
this->registerPub();
this->registerCallback();
}
```


**几个发布**
```c++
void trackingController::registerPub(){
// command publisher
this->cmdPub_ = this->nh_.advertise<mavros_msgs::AttitudeTarget>("/mavros/setpoint_raw/attitude", 100);
// acc comman publisher
this->accCmdPub_ = this->nh_.advertise<mavros_msgs::PositionTarget>("/mavros/setpoint_raw/local", 100);
// current pose visualization publisher
this->poseVisPub_ = this->nh_.advertise<geometry_msgs::PoseStamped>("/tracking_controller/robot_pose", 1);
// trajectory history visualization publisher
this->histTrajVisPub_ = this->nh_.advertise<nav_msgs::Path>("/tracking_controller/trajectory_history", 1);
// target pose visualization publisher
this->targetVisPub_ = this->nh_.advertise<geometry_msgs::PoseStamped>("/tracking_controller/target_pose", 1);
// target trajectory history publisher
this->targetHistTrajVisPub_ = this->nh_.advertise<nav_msgs::Path>("/tracking_controller/target_trajectory_history", 1);
// velocity and acceleration visualization publisher
this->velAndAccVisPub_ = this->nh_.advertise<visualization_msgs::Marker>("/tracking_controller/vel_and_acc_info", 1);
}
```

**几个回调**
```c++
void trackingController::registerCallback(){
// odom subscriber
this->odomSub_ = this->nh_.subscribe("/mavros/local_position/odom", 1, &trackingController::odomCB, this);
// imu subscriber
this->imuSub_ = this->nh_.subscribe("/mavros/imu/data", 1, &trackingController::imuCB, this);
// target setpoint subscriber
this->targetSub_ = this->nh_.subscribe("/autonomous_flight/target_state", 1, &trackingController::targetCB, this);
// controller publisher timer
this->cmdTimer_ = this->nh_.createTimer(ros::Duration(0.01), &trackingController::cmdCB, this);
if (not this->accControl_){
// auto thrust esimator timer
this->thrustEstimatorTimer_ = this->nh_.createTimer(ros::Duration(0.01), &trackingController::thrustEstimateCB, this);
}
// visualization timer
this->visTimer_ = this->nh_.createTimer(ros::Duration(0.033), &trackingController::visCB, this);
}
```
**其他几个主要是消息传递，对有作用的进行解释**
```c++
void trackingController::cmdCB(const ros::TimerEvent&){
if (not this->odomReceived_ or not this->targetReceived_){return;}
Eigen::Vector4d cmd;
// 1. Find target reference attitude from the desired acceleration
Eigen::Vector4d attitudeRefQuat;
Eigen::Vector3d accRef;
this->computeAttitudeAndAccRef(attitudeRefQuat, accRef);
if (this->bodyRateControl_){
// 2. Compute the body rate from the reference attitude
this->computeBodyRate(attitudeRefQuat, accRef, cmd);
// 3. publish body rate as control input
this->publishCommand(cmd);
}
if (this->attitudeControl_){
// direct attitude control
cmd = attitudeRefQuat;
this->publishCommand(cmd, accRef);
}
if (this->accControl_){
this->publishCommand(accRef);
}
this->targetReceived_ = false;
}
```
## 主要函数解释
```c++
this->computeAttitudeAndAccRef(attitudeRefQuat, accRef);
```
是