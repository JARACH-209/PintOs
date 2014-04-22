A Brief Configuration Guide in Plain Text
====

(2014-4-15) nekketsuing >> Some is derived from 'Pintos安装简明教程_201402200014.docx' by microdog. Thanks to Kainan Zhu!<br>
(2014-4-22) nekketsuing >> Some useful note added here!<br>

----

sudo apt-get install build-essential<br>
sudo apt-get install git-core<br>

git clone http://cs140.stanford.edu/pintos.git<br>

sudo gedit /etc/profile<br>
export PINTOSDIR=/home/pintos/pintos/<br>
source /etc/profile<br>

sudo apt-get install libx11-dev xorg-dev<br>
sudo apt-get install libncurses5 libncurses5-dev<br>
wget http://sourceforge.net/projects/bochs/files/bochs/2.2.6/bochs-2.2.6.tar.gz/download -O bochs-2.2.6.tar.gz<br>
cd $PINTOSDIR/src/misc/<br>
sudo env SRCDIR=/home/pintos/ PINTOSDIR=/home/pintos/pintos/ DSTDIR=/usr/local/ sh ./bochs-2.2.6-build.sh<br>

cd $PINTOSDIR/src/utils<br>
sudo ln backtrace pintos pintos-gdb pintos-mkdisk pintos-set-cmdline Pintos.pm /usr/local/bin/<br>

cd $PINTOSDIR/src/misc<br>
sudo ln gdb-macros /usr/local/bin/<br>
gedit $PINTOSDIR/src/utils/pintos-gdb<br>
GDBMACROS=/home/pintos/pintos/src/misc/gdb-macros<br>

cd $PINTOSDIR/src/utils/<br>
make<br>
sudo ln squish-pty /usr/local/bin/<br>

cd $PINTOSDIR/src/threads<br>
make check<br>
pintos -v -- run alarm-multiple<br>

<br>

#####And if bochs-2.2.6 doesn't work, try bochs-2.6 or higher!<br>
cd bochs-2.6<br>
./configure --with-nogui --enable-gdb-stub<br>
make<br>
sudo make install<br>
sudo chmod -R 777 /usr/local/<br>

#####And if you meet up with an error while compiling under /src/utils/ :<br>
cd $PINTOSDIR/src/utils/<br>
gedit Makefile<br>
Line 4 : LDFLAGS = -lm => LDLIBS = -lm<br>
