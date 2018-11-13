# init
安装Ubuntu后，进行初始设置&应用软件安装

## SYSTEM

### show desktop
快捷键
`System Settings - Keyboard - Shortcuts - Navigation - Hide all normal windows`

图标按钮
`System Settings - Appearance - Behavior - Add show desktop icon to the launcher`

### keys
+ show desktop
    * `System Settings - Keyboard - Shortcuts - Navigation - Hide all normal windows`
    * `System Settings - Keyboard - Shortcuts - Windows - Minimize window` ?
    * Super + D

+ maximize window
    * `System Settings - Keyboard - Shortcuts - Windows - maximize window`
    * Super + Up

+ alt
    * `System Settings - Keyboard - Shortcuts -  Launchers - Key to show the HUD`
    * Mod2 + Alt R
        改为右Alt

+ files
    * `System Settings - Keyboard - Shortcuts -  Launchers - Home folder`
    * Super + E

恢复系统默认的快捷键
```
gsettings reset-recursively org.gnome.settings-daemon.plugins.media-keys
gsettings reset-recursively org.compiz.integrated
gsettings reset-recursively org.gnome.desktop.wm.keybindings
gsettings reset-recursively org.gnome.shell.keybindings
```

### grub
+ 修改GRUB2等待时间：
  ```
  sudo vim /etc/default/grub
  ```
  代码:
  `GRUB_TIMEOUT=5`
  将5改为想要的数值，单位秒

修改后必须重新生成GRUB的启动菜单配置文件
```
sudo update-grub2
```
> http://blog.csdn.net/yc1022/article/details/50985078

### date format
任务栏时间格式设置
```
sudo apt-get install dconf-editor
```
1、当 dconf Editor 启动后，导航至 com -> canonical -> indicator -> datetime。
	将 time-format 的值设置为 custom。
2、现在你可以通过编辑 custom-time-format 的值来自定义时间和日期的格式。
```
%Y-%m-%d %H:%M
```
> http://www.techweb.com.cn/network/system/2015-12-25/2247172.shtml

### mount
将work盘挂载到`~/work`目录
+ 查看磁盘分区的UUID和分区类型TYPE
```
jeff@lab:~$ sudo blkid
/dev/sda7: LABEL="work" UUID="000DD4F70004C0AC" TYPE="ntfs" PARTUUID="c35caeed-07"
```
+ 修改/etc/fstab配置文件
```
jeff@lab:~$ sudo gedit /etc/fstab
```
在末尾追加一行：
    ​```
    # mount AA work
    UUID=000DD4F70004C0AC /home/jeff/work            ntfs    defaults              0       0
    ​```
配置文件包含以下几项：
```
<file system> <mount point>   <type>  <options>       <dump>  <pass>
<file system> ：分区定位，可以给磁盘号，UUID或LABEL，例如：/dev/sda2，UUID=6E9ADAC29ADA85CD或LABEL=software
<mount point> : 具体挂载点的位置，例如：/media/C
<type> : 挂载磁盘类型，linux分区一般为ext4，windows分区一般为ntfs
<options> : 挂载参数，一般为defaults
<dump> : 磁盘备份，默认为0，不备份
<pass> : 磁盘检查，默认为0，不检查
```
保存，重启，成功。
*注意：千万不要挂载到当前用户的根目录，因为挂载的分区会覆盖当前分区内容*

> http://blog.csdn.net/up_com/article/details/51264872
> http://www.cnblogs.com/A-Song/archive/2013/02/27/2935255.html
> 2017-06-17 16:54:23

### link
```
cd ~
ln -s work/handbook/ handbook
```
将~/work/handbook/快捷到home目录下

### shell -> file-manager
在终端 用文件管理器 打开当前目录
+ 编辑`.bashrc`
  ```
  vim ~/.bashrc
  ```
+ 新增alias
  ```
  # shell -> file-manager
  alias fm='nautilus "`pwd`"'
  ```
> http://forum.ubuntu.org.cn/viewtopic.php?f=169&t=276453
> 2017-07-03 12:11:23

### startup / Startup Applications Perferences / 开机启动项
按`Win`，搜索`Startup Applications Perferences`，添加
+ Name: sublime
+ Command: subl
> 2018-10-18 11:32:01

### setting
+ 系统设置
    * 将启动器移动到屏幕底部
        ```
        $ gsettings set com.canonical.Unity.Launcher launcher-position Bottom
        ```

    * 禁用访客模式
        ```
        $ sudo vi /etc/lightdm/lightdm.conf
        ```
        ```
        [SeatDefaults]
        greeter-session=unity-greeter
        allow-guest=false
        ```
+ 系统清理与更新
    * 删除Amazon的链接
        ```
        $ sudo apt-get purge unity-webapps-common
        ```

+ 清理旧版本的软件缓存
  ```
  $ sudo apt-get autoclean          # ubuntu 16 可以直接使用apt
  ```
+ 清理所有软件缓存
  ```
  $sudo apt-get clean
  ```
+ 删除系统不再使用的孤立软件
  ```
  $ sudo apt-get autoremove
  ```
> https://zhuanlan.zhihu.com/p/26032793
 2017-06-20 10:56:06

+ 设置vim为默认编辑器
  ```
  sudo update-alternatives --config editor 
  ```
  有4个候选项可用于替换editor，输入`3`，并Enter
  注：`vim.basic`是完全版的vim，`vim.tiny`为精简版不支持高亮等功能。
  update-alternatives 命令还可以配置 FTP、Telnet、rsh 等预设程序。更多可以查看 /etc/alternatives 目录。
  > http://blog.csdn.net/zhangsming/article/details/42460897
  > 2017-07-19 20:24:13

