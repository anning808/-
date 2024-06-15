基于该文章主要是一些[经验总结](https://so.csdn.net/so/search?q=%E7%BB%8F%E9%AA%8C%E6%80%BB%E7%BB%93&spm=1001.2101.3001.7020)（如果能算经验的话....）;

1、前言：

       硬件详情：笔记本电脑（rtx4060）+外接显示器

        安装完ubuntu20后，外接[显示屏] 发现啥也没发生;输入nvidia-smi没反应
        安装最新驱动：

        一开始是# 卸载驱动
        

    # 卸载驱动
    sudo apt-get --purge remove nvidia*
    sudo apt autoremove
    sudo apt-get --purge remove "*nvidia*"