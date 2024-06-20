## 录制数据（创建bag文件）

这一部分将指导你如何从正在运行的ROS系统中记录话题数据。话题数据将被积累到一个袋（bag）文件中。
首先，在**不同**的终端中分别执行：
_终端1：_
```
roscore
```
_终端2：_
```
rosrun turtlesim turtlesim_node
```
_终端3：_
```
rosrun turtlesim turtle_teleop_key
```
这将启动两个节点——turtlesim可视化工具和一个能让你用键盘方向键控制turtlesim的节点。如果你选中了启动turtle_teleop_key的终端窗口，你应该会看到如下内容：
Reading from keyboard
---------------------------
Use arrow keys to move the turtle. 'q' to quit.

按键盘上的箭头键可以使乌龟在屏幕上移动。请注意，要移动乌龟，你必须选中启动turtle_teleop_key的终端窗口，而不是turtlesim。

### 录制所有发布的话题

首先让我们来检查一下当前系统中发布的所有话题。打开一个新终端：

rostopic list -v

输出类似于：

Published topics:
 * /rosout_agg [rosgraph_msgs/Log] 1 publisher
 * /rosout [rosgraph_msgs/Log] 2 publishers
 * /turtle1/pose [turtlesim/Pose] 1 publisher
 * /turtle1/color_sensor [turtlesim/Color] 1 publisher
 * /turtle1/cmd_vel [geometry_msgs/Twist] 1 publisher
Subscribed topics:
 * /rosout [rosgraph_msgs/Log] 1 subscriber
 * /turtle1/cmd_vel [geometry_msgs/Twist] 1 subscriber

已发布主题的列表是唯一可能被记录在数据日志文件中的消息类型，因为只有发布的消息才能被录制。由teleop_turtle发布的/turtle1/cmd_vel话题是指令消息，作为turtlesim进程的输入。消息/turtle1/color_sensor和/turtle1/pose是turtlesim发布的输出消息。

现在我们将记录发布的数据。打开一个新终端：

mkdir ~/bagfiles
cd ~/bagfiles
rosbag record -a

这里我们只是创建了一个临时目录来记录数据，然后运行rosbag record带选项-a，表明所有发布的话题都应该积累在一个bag文件中。

然后回到turtle_teleop节点所在的终端窗口并控制乌龟随意移动10秒钟左右。

在运行rosbag record的窗口中按Ctrl+C以退出。现在查看~/bagfiles目录中的内容，你应该会看到一个以年份、日期和时间开头且扩展名是.bag的文件。这就是传说中的袋文件，它包含rosbag record运行期间由任何节点发布的所有话题。

## 检查并回放bag文件

现在我们已经使用rosbag record命令录制了一个bag文件，接下来我们可以使用rosbag info查看它的内容，或用rosbag play命令回放。首先我们来看看袋子里记录了什么。我们可以执行**info**命令检查bag文件的内容而不回放它。在bag文件所在的目录下执行以下命令：

rosbag info <your bagfile>

你会看到：

path:        2014-12-10-20-08-34.bag
version:     2.0
duration:    1:38s (98s)
start:       Dec 10 2014 20:08:35.83 (1418270915.83)
end:         Dec 10 2014 20:10:14.38 (1418271014.38)
size:        865.0 KB
messages:    12471
compression: none [1/1 chunks]
types:       geometry_msgs/Twist [9f195f881246fdfa2798d1d3eebca84a]
             rosgraph_msgs/Log   [acffd30cd6b6de30f120938c17c593fb]
             turtlesim/Color     [353891e354491c51aabe32df673fb446]
             turtlesim/Pose      [863b248d5016ca62ea2e895ae5265cf9]
topics:      /rosout                    4 msgs    : rosgraph_msgs/Log   (2 connections)
             /turtle1/cmd_vel         169 msgs    : geometry_msgs/Twist
             /turtle1/color_sensor   6149 msgs    : turtlesim/Color
             /turtle1/pose           6149 msgs    : turtlesim/Pose

