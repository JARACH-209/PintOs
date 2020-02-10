# Some notes on configuration

##### Great thanks to Kainan Zhu's note for basic ideas.

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

##### You may try bochs-2.6 or higher if bochs-2.2.6 doesn't work properly.
cd bochs-2.6<br>
./configure --with-nogui --enable-gdb-stub<br>
make<br>
sudo make install<br>
sudo chmod -R 777 /usr/local/<br>

##### A feasible solution to compilation error under /src/utils/ :
cd $PINTOSDIR/src/utils/<br>
gedit Makefile<br>
Line 4 : LDFLAGS = -lm => LDLIBS = -lm<br>
