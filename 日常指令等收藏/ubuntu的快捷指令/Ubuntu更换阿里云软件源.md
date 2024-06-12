1. 备份

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak 
```

2.修改

```shell
sudo gedit /etc/apt/sources.list
```

将source.list文件内容替换成下面的

```shell
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse 
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse 
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse 
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse 
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse 
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse

```

3. 更新

```shell
sudo apt-get update
```

#### 更新一下软件即可。

```bash
sudo apt-get upgrade
```