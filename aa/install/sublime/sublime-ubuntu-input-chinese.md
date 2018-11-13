# sublime输入中文

## 过程
+ 安装`文泉驿字体`
    * install
    ```
    sudo apt-get install ttf-wqy-zenhei
        # sudo apt-get install xfonts-wqy
    ```
    * config
    `sublime-text-3/Packages/User/Preferences.sublime-settings`增加
    ```
    "font_face": "WenQuanYi Zen Hei Sharp"
    ```

+ 安装`sublime-text-imfix`
    * update
    ```
    sudo apt-get update && sudo apt-get upgrade
    ```
    * download
    ```
    git clone https://github.com/lyfeyaj/sublime-text-imfix.git
    ```
    * install
    ```
    cd sublime-text-imfix && ./sublime-imfix
    ```
    * reboot
    完成! 重新启动后就可以在 Sublime Text 2/3 中 使用 Fcitx了! 注意: 皮肤可能需要自己选择 ^_^

+ 创建桌面图标
  * 新建`sublime.sh`
    ```
    subl
    ```
  * 在桌面新建`Sublime Text`链接
    ```
    cd Desktop/
    ln -s ../sublime.sh Sublime\ Text
    chmod +x ../sublime.sh
    chmod +x Sublime\ Text
    ```
  * 设置双击`sh`直接运行
    nautilus(文件管理器) -> Edit -> Preferences -> Behavior -> Executable Text Files -> `Run executable text files when they are opened`
    鼠标右键点击创建的快捷方式
  * 设置图标
    在图标处，选择自定义图标即可
    图标路径： `/opt/sublime_text/Icon/32x32/sublime-text.png`
    > 2017-10-17 14:26:46

## ref
> https://github.com/lyfeyaj/sublime-text-imfix

## log
+ 2017-06-30 11:33:56
    * create