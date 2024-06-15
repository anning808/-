PX4开发环境搭建

    编译工具链的安装
    vscode

编译工具链的安装

下载ubuntu.sh,requirements.txt

```shell
wget https://raw.githubusercontent.com/PX4/Firmware/master/Tools/setup/ubuntu.sh
wget https://raw.githubusercontent.com/PX4/Firmware/master/Tools/setup/requirements.txt
```

或者: 手动到 px4 github下载

然后运行：

```shell
source ubuntu.sh
```



在此目录下下载px4源码并切换v1.11.0-beta1的固件

```shell
git clone https://github.com/PX4/Firmware
```


然后更新submodule切换固件并编译

```shell
cd Firmware
git submodule update --init --recursive
git checkout v1.11.0-beta1
make distclean
```


编译代码
仿真

```shell
make px4_sitl_default gazebo
```

