第一步，安装dkms：

```bash
 sudo apt-get install dkms
```

第二步，查看本机连接不上的驱动版本：

```bash
    ls -l /usr/src/
```
第三步，使用dkms重新安装适合驱动:

```bash
  sudo dkms install -m nvidia -v 470.103.01
```

这条命令 -v 后面需要填写本机的nvidia驱动版本，根据第二步得到！

重启系统
如果安装失败了gcc版本更改。

```bash
  gcc --version
```

安装gcc7.5版本：