+ 壁纸路径
  /usr/share/backgrounds


### ~/.bashrc
  ```
  # some more ls aliases
  alias ll='ls -alF'
  alias la='ls -A'
  alias l='ls -CF'

  # shell -> file-manager
  alias fm='nautilus "`pwd`"'

  # du --max-depth=1 -h
  alias du.='du --max-depth=1 -h'

  # spring shell completion
  # . /opt/spring-2.0.5.RELEASE/shell-completion/bash/spring

  # find . -type f -name "KEYWORD"
  alias find.='find . -type f -name'
  ```
运行`. ~/.bashrc`生效
> 2018-10-18 10:44:08

### ~/.profile
  ```
  # maven - Add the bin directory to your PATH
  export PATH=/opt/maven/bin:$PATH

  # idea
  export IDEA_JDK_64=/usr/lib/jvm/java-8-oracle

  # gradle
  export PATH=$PATH:/opt/gradle/bin

  # jdk
  #export CLASSPATH=.:$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

  # sublime
  #subl
  ```

> 2018-10-18 10:55:16

## APP

### vim
`vi ~/.vimrc`
```
set number

" 设定光标离窗口上下边界 5 行时窗口自动滚动
set scrolloff=5
" 设定 << 和 >> 命令移动时的宽度为 4
set shiftwidth=4

" 设定 tab 长度为 4
set tabstop=4
" 使得按退格键时可以一次删掉 4 个空格,不足 4 个时删掉所有剩下的空格）
set softtabstop=4
" 设置自动缩进：即每行的缩进值与上一行相等；使用 noautoindent 取消设置
set autoindent
" morning: 亮色主题；evening: 暗色主题
"colorscheme morning
" 显示括号配对，当键入“]”“)”时，高亮度显示匹配的括号
set showmatch
" 处于文本输入方式时加亮按钮条中的模式指示器
set showmode
" 在状态栏显示目前所执行的指令，未完成的指令片段亦会显示出来
set showcmd

" 高亮查找
set hlsearch
" 自动语法高亮
syntax enable
" 开启语法检查
syntax on
```
> 2018-10-18 15:08:13

### input

#### fcitx
+ fcitx-clipboard
  + `Fcitx - Configure - Addon - Clipboard - Trigger Key for Clipboard History List`
  + Ctrl + Alt + ;

+ 配置`QuickPhrase`快捷键
  ```
  vi ~/.config/fcitx/conf/fcitx-quickphrase.config
  ```

  `AlternativeTriggerKey`改为`Alt + Shift + Q`

  ```
  AlternativeTriggerKey=ALT_SHIFT_Q
  ```

+ 时间快捷输入
  新建文件`~/.config/fcitx/lua/date.lua`
  ```
  function LookupDate(input)
    local fmt
    if input == '' then
      fmt = "%Y年%m月%d日"
    elseif input == 't' then
      fmt = "%Y-%m-%d %H:%M:%S"
    elseif input == 'd' then
      fmt = "%Y-%m-%d"
    end
    return os.date(fmt)
  end

  ime.register_command("dt", "LookupDate", "日期时间输入")
  ```
  按`Ctrl + 5`，生效配置。

  * 按`Ctrl + E`，输入`dt`： 2017-06-21 17:01:02
  * 按`Ctrl + E`，输入`dtd`： 2017-06-21
  * 按`Ctrl + E`，输入`dtt`： 17:02:35
  * 按`Ctrl + E`，输入`dtdc`： 2017年06月21日

+ 文本 / 符号 / 自定义表情 快捷输入
  创建`~/.config/fcitx/data/QuickPhrase.mb`
  ```
   #第一个字符为“#”的行是注释
   #格式：编码 符号
   #数学符号
   
   mail xxx@qq.com
   dianhua 123456789
   youbian 123456
   dizhi 中华人民共和国北京市长安街一号
   aowu ┗<(=｀O′=)>┛ 
   mobai ｍ<(＿　＿)>ｍ 
   baobao <(=′▽')爻 (`▽｀=)> 
   baobao <(=*′д｀)爻(′д｀*=)> 
   qiangbi ▄︻┻┳═一…… ☆<(=￣□￣=!)>
  ```
  按`Ctrl + 5`，生效配置

  > https://git.ustclug.org/farseer2013/dotfiles/blob/ae5d240c43c2a6e233103497c59065cc45b12fc7/.config/fcitx/conf/fcitx-quickphrase.config
  > https://github.com/fcitx/fcitx/issues/14
  > https://blog.lilydjwg.me/2012/10/7/fcitx-lua-plugin-input-ipa-symbols.35866.html
  > https://github.com/lilydjwg/fcitx-lua-scripts/blob/master/date.lua
  > https://wiki.archlinux.org/index.php/Fcitx_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)
  > 2017-06-21 17:25:09

#### *wubi (忽略，用下面的rime)*
    Ctrl+Alt+T打开终端安装Fcitx拼音和五笔输入法: 
    ​```
    sudo apt-get install fcitx
    sudo apt-get install fcitx-table-wubi
    ​```
    Alt+F2运行 im-config 设置Fcitx为默认输入法
    > https://my.oschina.net/eechen/blog/224291
    
    在系统设置->语言支持菜单中，将输入法设置“fcitx”，如果想使用回ibus，此处改回ibus
    http://bluexp29.blog.163.com/blog/static/33858148201062981256667/


#### rime
+ 安装
```
sudo apt-get install fcitx-rime
```
然后重启或者注销一次，
System Settings - Text Entry - `+` - Rime(Fcitx) / 在 设置 -› 文本输入 里面添加 “中州韵(Fcitx)” 就可以使用了。
但是默认是没有五笔输入法的需要安装一下

+ 添加五笔
```
sudo apt-get install librime-data-wubi

