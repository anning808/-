​官方安装部分
https://github.com/uzh-rpg/agile_autonomy
### 安装记录：

几个参考链接：

[“Learning High-Speed Flight in the Wild”安装方案_learning high-speed flight in the wild环境安装-CSDN博客](https://blog.csdn.net/L_egend_ing/article/details/128205911 "“Learning High-Speed Flight in the Wild”安装方案_learning high-speed flight in the wild环境安装-CSDN博客")

[Learning High-Speed Flight in the Wild 环境安装_in the wild环境搭建-CSDN博客](https://blog.csdn.net/qq_42412225/article/details/123537758 "Learning High-Speed Flight in the Wild 环境安装_in the wild环境搭建-CSDN博客")

[论文学习--Learning High-Speed Flight in the Wild-CSDN博客](https://blog.csdn.net/wxm__/article/details/121220816 "论文学习--Learning High-Speed Flight in the Wild-CSDN博客")

## 概述：
按照官方步骤走，先把官方的安装完，需要再安装open3d等库，以及对python3版本限制等。同时显卡和tensflow版本什么的都需要对应好，有些注意点对于小白并不友好。
在ubuntu18和20都安装了几遍，有一定的经验。建议还是ubuntu20安装。为了安装好能正确运行避免重装，最好确定好cuda版本之后再开始安装。
为了简单起见，下方整理一个笔者自己完整的安装方式。



我遇见的几个问题直接放在最后。

本机环境
ubuntu20   cuda11.2 cudnn8.0.5 
##### 先安装Open3D
源码安装C++版本的open3D， v0.9.0. 看完git的issue中最终选择比较稳定版本。装过其他版本，要改很多路径。为了直接能用

```
git clone --recursive https://github.com/intel-isl/Open3D

# You can also update the submodule manually
git submodule update --init --recursive

# 切换到指定版本
git checkout  v0.9.0

# On Ubuntu   install_deps_ubuntu.sh的路径可能不一样搜索一下去执行
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

#### 杂项：这些依赖提前装好，编译可以省很多事情

```
#依赖
sudo apt install python3-catkin-tools python3-osrf-pycommon python3-empy -y
sudo apt-get install python3-vcstool libsdl2-dev -y
sudo apt-get install libsdl-image1.2-dev libsdl1.2-dev -y
# 如果有python2的环境，首先设置软连接，切换到python3
# 设置软链接
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 100
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 150 
#依赖
sudo apt-get install ros-noetic-octomap-msgs ros-noetic-joy -y
```

安装mavros(编译报错geographic时装，需要清除后重新编译)//需要vpn
```
sudo apt-get install ros-noetic-mavros
cd /opt/ros/noetic/lib/mavros
sudo ./install_geographiclib_datasets.sh
```

 

## conda创建
我使用这个环境成功了，
```
name: tf_24
channels:
  - defaults
dependencies:
  - _libgcc_mutex=0.1=main
  - _openmp_mutex=5.1=1_gnu
  - ca-certificates=2024.3.11=h06a4308_0
  - certifi=2022.12.7=py37h06a4308_0
  - cudatoolkit=11.0.221=h6bb024c_0
  - ld_impl_linux-64=2.38=h1181459_1
  - libedit=3.1.20230828=h5eee18b_0
  - libffi=3.2.1=hf484d3e_1007
  - libgcc-ng=11.2.0=h1234567_1
  - libgomp=11.2.0=h1234567_1
  - libstdcxx-ng=11.2.0=h1234567_1
  - ncurses=6.4=h6a678d5_0
  - openssl=1.1.1w=h7f8727e_0
  - pip=22.3.1=py37h06a4308_0
  - python=3.7.3=h0371630_0
  - readline=7.0=h7b6447c_5
  - setuptools=65.6.3=py37h06a4308_0
  - sqlite=3.33.0=h62c20be_0
  - tk=8.6.12=h1ccaba5_0
  - wheel=0.38.4=py37h06a4308_0
  - xz=5.4.6=h5eee18b_0
  - zlib=1.2.13=h5eee18b_0
  - pip:
      - absl-py==0.15.0
      - anyio==3.7.1
      - argon2-cffi==23.1.0
      - argon2-cffi-bindings==21.2.0
      - astunparse==1.6.3
      - attrs==23.2.0
      - backcall==0.2.0
      - beautifulsoup4==4.12.3
      - bleach==6.0.0
      - cachetools==5.3.3
      - catkin-pkg==1.0.0
      - cffi==1.15.1
      - charset-normalizer==3.3.2
      - comm==0.1.4
      - cycler==0.11.0
      - debugpy==1.7.0
      - decorator==5.1.1
      - defusedxml==0.7.1
      - distro==1.9.0
      - docutils==0.20.1
      - entrypoints==0.4
      - exceptiongroup==1.2.0
      - fastjsonschema==2.19.1
      - flatbuffers==1.12
      - fonttools==4.38.0
      - gast==0.3.3
      - google-auth==2.29.0
      - google-auth-oauthlib==0.4.6
      - google-pasta==0.2.0
      - grpcio==1.32.0
      - h5py==2.10.0
      - idna==3.7
      - importlib-metadata==6.7.0
      - importlib-resources==5.12.0
      - ipykernel==6.16.2
      - ipython==7.34.0
      - ipython-genutils==0.2.0
      - ipywidgets==8.1.2
      - jedi==0.19.1
      - jinja2==3.1.3
      - jsonschema==4.17.3
      - jupyter-client==7.4.9
      - jupyter-core==4.12.0
      - jupyter-server==1.24.0
      - jupyterlab-pygments==0.2.2
      - jupyterlab-widgets==3.0.10
      - keras-preprocessing==1.1.2
      - kiwisolver==1.4.5
      - markdown==3.4.4
      - markupsafe==2.1.5
      - matplotlib==3.5.3
      - matplotlib-inline==0.1.6
      - mistune==3.0.2
      - nbclassic==1.0.0
      - nbclient==0.7.4
      - nbconvert==7.6.0
      - nbformat==5.8.0
      - nest-asyncio==1.6.0
      - nets==0.0.3.1
      - notebook==6.5.6
      - notebook-shim==0.2.4
      - numpy==1.19.5
      - oauthlib==3.2.2
      - open3d==0.9.0.0
      - opencv-python==4.9.0.80
      - opt-einsum==3.3.0
      - packaging==24.0
      - pandas==1.3.5
      - pandocfilters==1.5.1
      - parso==0.8.4
      - pexpect==4.9.0
      - pickleshare==0.7.5
      - pillow==9.5.0
      - pkgutil-resolve-name==1.3.10
      - prometheus-client==0.17.1
      - prompt-toolkit==3.0.43
      - protobuf==3.20.3
      - psutil==5.9.8
      - ptyprocess==0.7.0
      - pyasn1==0.5.1
      - pyasn1-modules==0.3.0
      - pycparser==2.21
      - pygments==2.17.2
      - pyparsing==3.1.2
      - pyquaternion==0.9.9
      - pyrsistent==0.19.3
      - python-dateutil==2.9.0.post0
      - pytz==2024.1
      - pyyaml==6.0.1
      - pyzmq==24.0.1
      - requests==2.31.0
      - requests-oauthlib==2.0.0
      - rospkg==1.2.3
      - rsa==4.9
      - scipy==1.7.3
      - send2trash==1.8.3
      - six==1.15.0
      - sniffio==1.3.1
      - soupsieve==2.4.1
      - tensorboard==2.11.2
      - tensorboard-data-server==0.6.1
      - tensorboard-plugin-wit==1.8.1
      - tensorflow-estimator==2.4.0
      - tensorflow-gpu==2.4.0
      - termcolor==1.1.0
      - terminado==0.17.1
      - tinycss2==1.2.1
      - tornado==6.2
      - tqdm==4.66.2
      - traitlets==5.9.0
      - typing-extensions==3.7.4.3
      - urllib3==2.0.7
      - utils==1.0.2
      - wcwidth==0.2.13
      - webencodings==0.5.1
      - websocket-client==1.6.1
      - werkzeug==2.2.3
      - widgetsnbextension==4.0.10
      - wrapt==1.12.1
      - zipp==3.15.0
prefix: /home/b/anaconda3/envs/tf_24
```

建一个environment.yml文件把上述内容复制进去，使用conda直接建立环境。
```
conda env create -f environment.yml
```
理论上编译和环境就都完成了。

conda环境2
也可以按照官方方法建立conda，但是要改几个内容
```
conda create --name tf_24 python=3.7.3
conda activate tf_24
pip install tensorflow-gpu==2.4
pip  install pycocotools
pip install rospkg==1.2.3 pyquaternion open3d opencv-python
```
### Step-by-Step Procedure

[](https://github.com/uzh-rpg/agile_autonomy#step-by-step-procedure)

Use the following commands to create a new catkin workspace and a virtual environment with all the required dependencies.

```shell
export ROS_VERSION=noetic
mkdir agile_autonomy_ws
cd agile_autonomy_ws
export CATKIN_WS=./catkin_aa
mkdir -p $CATKIN_WS/src
cd $CATKIN_WS
catkin init
catkin config --extend /opt/ros/$ROS_VERSION
catkin config --merge-devel
catkin config --cmake-args -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS=-fdiagnostics-color
cd src

git clone git@github.com:uzh-rpg/agile_autonomy.git
vcs-import < agile_autonomy/dependencies.yaml
cd rpg_mpl_ros
git submodule update --init --recursive

#install extra dependencies (might need more depending on your OS)
sudo apt-get install libqglviewer-dev-qt5

# Install external libraries for rpg_flightmare
sudo apt install -y libzmqpp-dev libeigen3-dev libglfw3-dev libglm-dev

# Install dependencies for rpg_flightmare renderer
sudo apt install -y libvulkan1 vulkan-utils gdb

# Add environment variables (Careful! Modify path according to your local setup)
echo 'export RPGQ_PARAM_DIR=/home/<path/to/>catkin_aa/src/rpg_flightmare' >> ~/.bashrc
```

Now open a new terminal and type the following commands.

```shell
# Build and re-source the workspace
catkin build -DPYTHON_EXECUTABLE=/usr/bin/python3
. ../devel/setup.bash

# Create your learning environment
roscd planner_learning
conda create --name tf_24 python=3.7
conda activate tf_24
pip install tensorflow-gpu==2.4
pip install rospkg==1.2.3 pyquaternion open3d opencv-python
```
## Let's Fly!

[](https://github.com/uzh-rpg/agile_autonomy#lets-fly)

Once you have installed the dependencies, you will be able to fly in simulation with our pre-trained checkpoint. You don't need necessarily need a GPU for execution. Note that if the network can't run at least at 15Hz, you won't be able to fly successfully.

Launch the simulation! Open a terminal and type:

```shell
cd agile_autonomy_ws
source catkin_aa/devel/setup.bash
roslaunch agile_autonomy simulation.launch
```

Run the Network in an other terminal:

```shell
cd agile_autonomy_ws
source catkin_aa/devel/setup.bash
conda activate tf_24
python test_trajectories.py --settings_file=config/test_settings.yaml

```

# 运行 test_trajectories.py 时，“PlanLearning”对象没有属性“XXX”
基本上就是 cuda cudnn没配置好 要和机子显卡还有tensflow对应 -需要重新改。 

# typing-extensions版本有个错误 
ｐｉｐ　里面几个一起装


## 别人安装方案１
```
conda activate tf24
# So far cuda11 isn't included in conda , so it must be added "-c conda-forge", which is not needed in the future
conda install cudatoolkit=11.1 -c conda-forge
conda install cudnn -c conda-forge
pip install tensorflow-gpu==2.4
pip install rospkg==1.2.3 pyquaternion open3d opencv-python pyyaml
```


installing the package 'python3-empy'
ｃｏｎｄａ　里面编译在
sudo apt install python3-empy 之后还报错，那么就是默认使用ｐｙｔｈｏｎ２了
使用下述方法切换优先级
```
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 100
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 150 
```
```
pip uninstall em
pip install empy==3.3.4
```
#### 有个　ｎｕｍｐｙ_eignt错误
```
error: return-statement with a value, in function returning 'void' [-fpermissive]  
#define import_array() {if (_import_array() < 0) {PyErr_Print(); PyErr_SetString(PyExc_ImportError, "numpy.core.multiarray failed to import"); return NULL; } }  
^  
/home/b/projects/7guihua/agile_autonomy_wsnew/catkin_aa/src/numpy_eigen/src/autogen_module/numpy_eigen_export_module.cpp:258:2: note: in expansion of macro ‘import_array’  
import_array();
```
numpy_eigen_export_module.cpp的第三行加一句：
```
#define import_array() {if (_import_array() < 0) {PyErr_Print(); PyErr_SetString(PyExc_ImportError, "numpy.core.multiarray failed to import"); return; } }
```

pip install PySide2　PyQt5