cd ~/projects/autoland_ws/Firmware && make px4_sitl_default gazebo
cd ~/projects/autoland_ws && sd &&  rosrun nb_ctrl cy_trak
roslaunch mavros px4.launch fcu_url:=udp://:14540@localhost:14580