cp /usr/share/rime-data/wubi86.schema.yaml ~/.config/fcitx/rime/
cp /usr/share/rime-data/wubi_pinyin.schema.yaml ~/.config/fcitx/rime/
```
以上无用，再用以下
```
cp /usr/share/rime-data/wubi86.schema.yaml ~/.config/fcitx/rime/
cp /usr/share/rime-data/wubi_pinyin.schema.yaml ~/.config/fcitx/rime/
ln -s /usr/share/rime-data/wubi86.dict.yaml ~/.config/fcitx/rime/
ln -s /usr/share/rime-data/wubi_pinyin.prism.bin ~/.config/fcitx/rime/
ln -s /usr/share/rime-data/pinyin_simp.dict.yaml ~/.config/fcitx/rime/
ln -s /usr/share/rime-data/pinyin_simp.* ~/.config/fcitx/rime/
ln -s /usr/share/rime-data/wubi86.* ~/.config/fcitx/rime/
```

这样就安装完成了，编辑~/.config/fcitx/rime/default.yaml
```
gedit ~/.config/fcitx/rime/default.yaml
```
在schema_list下方加入：
```
- schema: wubi86
- schema: wubi_pinyin
```
然后重新部署输入法，按F4就可以切换了。 五笔按z可以反查。
> http://goenitz.xyz/post/ubuntu-install-fcitx-rime-and-wubi
> http://www.tuicool.com/articles/r2eUF3

+ 取消Shift切换中英文功能
  * `~/.config/fcitx/rimedefault.yaml`
    ```
    ascii_composer:
      good_old_caps_lock: true
      switch_key:
        Shift_L: noop
        Shift_R: commit_code
    ```
    找到default.yaml把里面shift_L和Shift_R后面的内容改成noop。重启fcitx生效
  * 建议
    - 在default.custom.yaml加上 patch: "ascii_composer/switch_key/Shift_L": noop 然后重新布署 注意空格不能多也不能少
    - 直接改default.yaml，版本升级后自己之前的改动会被覆盖，推荐通过default.custom.yaml打补丁
> http://tieba.baidu.com/p/2752159880
> http://blog.csdn.net/endlch/article/details/44538755
> 2017-07-09 16:34:35

+ 取消F4热键
  * `~/.config/fcitx/rimedefault.yaml`
    ```
    switcher:
      caption: 〔方案選單〕
      hotkeys:
        * Control+grave
        * Control+Shift+grave
        * F4
    ```
    删除`F4`。保留Control+grave，以后用`Ctrl` + ```来切换
    > https://zhidao.baidu.com/question/38976883.html
    > 2017-07-09 17:13:49

### java / jdk
+ 编译open-jdk（搁置）
  * 版本选择
    > https://www.zhihu.com/question/60786248/answer/180169329
    > 2017-07-02 10:56:05
  * 源码下载
    > https://www.zhihu.com/question/41531114
    > 2017-07-02 14:20:31
  * 编译
    > http://www.concurrent.work/2014/04/26/compile-openjdk8-from-source-code/
    > http://www.voidcn.com/blog/pcsxk/article/p-6385331.html
    > 2017-07-02 15:03:03
  * 搁置
    搁置编译，选择安装JDK
    鉴于以下：
    - 暂时没深入的学习源码/JVM计划，为了后续JVM基础知识/数据结构与算法/Spring的学习进展，暂时搁置处理
    - 难度有点大，安装/调试等环境搭建挺费时
    - 没找到源码包，需要安装Mercurial代码版本工具
    > 2017-07-02 15:12:33

+ 安装jdk
  * 自动 (使用ppa/源方式安装) （推荐）
    推荐，因为可以通过 apt-get upgrade 方式方便获得jdk的升级
    - 添加ppa
      ```
      sudo add-apt-repository ppa:webupd8team/java
      sudo apt-get update
      ```

    - 安装oracle-java-installer
      ```
      sudo apt-get install oracle-java8-installer
      ```
      安装器会提示你同意 oracle 的服务条款,选择 ok；如果你懒,不想自己手动点击.也可以加入下面的这条命令,默认同意条款:
      `echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections`

    - 设置系统默认jdk
      ```
      sudo update-java-alternatives -s java-8-oracle
      ```

    - 测试jdk 是是否安装成功:
      ```
      java -version
      javac -version
      ```
    > http://www.cnblogs.com/a2211009/p/4265225.html
    > 2017-07-02 16:00:03

  * 手动 (下载jdk压缩包方式安装)
    下载`jdk-8u131-linux-x64.tar.gz`
    > http://www.cnblogs.com/a2211009/p/4265225.html
    > http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
    > http://blog.csdn.net/oh_mourinho/article/details/52691398
    > http://www.jianshu.com/p/8712cf20a5c9
    > 2017-07-02 15:29:18

