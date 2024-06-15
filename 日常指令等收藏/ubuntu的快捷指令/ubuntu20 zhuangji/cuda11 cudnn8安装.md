cuda安装：
直接官网下载对应安装包，使用sudo sh 安装.run的包就好
安装完添加环境，格式如下/////记得换地址。
```shell
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-10.1/lib64
export PATH=$PATH:/usr/local/cuda-10.1/bin
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda-10.1

```
## 第二步 ：Cudnn8.05的安装

### 1.进入官网：[https://developer.nvidia.com/rdp/cudnn-archive](https://developer.nvidia.com/rdp/cudnn-archive)下载

或者你也可以直接百度cudnn进入  
然后，需要登录，选择Sign in（如已有帐号自动忽略）  
注册后，打开页面：
```
sudo dpkg -i libcudnn8_8.0.5.39-1+cuda10.1_amd64.deb                       sudo dpkg -i libcudnn8-dev_8.0.5.39-1+cuda10.1_amd64.deb
sudo dpkg -i libcudnn8-samples_8.0.5.39-1+cuda10.1_amd64.deb
```
确认cudnn安装成功
通过上步骤的安装，得到了cudnn的安装路径：/usr/include/
因此，执行：

```
sudo cp /usr/include/cudnn.h /usr/local/cuda/include
sudo chmod a+x /usr/local/cuda/include/cudnn.h
```

复制cudnn的头文件到cuda文件夹中，然后确认cudnn安装是否成功：

```
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
cp -r /usr/src/cudnn_samples_v8/ $HOME
cd $HOME/cudnn_samples_v8/mnistCUDNN
make clean && make
./mnistCUDNN
```

最后测试：

```
cat /usr/local/cuda-11.0/include/cudnn_version.h | grep CUDNN_MAJOR -A  2
```