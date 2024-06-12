

本文安装谷歌输入法。
其实之前一直用的是搜狗输入法，因为20.04取消qt4了没装成，就去尝试别的输入法了。发现谷歌输入法用起来极舒服，比sougou for linux好用多了。记得谷歌的中文输入法主要是北京分部在做，对google cn的好感度飙升！！！

文章目录

    ubuntu 20.04中文输入法安装
        安装fcitx-googlepinyin
        配置language support
        输入法配置

安装fcitx-googlepinyin

Ctrl+Alt+T打开终端，输入

``` sudo apt-get install fcitx-googlepinyin```

输入密码开始安装（输入密码的时候光标是不会移动的，不会有对应密码的***这样的星号出来，只管输完密码按回车就行），命令行会停在[y/n]的确认行，输入y并回车后开始安装。
![[Pasted image 20240612142330.png]]

安装完成后打开菜单栏（按键盘上ctrl和alt之间的那个键，就是windows里的win键，在ubuntu里叫super），键盘输入language support并打开。
第一次打开会显示语言支持没有完全安装，点击安装并输入密码开始安装。
安装好后就能进入语言支持界面，最下面一行Keyboard input method system，默认是iBus，点击下拉单切换到fcitx（系统初始没有fctix，安装fcitx-googlepinyin的时候会装好fcitx）。然后重启电脑。
![[Pasted image 20240612142417.png]]

重启之后在右上角状态栏点击键盘图标，在下拉单里选择倒数第三个Configure进入配置界面。
![[Pasted image 20240612142433.png]]
点击输入方法设置左下角的+号，进入添加输入方法界面。取消“只显示当前语言”选项的勾选，输入pinyin搜索到系统现有的拼音输入法。选择Google Pinyin并点击OK确认。
![[Pasted image 20240612142547.png]]
关闭设置，谷歌输入法配置完成。可以点击右上角状态栏的键盘图片切换到谷歌输入法，切换输入法的快捷键是ctrl+space，可以在刚关闭的输入方法设置界面里第二项Global Config里修改快捷键。