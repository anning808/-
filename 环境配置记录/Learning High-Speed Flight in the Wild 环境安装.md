​

###  安装记录：

几个参考链接：

[“Learning High-Speed Flight in the Wild”安装方案_learning high-speed flight in the wild环境安装-CSDN博客](https://blog.csdn.net/L_egend_ing/article/details/128205911 "“Learning High-Speed Flight in the Wild”安装方案_learning high-speed flight in the wild环境安装-CSDN博客")

[Learning High-Speed Flight in the Wild 环境安装_in the wild环境搭建-CSDN博客](https://blog.csdn.net/qq_42412225/article/details/123537758 "Learning High-Speed Flight in the Wild 环境安装_in the wild环境搭建-CSDN博客")

[论文学习--Learning High-Speed Flight in the Wild-CSDN博客](https://blog.csdn.net/wxm__/article/details/121220816 "论文学习--Learning High-Speed Flight in the Wild-CSDN博客")

### 前期准备中，github没有给出的几个：

##### 先安装Open3D源码安装C++版本的open3D， v0.9.0. 由于采用gcc6编译，因此不用最新版本v0.13.0的open3D。

```
git clone --recursive https://github.com/intel-isl/Open3D

# You can also update the submodule manually
git submodule update --init --recursive

# 切换到指定版本
git checkout  v0.9.0

# On Ubuntu
util/install_deps_ubuntu.sh

mkdir build
cd build
cmake ..
# On Ubuntu
make -j$(nproc)
sudo make install
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

#### 安装完成之后，再安装gcc/g++ 7.5.0

```
# 首先安装gcc7.5.0
sudo apt install gcc-7 g++-7
# 修改优先级，首先使用gcc/g++ 7.5.0，最后面的数字是优先级，谁的大，就是选择谁。
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 100
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 100
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

杂项：

```
#依赖
sudo apt install python3-catkin-tools python3-osrf-pycommon
sudo apt-get install python3-vcstool
# 如果有python2的环境，首先设置软连接，切换到python3
# 设置软链接
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 100
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 150
sudo apt-get install python3-empy
#依赖
sudo apt-get install ros-noetic-octomap-msgs
sudo apt-get install ros-noetic-joy
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

  

​