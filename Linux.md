## Linux：是个多用户、多任务的支持远程操作的系统

#### 临时添加全局变量（重启失效）

`echo $PATH`   查看全局变量位置

`export PATH=$PATH`:(+你的全局变量目录)

`ctrl + Z`    放到后台执行

`fg %1`    切换回程序

`python3.6   -m venv  .`    (在当前目录开启虚拟环境)

`source  ./bin/activate`      (进入虚拟环境)

`deactivate`     (退出虚拟环境)

#### 永久添加全局变量

`ln -s`  目的命令  添加后的命令     (做软连接)

### 浏览器的功能（内核）：

- 显示内容        （解析内容和样式  -webkit-）
- 实现交互逻辑       （v8引擎---解析JS引擎）
- 进行数据传递         （Chrome  net  引擎）

### 关于Ubuntu：

> 基于Linux内核的最受欢迎的发行版

#### 优点：永久免费、开源分享

#### 目录结构：

只有一个根目录

`shift+Ctrl++`  放大字体

`Ctrl+- `  缩小字体

`Ctrl+shift+t`   多开窗口

`alt+`数字   切换窗口

#### 命令：在命令行查找：/ + 要查找的内容  下一个n

##### bin目录里面就是命令：命令 + --help 查看命令使用帮助   man + 命令

- `~`  代表家目录   /  代表根目录
- `cd + 选项（参数）`   切换目录
  - `..  ` 代表上级目录  ` .`  代表当前目录    `cd ../../`   返回上上级目录
  - `cd`   后面跟随路径
  - `cd -`   返回上一次的目录（最近一次的目录）
- `ls`    目录清单（查看当前目录下的内容）
  - `ls -a`   显示所有文件夹
  - `ls -l`    这个文件的详细信息
    - 显示参数：权限    硬链接数量 （文件）     目录里文件的数量（目录）  
  - `ls -al`   组合使用
- `which + 命令`     查看该命令的位置
- `pwd`   查看当前所在目录
- `clear`   清屏
- `ps -aux | grep xx`   查看进程
- `netstat -ano | grep xx`    查看端口号

#### 系统文件夹所存放的文件类型：

- `dev` :设备文件
- `boot`：启动项
- `etc`：程序应用里面的配置项、启动脚本等（系统级别）
- `home`：家目录（有几个用户就有几个文件夹）
- `lib`：包含所有核心的系统函数
- `sbin`：超级管理员使用的命令
- `lost + found`：垃圾处理（临时存储）
- `media`：管理硬件，移动设备（鼠标键盘等）
- `mnt`：文件系统管理
- `opt`：提供一些可选的应用程序安装目录
- `proc`：特殊的动态目录，这个目录本身是一个系统，管理进程运行时产生的垃圾信息
- `root`：关于超级管理员的目录
- `sys`：系统文件
- `tmp`：临时文件，所有用户都能操作
- `usr`：关于用户的信息、配置文件（关于用户自己的）
- `var`：放变量的
- `run``：存放程序运行

#### 操作文件的命令：

- `> `   重定向
- `>>`    追加
- `echo`   输出     `echo hello > '1.txt'`    （创建1.txt    内容为hello）
- `cat`   预览内容
- `touch`   创建文件（同名文件不覆盖,不创建新文件，只能创建不同名的文件）
- `vim`   编辑器  也能创建文件
- `gedit`   编辑（Ubuntu   局限性）
- `ifconfig`    查看本机的IP地址    （Windows操作系统中为ipconfig）
- `history`   查看历史命令记录
  - `history -c`    全部清空

#### 操作文件夹（目录）的命令：

- `mkdir`   创建目录（文件夹）
  - `mkdir  -p  a/b/c`    创建多层级目录
- `rmdir`    删除目录（不常用，只能删除空目录）
  - `rmdir -p   a/b/c`   删除多层级目录（只能删除都是空目录）
  - `rmdir    a/b/c`    只删除了c这个空目录
- `rm`  可以删除任意东西
  - `rm -rf`    删除文件夹
  - `rm  +名字`    删除文件
  - `rm  *.txt`    删除所有txt文件（正则）
- `mv`   重命名/剪切
  - `mv  a   aaa`    将a重命名为aaa
  - `mv  a   Document/ccc`    将a剪切到Document文件夹并重命名为ccc
- `cp`   拷贝
  - `cp   a   aaa`    拷贝a 的内容到aaa 中    cp   源文件   目标文件
  - `cp  -r   a   aaa`     将文件夹a 的内容拷贝到文件夹aaa 中
  - `cp   a   -r  aaa`     将文件夹a 的内容拷贝到文件夹aaa 中
  - `cp   a   aaa   -r`
- `ln`  链接
  - `ln -s`  源文件的路径（绝对路径或者是相对于目标文件的路径）  目标文件的路径（目标文件可以改名）     软连接（不占用磁盘空间，做了个镜像），都能做
  - `ln`  源文件的路径（绝对路径或者是相对于目标文件的路径）   目标文件的路径（目标文件可以改名）     硬链接（拷贝了一个并链接），只能做文件，不能做文件夹
- `find`   查找相关文件   + 路径  可用星号（*）
  - `find  ngi* |  grep  con`    查找ngi*  里有con 的内容（查找内容）、管道过滤
  - `grep  a   *.txt`    寻找txt文件里内容有a 的文件
- `ps`   (进程processes)
  - `ps  -unx`  查看所有进程（-aux）
- 压缩和打包（先打包后压缩）
  - `tar -cvf`  目标文件  源文件    创建一个打包文件（打包过程都能看到）：c 创建  v 可视化  f 文件
  - `tar -xvf`  源文件  -C   目标文件夹    将源文件解包到目标文件夹中
    - `tar -xvf`  源文件   解包到当前目录
  - `tar -zcvf`  目标文件  压缩成gzip 格式  
    - `tar -jcvf`  目标文件    压缩成bz2 格式
  - `tar -zxvf`  源文件  -C   目标文件夹    将源文件解压到目标文件夹中
  - `tar zip` 目标文件  源文件
  - `tar unzip`   源文件  -C   目标文件夹    将源文件解压到目标文件夹中

#### 文件传输：

- 上传：`scp -r  本地目录  用户名  @服务器IP:目录`
- 下载：`scp -r  用户名  @服务器IP:目录  本地目录`

#### 磁盘管理：

- `df  选项  参数`   查看磁盘的使用情况
- `df  -h`    查看磁盘的使用情况
- `du -h`     家目录中的使用情况

#### 网络通讯：（虚拟技术、桥接）

- `ifconfig`      查看或设置网卡 
- `ifconfig  eth0  ip地址`   设置ip地址
- `netstat  -aup`    查看所有正在使用udp协议的端口   a 所有  u  udp协议   p 端口
- `netstat  -atp`     查看所有正在使用tcp协议的端口   a 所有  t  tcp协议   p 端口
- `ping`   检测主机
- udp协议    访问视频      tcp协议   

#### 权限管理：

- `sudo -s`
- `whoami`     查看当前用户（我是谁？）
- `who`    查看所有登录用户
- `useradd(选项)(参数)`    添加用户   (查看所有的用户 cat  /etc/passwd )
  - `sudo useradd   -m  aaa`    添加用户aaa 同时创建家目录
  - `-d`     指定登入是的起始目录
  - `-g`     指定创建组
  - `-G`     追加组（一个用户属于多个组）
- `userdel(选项)(参数)`     删除用户
  - `sudo userdel -f  aaa` 
- `usermod(选项)(参数)`    修改用户的信息
  - `sudo  usermod  first -d  /home/first/aaa`     登录之后的起始目录更改为aaa
- `groupadd`     创建组（查看所有的组cat /etc/group）
  - `groups  +  名字`    看这个属于哪个组
  - `gpasswd  -d  first  demo`    将first从demo组中删除
  - `gpasswd  -a  first   demo`    将first拉进demo组中
  - `usermod  -g demo  first`  将first放入demo组中
- `groupdel`     删除组
- `groupmod   ***  -a   +++`    将前一个组名修改为+++
- 给某用户sudo权限
  - `sudo  usermod  -a  -G sudo  username`      给某用户sudo权限
  - `sudo  usermod  -a  -G adm   username`      给某用户adm权限
- `su  username`    切换用户
- `chown`   所有权限
  - `sudo  chown  first:first   aaa`     更改aaa文件的所属者和所属组（只能更改当前aaa目录，不能层级传递）
  - `sudo  chown  -R  first:first   aaa`     更改aaa文件的所属者和所属组，层级目录也改
- `chmod`    修改权限
  - `1-->x   2-->w   4-->r   3-->xw   5-->xr    6-->wr   7-->xwr`
  - `sudo  chmod  u=rwx  g=rwx  o=rwx  file`    修改文件或目录file的权限
- `#!/usr/bin/env python`    头部表明使用python命令执行文件

