How to build LiveSuite for Cubieboard on a recent Ubuntu machine:

```
root@ubuntu2204:~/sunxi/sunxi-livesuite-master/awusb# uname -a
Linux ubuntu2204 6.8.0-31-generic #31-Ubuntu SMP PREEMPT_DYNAMIC Sat Apr 20 00:40:06 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

Clone this repo:
https://github.com/linux-sunxi/sunxi-livesuite


Makefile required some patches (note: the override of the compiler and SUBDIRS to M):

```
obj-m := awusb.o
KDIR := /lib/modules/$(shell uname -r)/build
PWD := $(shell pwd)
CC := x86_64-linux-gnu-gcc-13

default:
	$(MAKE) -C $(KDIR) M=$(PWD) modules
clean:
	$(MAKE) -C $(KDIR) M=$(PWD) clean
	rm -rf Module.markers module.order module.sysvers 
```

also need to install flex and bison.


To fix the missing png dependency for the userland tool:

```
apt-get install libtool autoconf build-essential pkg-config automake tcsh zlib1g-dev
wget http://archive.ubuntu.com/ubuntu/pool/main/libp/libpng/libpng_1.2.54.orig.tar.xz
tar xvf  libpng_1.2.54.orig.tar.xz 
cd libpng-1.2.54
./autogen.sh
./configure
make -j8 
sudo make install
ldconfig
```

... and now you may finally start ./LiveSuite.sh