### maven
+ 下载`apache-maven-3.5.0-bin.tar.gz`
+ Ensure JAVA_HOME environment variable is set and points to your JDK installation
  ```
  echo $JAVA_HOME
  ```
+ 解压 Extract distribution archive in any directory
  ```
  sudo tar -zxvf apache-maven-3.5.0-bin.tar.gz -C /opt/
  cd /opt
  sudo mv apache-maven-3.5.0/ maven
  ```
+ Add the bin directory to your PATH
  ```
  vim ~/.profile
  ```
  `~/.profile`追加以下：
  ```
  # maven - Add the bin directory to your PATH
  export PATH=/opt/maven/bin:$PATH
  ```
+ 生效配置
  ```
  . .profile
  ```
+ ~~设置本地仓库(过时)~~
  ```
  sudo vim /opt/maven/conf/settings.xml
  ```
  `settings.xml `设置：
  ```
  <localRepository>${user.home}/dev/maven/repo</localRepository>
  ```
+ 验证
  ```
  jeff@lab:~$ mvn -v
  Apache Maven 3.5.0 (ff8f5e7444045639af65f6095c62210b5713f426; 2017-04-04T03:39:06+08:00)
  Maven home: /opt/maven
  Java version: 1.8.0_131, vendor: Oracle Corporation
  Java home: /usr/lib/jvm/java-8-oracle/jre
  Default locale: en_US, platform encoding: UTF-8
  OS name: "linux", version: "4.8.0-58-generic", arch: "amd64", family: "unix"
  ```
> http://zyjustin9.iteye.com/blog/2177979
> http://maven.apache.org/install.html
> https://my.oschina.net/hongdengyan/blog/150472
> 2017-07-04 17:10:42

### ide - IDEA
IDEA
+ 下载`ideaIU-2017.1.4-no-jdk.tar.gz`
+ 解压
  ```
  sudo tar -zxvf ideaIU-2017.1.4-no-jdk.tar.gz -C /opt
  ```
+ 运行
  cd into "{installation home}/bin" and type: `./idea.sh`
  ```
  jeff@lab:/opt/idea$ ./bin/idea.sh 
  ```
+ setting
  * 字体 - 颜色
    `Setting -> Editor -> Colors & Fonts -> General -> Text -> Default text`
    - Foreground: #CCCCCC
    - Background: #000000
  * maven
    `Setting -> Build, Execution, Deployment -> Build Tools -> Maven`
    - Maven home directory: /opt/maven
    - User setting file: /opt/maven/conf/settings.xml

> https://www.jetbrains.com/idea/download
> http://blog.csdn.net/qinkang1993/article/details/54631606
> 2017-07-02 17:00:06
> http://blog.csdn.net/shizhebsys/article/details/24591127
> 2017-07-17 11:53:45

### sublime
参见`handbook/install/sublime/sublime-install.md`

### wiz
1. 
  https://www.idaima.com/a/14300.html
  https://tieba.baidu.com/p/5121193231
  http://www.cnblogs.com/zhuandshao/p/6880809.html

2.
sudo add-apt-repository ppa:wiznote-team
sudo apt-get update
sudo apt-get install wiznote  (如果这一步出错,请自觉翻墙,个人推荐lantern)

3. compile
  http://blog.csdn.net/qq_33429968/article/details/65634176
  https://github.com/WizTeam/WizQTClient

### browser - vivaldi
https://vivaldi.com/download/

+ Search
  * https://www.google.com.hk/#q=%s
  * https://www.baidu.com/s?wd=%s
  * https://www.zhihu.com/search?q=%s
  * http://dict.cn/%s
+ Extensions
  * Vimium
+ 取消keyring弹出提示
  加上启动参数`--password-store=basic`
  ```
  jeff@lab:~/Desktop$ dpkg -l|grep vivaldi
  ii  vivaldi-stable                              1.10.867.42-1                                 amd64        A new browser for our friends

  jeff@lab:~/Desktop$ dpkg-query --listfiles vivaldi-stable|grep "\.desktop"
  /usr/share/xfce4/helpers/vivaldi.desktop
  /usr/share/applications/vivaldi-stable.desktop

  jeff@lab:~/Desktop$ sudo vim /usr/share/applications/vivaldi-stable.desktop
  ```
  将`Exec=/usr/bin/vivaldi-stable %U` 改为 `Exec=/usr/bin/vivaldi-stable %U --password-store=basic`
  > https://xerocrypt.wordpress.com/2017/04/13/how-to-disable-the-vivaldi-browser-keyring-request/
  > 2017-07-12 09:39:53

### browser - firefox
+ Search
  Preference - Search - Add more search engines...
  * Google NCR
+ Extensions
  * Vimperator / Vimium
  * FireGestures
  * VivaldiFox
+ Appearance
  * Compact Dark
+ config
  * browser.sessionhistory.max_total_viewer
    在搜索栏输入 browser.sessionhistory.max_total_viewer ，查询一下，双击并赋值为10。
    解释：火狐为了加速网页浏览速度增加了网页的快进快退功能。默认是保存8个网页信息，很显然这增加了内存的负荷，如果想降低内存的占用，可以将默认值“-1”设为“0”，但是这样做会在一定程度上影响你的浏览体验。browser.sessionhistory.max_total_viewers 赋值为 10。（“-1”的意思是无限，对一般用户来说设置为10差不多了）
