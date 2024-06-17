https://blog.csdn.net/Williamcsj/article/details/123514435?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171841615716800178577241%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=171841615716800178577241&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-123514435-null-null.142^v100^pc_search_result_base3&utm_term=cudnn%E5%8D%B8%E8%BD%BD&spm=1018.2226.3001.4187

## 一、卸载cuda
cd到 /usr/local/cuda-xx/bin下，执行
```
sudo ./cuda-uninstaller
```

选择done，提示Successfully uninstalled 完成卸载
## 二、卸载cudnn
### 使用ｄｅｂ安装的
查询：
```
sudo dpkg -l | grep cudnn
```



将其全部卸载：
```
sudo dpkg -r libcudnn8-samples
sudo dpkg -r libcudnn8-dev
sudo dpkg -r libcudnn8
```
### 使用ｔｇｚ安装的
`sudo` `rm` `-rf` `/usr/local/cuda/include/cudnn``.h`

`sudo` `rm` `-rf` `/usr/local/cuda/lib64/libcudnn``*`
检查：
输入下面指令后，没有任何输出即卸载成功。
```
sudo dpkg -l | grep cudnn
```
