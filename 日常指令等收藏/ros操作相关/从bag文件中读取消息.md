首先，您需要一个袋（bag）文件。可以按照[上一教程](https://wiki.ros.org/ROS/Tutorials/Recording%20and%20playing%20back%20data)自己生成，或从https://webviz.io下载一个[示例文件](https://open-source-webviz-ui.s3.amazonaws.com/demo.bag)：

wget https://open-source-webviz-ui.s3.amazonaws.com/demo.bag

接下来的教程将用上面的示例文件进行演示。有两种方法从bag文件中回放或提取消息。

_请注意，下面所有的命令中，前面都有一个time，这样做可以同时输出执行每个命令花费的时间，而且有时这些命令需要很长时间，因此使用time命令了解给定命令所需的时间是有必要的。如果您不想使用它，可以放心删除下面任何命令中的time。_

# 方法1：立即回放消息并在多个终端中查看输出

[来源：这部分教程是根据本文件中首次发表的指引改编的。](https://github.com/ElectricRCAircraftGuy/eRCaGuy_dotfiles/blob/master/git%20%26%20Linux%20cmds%2C%20help%2C%20tips%20%26%20tricks%20-%20Gabriel.txt)

1. 你需要知道你想从bag文件中读取的**准确**话题名。那让我们看看袋子里有什么。在任何终端中用这个命令，来手动检查所有已发布的话题，以及向每个话题发布了多少消息：
    
    time rosbag info demo.bag  
    # 或者你已经知道话题名称的话：
    time rosbag info mybag.bag | grep -E "(topic1|topic2|topic3)"
    
    你会看到：
    
    $ time rosbag info demo.bag  
    path:        demo.bag
    version:     2.0
    duration:    20.0s
    start:       Mar 21 2017 19:37:58.00 (1490150278.00)
    end:         Mar 21 2017 19:38:17.00 (1490150298.00)
    size:        696.2 MB
    messages:    5390
    compression: none [600/600 chunks]
    types:       bond/Status                      [eacc84bf5d65b6777d4c50f463dfb9c8]
                 diagnostic_msgs/DiagnosticArray  [60810da900de1dd6ddd437c3503511da]
                 diagnostic_msgs/DiagnosticStatus [d0ce08bc6e5ba34c7754f563a9cabaf1]
                 nav_msgs/Odometry                [cd5e73d190d741a2f92e81eda573aca7]
                 radar_driver/RadarTracks         [6a2de2f790cb8bb0e149d45d297462f8]
                 sensor_msgs/Image                [060021388200f6f0f447d0fcd9c64743]
                 sensor_msgs/NavSatFix            [2d3a8cd499b9b4a0249fb98fd05cfa48]
                 sensor_msgs/PointCloud2          [1158d486dd51d683ce2f1be655c3c181]
                 sensor_msgs/Range                [c005c34273dc426c67a020a87bc24148]
                 sensor_msgs/TimeReference        [fded64a0265108ba86c3d38fb11c0c16]
                 tf2_msgs/TFMessage               [94810edda583a504dfda3829e70d7eec]
                 velodyne_msgs/VelodyneScan       [50804fc9533a0e579e6322c04ae70566]
    topics:      /diagnostics                      140 msgs    : diagnostic_msgs/DiagnosticArray 
                 /diagnostics_agg                   40 msgs    : diagnostic_msgs/DiagnosticArray 
                 /diagnostics_toplevel_state        40 msgs    : diagnostic_msgs/DiagnosticStatus
                 /gps/fix                          146 msgs    : sensor_msgs/NavSatFix           
                 /gps/rtkfix                       200 msgs    : nav_msgs/Odometry               
                 /gps/time                         192 msgs    : sensor_msgs/TimeReference       
                 /image_raw                        600 msgs    : sensor_msgs/Image               
                 /obs1/gps/fix                      30 msgs    : sensor_msgs/NavSatFix           
                 /obs1/gps/rtkfix                  200 msgs    : nav_msgs/Odometry               
                 /obs1/gps/time                    136 msgs    : sensor_msgs/TimeReference       
                 /radar/points                     400 msgs    : sensor_msgs/PointCloud2         
                 /radar/range                      400 msgs    : sensor_msgs/Range               
                 /radar/tracks                     400 msgs    : radar_driver/RadarTracks        
                 /tf                              1986 msgs    : tf2_msgs/TFMessage              
                 /velodyne_nodelet_manager/bond     80 msgs    : bond/Status                     
                 /velodyne_packets                 200 msgs    : velodyne_msgs/VelodyneScan      
                 /velodyne_points                  200 msgs    : sensor_msgs/PointCloud2
    real    0m1.003s
    user    0m0.620s
    sys 0m0.283s
    
    可以看到，有30条消息发布在/obs1/gps/fix话题上，有40条消息发布在/diagnostics_agg话题上。让我们把这些提取出来。
    
2. 在终端1（比如本终端）中，启动roscore，这样可以运行必需的ROS主节点：
    
    roscore
    
3. 打开另一个终端（试试按下Ctrl + Shift + T），订阅/obs1/gps/fix话题并复读该话题上发布的所有内容，同时用tee命令转储到一个yaml格式的文件中以便之后查看：
    
    rostopic echo /obs1/gps/fix | tee topic1.yaml
    
    你会看到：
    
    $ rostopic echo /obs1/gps/fix | tee topic1.yaml
    WARNING: topic [/obs1/gps/fix] does not appear to be published yet
    
4. 再打开一个新终端，订阅另一个话题/diagnostics_agg。
    
    rostopic echo /diagnostics_agg | tee topic2.yaml
    
    你会看到：
    
    $ rostopic echo /diagnostics_agg | tee topic2.yaml
    WARNING: topic [/diagnostics_agg] does not appear to be published yet
    
5. 对其他你感兴趣的话题重复这一步骤，每个话题必须有自己的终端。
6. 再打开另一个新终端来回放bag文件。这一次我们将尽可能快地回放bag文件（使用--immediate选项），**只**会发布我们感兴趣的话题。格式如下：
    
    time rosbag play --immediate demo.bag --topics /topic1 /topic2 /topic3 /topicN
    
    本例中，命令如下：
    
    time rosbag play --immediate demo.bag --topics /obs1/gps/fix /diagnostics_agg
    
    你会看到：
    
    $ time rosbag play --immediate demo.bag --topics /obs1/gps/fix /diagnostics_agg
    [ INFO] [1591916465.758724557]: Opening demo.bag
    Waiting 0.2 seconds after advertising topics... done.
    Hit space to toggle paused, or 's' to step.
     [RUNNING]  Bag Time: 1490150297.770734   Duration: 19.703405 / 19.703405               
    Done.
    real  0m1.570s
    user  0m0.663s
    sys 0m0.394s
    
7. 完成！现在看一下你的两个终端，每个终端都订阅了一个话题，每个话题类型的所有消息用YAML格式输出，每条消息之间用---分割。用你喜欢的文本编辑器（最好支持YAML的语法高亮，例如Visual Studio Code）来查看文件中的消息。例如，topic1.yaml中的最后两条消息是这样的：
    
    ---
    header: 
      seq: 4027
      stamp: 
        secs: 1490150296
        nsecs:  66947432
      frame_id: "gps"
    status: 
      status: 0
      service: 1
    latitude: 37.4008017844
    longitude: -122.108119889
    altitude: -6.4380177824
    position_covariance: [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0]
    position_covariance_type: 0
    ---
    header: 
      seq: 4028
      stamp: 
        secs: 1490150297
        nsecs: 744347249
      frame_id: "gps"
    status: 
      status: 0
      service: 1
    latitude: 37.4007565466
    longitude: -122.108159482
    altitude: -6.35130467023
    position_covariance: [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0]
    position_covariance_type: 0
    ---
    
    如果由于一些原因某个rostopic进程丢失了消息，可以使用Ctrl+C终止该进程，然后重新启动它，并再次调用rosbag play命令。
    

# 方法2：使用ros_readbagfile脚本轻松地提取感兴趣的话题

来源：[这部分教程是根据本文件中首次发表的指引改编的](https://github.com/ElectricRCAircraftGuy/eRCaGuy_dotfiles/blob/master/git%20%26%20Linux%20cmds%2C%20help%2C%20tips%20%26%20tricks%20-%20Gabriel.txt),[Python脚本来自：ros_readbag.py](https://github.com/ElectricRCAircraftGuy/eRCaGuy_dotfiles/blob/master/useful_scripts/ros_readbagfile.py)。

注意：您可以杀死任何正在运行的进程。比如说连roscore都不需要运行。

1. 下载并安装[`ros_readbag.py`](https://github.com/ElectricRCAircraftGuy/eRCaGuy_dotfiles/blob/master/useful_scripts/ros_readbagfile.py)：
    
    # Download the file
    wget https://raw.githubusercontent.com/ElectricRCAircraftGuy/eRCaGuy_dotfiles/master/useful_scripts/ros_readbagfile.py
    # Make it executable
    chmod +x ros_readbagfile.py
    # Ensure you have the ~/bin directory for personal binaries
    mkdir -p ~/bin
    # Move this executable script into that directory as `ros_readbagfile`, so that it will
    # be available as that command
    mv ros_readbagfile.py ~/bin/ros_readbagfile
    # Re-source your ~/.bashrc file to ensure ~/bin is in your PATH, so you can use this
    # new `ros_readbagfile` command you just installed
    . ~/.bashrc
    
2. 通过rosbag info命令确定要从bag文件中读取的**准确**话题名，如上面方法1的第一步所示。
    
3. 然后使用ros_readbagfile，大体格式如下：
    
    ros_readbagfile <mybagfile.bag> [topic1] [topic2] [topic3] [...]
    
    要阅读上面方法1中显示的相同消息，请使用：
    
    time ros_readbagfile demo.bag /obs1/gps/fix /diagnostics_agg | tee topics.yaml
    
    就是这样！你会看到它快速打印出所有70条信息。以下是终端输出的最后部分：
    
            key: "Early diagnostic update count:"
            value: "0"
          - 
            key: "Zero seen diagnostic update count:"
            value: "0"
    =======================================
    topic:           /obs1/gps/fix
    msg #:           30
    timestamp (sec): 1490150297.770734310
    - - -
    header: 
      seq: 4028
      stamp: 
        secs: 1490150297
        nsecs: 744347249
      frame_id: "gps"
    status: 
      status: 0
      service: 1
    latitude: 37.4007565466
    longitude: -122.108159482
    altitude: -6.35130467023
    position_covariance: [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0]
    position_covariance_type: 0
    =======================================
    Total messages found = 70.
    DONE.
    real  0m2.897s
    user  0m2.457s
    sys 0m0.355s
    
    现在用你喜欢的文本编辑器打开**topics.yaml**，看看它从bag文件中提取的所有消息。
    
    _请注意，尽管我给了这个文件一个“.yaml”扩展名，但并不代表所有部分都是正确的YAML格式。相反，尽管文件中存储的每条消息都是有效的YAML语法，但是消息之间的标题和行分隔符（例如=====）不是有效的。请记住这一点，避免试图将输出解析为YAML。如果你愿意，也可以很容易地修改ros_readbagfile这个Python脚本来删除这些非YAML特性。_
    

# 为什么用ros_readbagfile而不是rostopic echo -b呢？

1. 因为rostopic**极其地**慢！ 举个例子，就算在高配计算机（4核8线程的奔腾i7和m.2固态硬盘）上运行这个命令，也需要**11.5分钟**才能读取一个18GB的bag文件！
    
    time rostopic echo -b large_bag_file.bag /topic1
    
    而用ros_readbagfile脚本，在相同计算机上只要花费**1分钟37秒**就能读取同样的话题和18GB的bag文件！因此ros_readbagfile比rostopic快了11.5/(1+37/60) = **大约7倍**！
    
    time ros_readbagfile large_bag_file.bag /topic1
    
2. 因为rostopic一次只能读取**单个话题**，而ros_readbagfile可以同时读取**任意多的话题**！
    
    ros_readbagfile <mybagfile.bag> [topic1] [topic2] [topic3] [...] [topic1000]
    

就酱。

现在，您已经了解了如何从预先录制的包文件中读取消息，可以开始[roswtf入门](https://wiki.ros.org/cn/ROS/Tutorials/Getting%20started%20with%20roswtf)了。