+ Performance
  * about:memory
    页面可以使您方便的发现由网页、扩展或者主题引发内存占用问题，有时页面上的 最小化内存占用 按钮可以使您立即减少 Firefox 的内存占用。关于about:memory的使用指导，参阅about:memory。 



### browser - Extensions - Vimium(chrome) / VimFx(firefox)
+ download
  * https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb
  * https://addons.mozilla.org/en-GB/firefox/addon/vimium-ff/?src=search
+ setting
  - Show Advanced Options - Default Search engine
    + https://www.google.com.hk/search?q=
  - Custom search engines
    ```
    g: https://www.google.com.hk/#q=%s google
    b: https://www.baidu.com/s?wd=%s baidu
    z: https://www.zhihu.com/search?q=%s zhihu
    d: http://dict.cn/%s dict
    w: http://www.wikipedia.org/w/index.php?title=Special:Search&search=%s wiki
    a: https://www.amazon.cn/s/keywords=%s Amazon
    ```
  - Characters used for link hints
    + 默认
      * vimium
        - sadfjklewcmpgh
      * VimFx
        - fjdkslaghrueiwonc mv
          不太理解空格的作用，mv又有啥特别用意呢[TODO]
    + 左手键盘位
      * qwerasdfzxcvtgb
    + 双手中间键盘位
      * asldkfjgh
  - Custom key mappings
    + vimium
      ```
      # Insert your preferred key mappings here.
      map gp togglePinTab
      map mp toggleMuteTab
      unmap <a-p>
      unmap <a-m>
      ```
    + VimFx
      * Tabs
        - Move tab left
          + `<<`
        - Move tab right
          + `>>`
  - 更改hints提示颜色
    + vimium
      Vimium Options - Advanced Options - CSS for link hints
      ```
      div > .vimiumHintMarker span {
        /* linkhint text */
        color: white;
        /*font-family: "monospace";*/
        font-family: "YaHei Consolas Hybrid";
        font-size: 14px;
        padding-left: 2px;
      }

      div.internalVimiumHintMarker {
        background: black !important;
        border: 1px dashed white;
      }
      ```
      > 2017-07-05 15:40:03
    + vimfx
      ```
      @namespace url(http://www.mozilla.org/keymaster/gatekeeper/there.is.only.xul);

      /*
        marker-border： 浅 #FFF8E1 / #FAFAFA / #E0F7FA / #E8F5E9
        marker-background: [page.background.color]
        char-color: [page.background.color]
        char-background: 深 #FFF59D / #EEEEEE / #80DEEA / #A5D6A7
        配色参考： 扁平化设计颜色表
        > http://htmlcolorcodes.com/zh/yanse-biao/
        > 2017-07-06 23:16:17
      */

      #VimFxMarkersContainer .marker {
        /*提示*/
        background: black !important;
        border: 1px dashed #FFF8E1 !important;
        padding: 0.5px 1px !important;
        font-size: 14px !important; /* Specific font size. */
        opacity: 0.8 !important; /* Semi-transparent. Warning: Might be slow! */
      }

      #VimFxMarkersContainer .marker-char {
        /*提示字母*/
        background-color: #FFF59D !important;
        margin: 1px;
        padding: 1px 3px !important;
      }

      #VimFxMarkersContainer .marker-char--matched {
        /*按下后匹配的提示字母: 交换前景/背景色*/
        color: #FFF59D !important;
        background-color: black !important;;
      }
      ```
      > https://github.com/akhodakivskiy/VimFx/blob/master/documentation/styling.md
      > 2017-07-06 23:25:20

### Geary mail
点击`Ubuntu Software`，搜索`Geary`，进行安装
> 2018-06-03 15:48:04

Evolution
邮件客户端，没试用
> 2018-06-03 16:35:41

### total commander


### git
```
sudo apt-get install git
```
> 2017-06-29 17:54:57

### compare - deff
+ deff

```
sudo apt-get install meld
```
> http://os.51cto.com/art/201607/513796.htm
> 2017-06-26 10:31:59

### markdown - typora
+ typora
    * install
    ```
    # optional, but recommended
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
    # add Typora's repository
    sudo add-apt-repository 'deb https://typora.io ./linux/'
    sudo apt-get update
    # install typora
    sudo apt-get install typora
    ```
    * style
      - `File->Preferences->Open Theme Folder`
      - 复制一份`github.css`文件为`github(font).css`，并修改：
        + 修改`字体`
            在`body - font-family`加`WenQuanYi Zen Hei Sharp`，并设为第一项
            ```
            body {
                font-family: "WenQuanYi Zen Hei Sharp","Open Sans","Clear Sans","Helvetica Neue",Helvetica,Arial,sans-serif;
                color: rgb(51, 51, 51);
                line-height: 1.6;
            }
            ```
        + 修改`编辑界面最大宽度`
            在`#write - max-width`设置宽度值
            ```
            #write{
                max-width: 88%;
                margin: 0 auto;
                padding: 20px 30px 40px 30px;
                padding-top: 20px;
                padding-bottom: 100px;
            }
            ```
      - 将菜单`Theme`勾选设置为`github(copy)` 
      > http://www.jianshu.com/p/5634b207d516
      > http://www.jianshu.com/p/e1baaea15458
      > 2017-10-15 17:49:37

> https://typora.io
> 2017-06-20 10:59:55

### dict - goldendict / sdcv
goldendict

```
sudo apt-get install goldendict
```
> http://blog.csdn.net/jubincn/article/details/6094702
> 2017-06-28 18:22:05

