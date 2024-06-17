# 设置 gcc 优先级
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 9
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 1

# 设置 g++ 优先级
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 9
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 1
- 查看优先级

```bash
# 查看 gcc 优先级
sudo update-alternatives --display gcc

# 查看 g++ 优先级
sudo update-alternatives --display g++
```