这些信息告诉你bag文件中所包含话题的名称、类型和消息数量。我们可以看到，在之前使用**rostopic**命令查看到的五个已公告的话题中，其实只有四个在我们录制期间发布了消息。因为我们带-a参数选项运行**rosbag record**命令时系统会录制下所有节点发布的所有消息。

下一步是回放bag文件以再现系统运行过程。首先用Ctrl+C杀死之前运行的turtle_teleop_key，但让turtlesim继续运行。在终端中bag文件所在目录下运行以下命令：

rosbag play <your bagfile>

在这个窗口中你应该会立即看到如下类似信息：

[ INFO] [1418271315.162885976]: Opening 2014-12-10-20-08-34.bag
Waiting 0.2 seconds after advertising topics... done.
Hit space to toggle paused, or 's' to step.

默认模式下，**rosbag play**命令在公告每条消息后会等待一小段时间（0.2秒）才真正开始发布bag文件中的内容。等待一段时间是为了可以通知订阅者，消息已经公告且数据可能会马上到来。如果**rosbag play**在公告消息后立即发布，订阅者可能会接收不到几条最先发布的消息。等待时间可以通过-d选项来指定。

最终/turtle1/cmd_vel话题将会被发布，同时在turtuelsim中乌龟应该会像之前用turtle_teleop_key控制它那样开始移动。从运行**rosbag play**到乌龟开始移动时所经历时间应该近似等于之前在本教程开始部分运行**rosbag record**后到开始按下键盘发出控制命令时所经历时间。你可以通过-s参数选项让**rosbag play**不从bag文件的开头开始，而是从某个指定的时间开始。最后一个可能比较有趣的参数选项是-r选项，它允许你通过设定一个参数来改变消息发布速率。如果执行：

rosbag play -r 2 <your bagfile>

你应该会看到乌龟的运动轨迹有点不同了，这时的轨迹应该是相当于当你以两倍的速度通过按键发布控制命令时产生的轨迹。

## 录制数据子集

当运行一个复杂的系统时，比如PR2软件套装，会有几百个话题被发布，有些话题亦会发布大量数据（比如包含摄像头图像流的话题）。在这种系统中，将包含所有话题的日志写入一个bag文件到磁盘通常是不切实际的。**rosbag record**命令支持只录制特定的话题到bag文件中，这样就可以只录制用户感兴趣的话题。

如果还有turtlesim节点在运行，先退出他们，然后重新启动键盘控制节点相关的启动文件：

rosrun turtlesim turtlesim_node 
rosrun turtlesim turtle_teleop_key

在bag文件所在目录下执行以下命令：

rosbag record -O subset /turtle1/cmd_vel /turtle1/pose

上述命令中的-O参数告诉**rosbag record**将数据记录到名为subset.bag的文件中，而后面的topic参数告诉**rosbag record**只能订阅这两个指定的话题。然后通过键盘控制乌龟随意移动几秒钟，最后按Ctrl+C退出**rosbag record**命令。

现在看看bag文件中的内容（rosbag info subset.bag）。你应该会看到如下类似信息，里面只包含指定的话题：

path:        subset.bag
version:     2.0
duration:    12.6s
start:       Dec 10 2014 20:20:49.45 (1418271649.45)
end:         Dec 10 2014 20:21:02.07 (1418271662.07)
size:        68.3 KB
messages:    813
compression: none [1/1 chunks]
types:       geometry_msgs/Twist [9f195f881246fdfa2798d1d3eebca84a]
             turtlesim/Pose      [863b248d5016ca62ea2e895ae5265cf9]
topics:      /turtle1/cmd_vel    23 msgs    : geometry_msgs/Twist
             /turtle1/pose      790 msgs    : turtlesim/Pose

## rosbag录制和回放的局限性

在上一小节中，你可能已经注意到了乌龟的路径可能并没有完全地映射到原先通过键盘控制时产生的路径上——整体形状应该是差不多的，但没有完全一样。这是因为turtlesim的移动路径对系统定时精度的变化非常敏感。rosbag受制于其本身的性能无法完全复制录制时的系统运行行为，rosplay也一样。对于像turtlesim这样的节点，当处理消息的过程中系统定时发生极小变化时也会使其行为发生微妙变化，用户不应该期望能够完美地模仿系统行为。