+ sdcv (可选)
  * 安装sdcv
  ```
  sudo apt-get install sdcv
  ```
  * 安装字典
    - 下载字典
      + 简体中文: http://download.huzheng.org/zh_CN/
      + 繁体中文: http://download.huzheng.org/zh_TW/
      ```
      $ cd /tmp
      朗道英汉字典
      $ wget http://download.huzheng.org/zh_CN/stardict-langdao-ec-gb-2.4.2.tar.bz2
      朗道汉英字典
      $ wget http://download.huzheng.org/zh_CN/stardict-langdao-ce-gb-2.4.2.tar.bz2
      ```
    - 安装字典
      ```
      $ mkdir -p ~/.stardict/dic
      $ cd ~/.stardict/dic
      $ tar xvf /tmp/stardict-langdao-ec-gb-2.4.2.tar.bz2
      $ tar xvf /tmp/stardict-langdao-ce-gb-2.4.2.tar.bz2
      ```
  * 使用
    - 查看字典字库数量
      ```
      $ sdcv -l
      Dictionary's name   Word count
      朗道汉英字典5.0    405719
      朗道英汉字典5.0    435468
      ```
    - 单字查询
      ```
      $ sdcv hello
      发现 1 条记录和 hello 相似。
      -->朗道英汉字典5.0
      -->hello

      *[hә'lәu]
      interj. 喂, 嘿
      ```
    - 多重查询 (进入无限查询状态)，使用 Ctrl + C 或 D 离开
    - 观看历史查询记录
      ```
      $ cat $HOME/.sdcv_history|tail
      clear
      clean
      hello
      hello
      ```
  > https://chusiang.gitbooks.io/working-on-gnu-linux/content/15.sdcv.html
  > 2017-06-28 19:53:06
  > http://www.10tiao.com/html/357/201605/2247483855/1.html
  > 2017-07-10 18:49:18

