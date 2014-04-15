A Brief Configuration Guide in Plain Text
====

Derived from 'Pintos安装简明教程_201402200014.docx' by microdog. Thanks to Kainan Zhu!
-------

......<br>

<br>3.2	设置软件源，并安装编译所需的基础组件。<br>
按Win键或点击左上角Ubuntu图标，在搜索中输入software，执行Software & Updates。<br>
 <br>
展开Download from下拉列表，点击Other。在新窗口中展开China，选择中科大软件源（mirrors.ustc.edu.cn），点击Choose Server关闭对话框。可能会提示输入密码。完成后点击Close关闭Software & Updates窗口。<br>
 <br>
打开终端<br>
执行 sudo apt-get update 更新软件源清单，时间长短与网速有关。<br>
 <br>
（可选）更新操作系统。执行 sudo apt-get upgrade。一路确认即可，时间较长，与性能和网速有关。<br>
 <br>
安装build-essential。<br>
执行 sudo apt-get install build-essential<br>
 <br>
（可选，若不安装则可使用gedit）安装vim。<br>
执行 sudo apt-get install vim<br>
 <br>
安装git。<br>
执行 sudo apt-get install git-core<br>
 <br>
4	获取pintos代码<br>
打开终端。<br>
执行 git clone http://cs140.stanford.edu/pintos.git<br>
则home目录下会多出一个pintos文件夹，其中为pintos代码。此时pintos代码路径为/home/pintos/pintos/。（第一个pintos为用户名，第二个pintos是我们使用git工具通过网络下载的pintos代码）若需将pintos放置于其他位置，在执行git clone操作之前切换目录即可。 <br>
 <br>
配置PINTOSDIR环境变量。有两种方法：<br>
一，使用一条语句解决问题，在/home/pintos/pintos/目录下（接上步）执行<br>
sudo bash -c 'echo -e "\nexport PINTOSDIR="`pwd`"/\n" >> /etc/profile' && source /etc/profile<br>
 <br>
二，执行 sudo gedit /etc/profile ，使用图形化编辑器打开/etc/profile文件，在文件结尾新开一行，输入：<br>
export PINTOSDIR=/home/pintos/pintos/<br>
保存后退出，执行 source /etc/profile 使改变生效。<br>
 <br>
5	获取并安装bochs虚拟机。<br>
5.1	下载bochs 2.2.6<br>
访问bochs项目主页http://bochs.sourceforge.net/<br>
在左侧点击进入See All Releases，若无法正常显示，尝试访问http://sourceforge.net/projects/bochs/files<br>
展开bochs文件夹，找到并进入2.2.6文件夹，找到bochs-2.2.6.tar.gz文件，并复制其超链接（http://sourceforge.net/projects/bochs/files/bochs/2.2.6/bochs-2.2.6.tar.gz/download）。<br>
打开终端，执行<br>
wget http://sourceforge.net/projects/bochs/files/bochs/2.2.6/bochs-2.2.6.tar.gz/download -O bochs-2.2.6.tar.gz<br>
将bochs-2.2.6.tar.gz下载到本地。<br>
 <br>
5.2	使用安装脚本一键安装bochs<br>
本节使用pintos提供的安装脚本一键安装bochs，分步安装较为繁琐暂不介绍。（该安装脚本中会自动安装两种版本的bochs，分别为开启--enable-gdb-stub参数和--enable-debugger参数，参见pintos手册G.1部分）<br>

<br>打开终端。<br>
执行<br>
cd $PINTOSDIR/src/misc/<br>
进入$PINTOSDIR/src/misc/目录。<br>

<br>执行<br>
sudo env SRCDIR=/home/pintos/ PINTOSDIR=/home/pintos/pintos/ DSTDIR=/usr/local/ sh ./bochs-2.2.6-build.sh<br>
启动安装脚本。其中SRCDIR指向的是bochs安装压缩包所在的目录（本例为/home/pintos/），PINTOSDIR指向pintos代码（本例为/home/pintos/pintos/），DSTDIR指向安装目录（默认/usr/local/即可）。<br>
配置和安装过程可能会遇到很多缺少依赖库的提示，多利用谷歌，补装对应的包，下文列出了本文安装过程中遇到的两个缺少依赖库的错误及解决方法。<br>
整个安装过程大约在10分钟左右，与电脑性能有关。<br>
 <br>
安装过程中若出现ERROR: X windows gui was selected, but X windows libraries were not found.提示，则执行 sudo apt-get install libx11-dev xorg-dev 后重试。<br>
 <br>
安装过程中若出现Curses library not found: tried curses, ncurses, termlib and pdcurses.提示，则执行 sudo apt-get install libncurses5 libncurses5-dev 后重试。<br>
 <br>
等待安装完毕。<br>
 <br>
6	配置pintos相关程序<br>
6.1	安装pintos脚本<br>
打开终端。执行 cd $PINTOSDIR/src/utils 进入$PINTOSDIR/src/utils目录。<br>
执行<br>
sudo ln backtrace pintos pintos-gdb pintos-mkdisk pintos-set-cmdline Pintos.pm /usr/local/bin/<br>
 <br>
6.2	配置pintos-gdb<br>
打开终端。执行 cd $PINTOSDIR/src/misc 进入$PINTOSDIR/src/misc目录。<br>
执行<br>
sudo ln gdb-macros /usr/local/bin/<br>

<br>执行 gedit $PINTOSDIR/src/utils/pintos-gdb ，编辑pintos-gdb文件。（或使用vim $PINTOSDIR/src/utils/pintos-gdb编辑）。修改文件开始部分的GDBMACROS宏为gdb-macros所在路径，本例中为/home/pintos/pintos/src/misc/gdb-macros。保存后退出。<br>
 <br>
执行 pintos-gdb，若无错误提示则配置成功。<br>
 <br>
6.3	编译并安装其余pintos工具<br>
打开终端。执行 cd $PINTOSDIR/src/utils/ 进入$PINTOSDIR/src/utils/目录。<br>
执行make，编译剩余工具。执行 sudo ln squish-pty /usr/local/bin/。<br>
 <br>
6.4	编译运行pintos<br>
打开终端。执行cd $PINTOSDIR/src/threads，进入$PINTOSDIR/src/threads目录。<br>
执行make，编译Project 1（threads）。不应超过30秒，且不应有错误。<br>
 <br>
执行pintos -- run alarm-multiple，观察运行结果。<br>
 <br>
点击bochs右上角电源图标结束虚拟机。<br>
 <br>
执行pintos -v -- run alarm-multiple，观察运行结果。键入Ctrl+C中断虚拟机。<br>
 <br>
若本步骤中没有错误出现，则pintos已被成功安装。<br>