#### 软件的安装和管理：

- `apt-get`  方式安装（Ubuntu）
  - 获取新的软件包   `apt-get update`
  - 添加包地址
    - `add-apt-repository ppa:webupd8team/java(地址)`             ppa(个人软件包档案)
    - 添加地址---更新软件包---安装
  - 删除软件包
    - `apt-get remove packagename`
  - 删除软件包包括配置文件
    - `apt-get --purge remove packagename`
  - 删除该软件的依赖包
    - `apt-get autoremove packagename`
  - 清理无用的包
    - `sudo apt-get clean && sudo apt-get autoclean`
  - 获取新的软件包
    - `apt-get update`
  - 升级有可用更新的系统
    - `apt-get  upgrade`
  - 下载该包的源代码
    - `apt-get source package`
  - 更多命令和选项
    - `apt-get help`

- 源码安装

  - 下载源码
    - `wget  url`
    - `wget `
  - 解压压缩包
    - `tar -zxvf`  压缩包名
  - `cd` 到压缩文件夹
    - 找到configure    这是一个可执行脚本
    - 执行    `./configure --prefix +编译路径  [-endable-optimizations]`   注意前面的  ./  不可少
  - 编译
    - `make all`
  - 安装
    - `make install`
  - 多版本共存
    - `sudo update-alternatives  --install  link  name  path(目录)   priority(优先级)[]`

  #### 操作电脑的命令：

  - 关机
    - `halt`  立刻关机
    - `poweroff`   立刻关机
    - `shutdown  -h  now`  立刻关机（root用户使用）
    - `shutdown  -h  10`    10分钟后自动关机
  - 重启
    - `reboot`
    - `shutdown  -r now`
    - `shutdown  -r  now`  立刻重启（root用户使用）
    - `shutdown  -r  8:35`    10分钟后自动重启
    - `shutdown  -c`    取消重启
  - 进程
    - `ps  -aux`   显示所有包含使用者的进程
    - `top`   动态显示所有进程
    - `kill  pid`   杀死进程
    - `kill  -9  pid`   强制杀死进程
    - 杀不掉的进程是守护进程  查找守护进程  `cd /etc/init.d/`    （所有守护进程都在这）
      - `sudo  /etc/init.d/进程名  stop(start/restart)`     停止/开启/重启 守护进程