### pdf / djvu - zathura
+ zathura
  + install
    ```
    sudo apt-get install zathura
    ```

  + setting
    ```
    vi .config/zathura/zathurarc
    ```
    .config/zathura/zathurarc:
    ```
    set recolor true
    set recolor-keephue true
    set font "WenQuanYi Zen Hei Sharp normal 13"
    # hide status bar
    set guioptions ""
    # copy to clipboard
    set selection-clipboard clipboard
    ```
  + [http://pwmt.org/projects/zathura/download/](http://pwmt.org/projects/zathura/download/)

+ apvlv
  * http://naihe2010.github.com/apvlv/

> http://80x86.io/post/pdf-viewer-for-linuxer
> 2017-06-28 15:22:56

### doc - wps
+ 卸载LibreOffice
  ```
  sudo apt-get remove --purge libreoffice*
  sudo apt-get clean
  sudo apt-get autoremove
  ```
+ 安装wps
  ```
  sudo dpkg -i Downloads/wps-office_10.1.0.5672~a21_amd64.deb
  ```
+ 安装字体
  * 下载 wps_symbol_fonts.zip
    - https://github.com/Sachs27/dotfiles/tree/master/.fonts/wps_symbol_fonts
  * 安装
    解压后将整个wps_symbol_fonts目录拷贝到 /usr/share/fonts/ 目录下
    注意，wps_symbol_fonts目录要有可读可执行权限`chmod 755 /usr/share/fonts/wps_symbol_fonts`
    ```
    sudo mkdir -p /usr/share/fonts/wps_symbol_fonts
    sudo mv Downloads/*.ttf /usr/share/fonts/wps_symbol_fonts
    sudo chmod 755 /usr/share/fonts/wps_symbol_fonts
    ```
+ 无法输入中文问题
  原因：环境变量未正确设置，以上可以直接针对wps设置。
  * wps文档
    打开终端输入：
    ```
    $ sudo gedit /usr/bin/wps 
    ```
    添加以下内容：
    ```
    export XMODIFIERS="@im=fcitx"
    export QT_IM_MODULE="fcitx"
    ```
    修改后如下：
    ```
    #!/bin/bash
    **export XMODIFIERS="@im=fcitx"**
    **export QT_IM_MODULE="fcitx"**
    gOpt=
    #gOptExt=-multiply
    gTemplateExt=("wpt" "dot" "dotx")
    ...
    ```
  * wps表格
    ```
    $ sudo gedit /usr/bin/et 
    ```
    修改后如下：
    ```
    #!/bin/bash
    export XMODIFIERS="@im=fcitx"
    export QT_IM_MODULE="fcitx"
    gOpt=
    #gOptExt=-multiply
    ...
    ```
  * wps演示
    ```
    $ sudo gedit /usr/bin/wpp 
    ```
    修改后如下：
    ```
    #!/bin/bash
    export XMODIFIERS="@im=fcitx"
    export QT_IM_MODULE="fcitx"
    gOpt=
    #gOptExt=-multiply
    ...
    ```
    修改完后保存，在打开相应的程序切换输入法就可以输入中文了。
    > http://forum.ubuntu.org.cn/viewtopic.php?f=48&t=476937
    > 2017-07-12 10:52:55

+ 创建桌面快捷图标
  ```
  dpkg-query --listfiles wps-office | grep "\.desktop"
  cp /usr/share/applications/wps-office-*.desktop ~/Desktop/
  chmod +x ~/Desktop/wps-office-*.desktop
  ```
 
+ 卸载
  ```
  jeff@lab:~$ dpkg -l|grep wps
  ii  libwps-0.4-4:amd64                          0.4.3-1ubuntu1                                amd64        Works text file format import filter library (shared library)
  ii  wps-office                                  10.1.0.5672~a21                               amd64        WPS Office, is an office productivity suite.
  jeff@lab:~$ sudo dpkg -r wps-office
  ```
  使用dpkg -l可以列出所有已经安装的软件包名；使用dpkg -l|grep wps列出与wps相关的包名
  安装某个deb包
  sudo dpkg -i xxx.deb
  卸载
  sudo dpkg -r 包名

> https://www.zhihu.com/question/19811112
> 2017-06-27 18:44:13
> https://blog.huzhifeng.com/2017/01/15/WPS/
> http://os.51cto.com/art/201402/430505.htm
> 2017-07-11 16:14:45
> https://my.oschina.net/uniquejava/blog/101440
> 2017-07-11 22:39:28

### mindmap - xmind
由于安装到`/opt/xmind`，整个目录都归属`root`，从而导致没在`root`组的当前用户`jeff`无法正常使用
再重装到`/home/jeff/app/application/xmind`

+ 安装JDK
+ 下载`xmind-8-update2-linux.zip`
+ 解压
  ```
  sudo mkdir /home/jeff/app/application/xmind
  sudo unzip xmind-8-update2-linux.zip -d /home/jeff/app/application/xmind
  ```
+ 安装
  ```
  sudo ./setup.sh 
  ```
+ 运行
+ 关联`xmind`文件 (没有成功)
  ```
  sudo vim /usr/share/applications/xmind.desktop
  ```
  `xmind.desktop`
  ```
  Name=xmind
  Exec=/home/jeff/app/application/xmind/XMind_amd64/XMind %f
  ```
  没有成功
+ 修改默认字体 (没有成功)
  ```
  cd ./plugins/org.xmind.ui.resources_3.7.2.201705011955/styles
  cp defaultStyles.xml defaultStyles.xml.20170704
  vim defaultStyles.xml
  ```
  ```
  :%s/\$system\$/WenQuanYi Zen Hei Sharp
  ```
  没有成功
+ 创建桌面图标
  * 复制可运行的`sublime-text.desktop`，并用`vim`打开
  * 修改以下：
    ```
    Exec=/home/jeff/shell/app/xmind.sh
    Icon=/home/jeff/app/application/xmind/XMind_amd64/configuration/org.eclipse.osgi/983/0/.cp/icons/xmind.64.png
    ```
  > 2018-07-05 20:54:32

+ 创建桌面图标 (没有成功)
  * 按`super`，输入`xmind`，将图标拖至桌面
  * 编辑`xmind.desktop`，将默认的 `/opt/xmind/XMind_amd64` 更改为 `/home/jeff/app/application/xmind/XMind_amd64`
+ 创建桌面图标（过时，太复杂）
  * 新建`xmind.sh`
    ```
    cd ~/app/application/xmind/XMind_amd64
    ./XMind        
    ```
  * 在桌面新建`xmind.sh`链接
    ```
    cd Desktop/
    ln -s ../xmind.sh xmind
    chmod +x ../xmind.sh
    chmod +x xmind
    ```
  * 设置双击`sh`直接运行
    nautilus(文件管理器) -> Edit -> Preferences -> Behavior -> Executable Text Files -> `Run executable text files when they are opened`
    鼠标右键点击创建的快捷方式，在图标处，选择自定义图标即可

> http://www.xmind.net/download/linux/
> 2017-07-03 11:31:45
> https://blog.felixc.at/2011/08/linux-xmind-double-click-open/
> 2017-07-03 17:54:39
> https://segmentfault.com/a/1190000000451226
> 2017-07-03 18:17:31
> https://my.oschina.net/OriginLeon/blog/820718
> 2017-07-14 10:03:10

### font
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

+ YaHei Consolas Hybrid

```
    Ubuntu下字体:
        Ubuntu Mono
        Dejavu Sans Mono

    Windows下字体
        文泉驿的Unibit
        courier new
        YaHei Consolas Hybrid
        monaco
        YaHei Consolas Hybrid 11
        Menlo
```
> http://blog.leanote.com/post/mynote/Ubuntu%E5%AD%97%E4%BD%93%E5%AE%89%E8%A3%85

### flash
+ 下载`flash_player_ppapi_linux.x86_64.tar.gz`
+ 解压
  ```
  sudo mkdir /usr/lib/adobe-flashplugin/
  sudo tar -zxvf flash_player_ppapi_linux.x86_64.tar.gz -C /usr/lib/adobe-flashplugin/
  ```
  安装，参见`readme.txt`
> https://get.adobe.com/flashplayer
> 2017-07-03 10:47:39

### chm - xCHM
+ xCHM
  ```
  sudo apt-get install xchm
  ```
  > https://www.linuxdashen.com/view-chm-files-xchm-kchmviewer-ubuntu
  > http://blog.sina.com.cn/s/blog_803527e7010145po.html
  > 2017-07-06 11:36:27
+ kchmviewer
  kchmViewer是KDE桌面下的一款CHM文件阅读器
  ```
  sudo apt-get install kchmviewer
  ```
  > http://blog.csdn.net/tao_627/article/details/46891009
  > 2017-07-06 11:31:39

### f.lux
+ 添加PPA
  ```
  sudo add-apt-repository ppa:nathan-renniewaldock/flux
  ```
+ 安装fluxgui
  ```
  sudo apt-get update
  sudo apt-get install fluxgui
  ```
+ How to use f.lux?
  * 在Ubuntu unity 中搜索打开
  * 输入经纬度
    - longitude
      113.862751
    - latitude
      22.581116
  * 勾选Autostart f.lux indicator applet」自动启动
  * 接下来就可以从下拉表中选则要自动切换的色温。
> http://blog.topspeedsnail.com/archives/4691
> http://blog.csdn.net/qq_35523593/article/details/63253547
> 2017-07-03 18:59:27

### uGet
```
sudo add-apt-repository ppa:plushuang-tw/uget-stable
sudo apt update
sudo apt install uget
```
> http://ugetdm.com/downloads-ubuntu
> 2017-08-08 17:07:09

### FileZilla
```
sudo apt-get install filezilla
```
> http://www.linuxidc.com/Linux/2015-02/112996.htm
> 2017-10-17 15:31:22

### util - color - gpick
```
sudo apt-get install gpick
```
> http://zy-email1991.iteye.com/blog/2194079
> 2017-10-17 16:40:11

### shadowsocks-qt5
+ shadowsocks-qt5
  ```
  sudo add-apt-repository ppa:hzwhuang/ss-qt5
  sudo apt-get update
  sudo apt-get install shadowsocks-qt5
  ```
+ 浏览器代理
  ```
  /usr/bin/vivaldi-stable --proxy-server=127.0.0.1:4848
  /usr/bin/vivaldi-stable %U --password-store=basic --proxy-server=127.0.0.1:4848
  ```
> http://blog.heyikan.cn/2017/02/19/Linux%EF%BC%88Ubuntu%EF%BC%89%E4%B8%8A%E7%BF%BB%E5%A2%99%E6%96%B9%E6%A1%88/
> https://vivaldi.club/d/704
> 2018-01-06 19:39:47

### wxHexEditor / gHex
```
sudo apt-get install wxhexeditor 
```
> http://xmodulo.com/good-hex-editor-linux.html
> 2018-03-12 15:43:06

```
sudo apt-get install ghex
```
> https://wiki.gnome.org/Apps/Ghex
> 2018-03-12 15:46:35

### shutter
截图
```
sudo apt-get install shutter
```

### gimp
图片处理
```
sudo apt-get install gimp
```
> https://www.zhihu.com/question/19811112/answer/132006027
> 2018-03-12 16:21:45


### FBReader
+ add the following lines to your /etc/apt/sources.list:
  ```
  sudo gedit /etc/apt/sources.list
  ```
  添加：
  ```
  deb http://fbreader.org/desktop/debian stable main
  deb-src http://fbreader.org/desktop/debian stable main
  ```
+ as root, run ‘apt-get update’.
  ```
  sudo apt-get update
  ```
+ as root, run ‘apt-get install fbreader’.
  ```
  sudo apt-get install fbreader
  ```
> https://fbreader.org/content/linux
> 2018-09-09 16:16:43

### rar / 解压rar
+ 解压
  安装
  ```
  sudo apt-get install unrar
  ```
  使用
  * 可以直接在UI界面使用了
  * rar x test.rar

+ 压缩
  安装
  ```
  sudo apt-get install rar
  ```
  使用
  ```
  rar a FileName.rar DirName
  ```
  添加文件到操作文档
  1. rar a test.rar file1.txt
    若test.rar文件不存在，则打包file1.txt文件成test.rar
  2. rar a test.rar file2.txt
    若test.rar文件已经存在，则添加file2.txt文件到test.rar中(这样test.rar中就有两个文件了）
  注:
  * 如果操作文档中已有某文件的一份拷贝，则a命令更新该文件
  * 对目录也可以进行操作，例: rar a test.rar dir1
> https://blog.csdn.net/jason_cuijiahui/article/details/79860466
> https://www.linuxidc.com/Linux/2017-03/141564.htm
> 2018-10-17 20:25:42



## 故障

### 屏幕超时关闭后不能唤醒 - 失败
+ 修改配置文件`/etc/laptop-mode/laptop-mode.conf`
sudo vim /etc/laptop-mode/laptop-mode.conf 

> https://www.cnblogs.com/xy123001/p/6004133.html
> 2017-07-25 09:55:57

### 关闭屏幕阅读功能
`System Setting -> Universal Access -> Seeing -> Screen Reader`，改为`OFF`
> https://www.cnblogs.com/EasonJim/p/8266735.html
> 2018-01-30 17:22:59

## 其他
sudo apt-get -f install   // 尝试修正安装过程中出现的依赖性关系

ubuntu 9.04不太稳定，经常完全死掉，上述办法往往没用，可采用下面办法：
    同时按下左 Alt 键、SysRq 键(与 PrintScreen在一个键上)和一个字母键
    这些键要起作用，好像要在编译内核时启用该功能，ubuntu 9.04 的内核有这功能。
（1）Alt-SysRq-R，然后Ctrl-Alt-Backspace，如果无效，则依次采用如下步骤
（2）Alt-SysRq-S   保存
（3）Alt-SysRq-E   终止所有进程
（4）Alt-SysRq-I    杀死尚未终止的进程
（5）Alt-SysRq-U   umount
（6）Alt-SysRq-B   reboot，O 便是关机
> http://www.cppblog.com/klion/archive/2010/08/12/123139.aspx
> 2017-07-25 10:06:00

## 参考





