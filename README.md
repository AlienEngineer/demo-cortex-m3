# Ajhc demo for Cortex-M3/4 board [![Build Status](https://travis-ci.org/ajhc/demo-cortex-m3.png)](https://travis-ci.org/ajhc/demo-cortex-m3)

First, you have to know there are several boards supported by the demo.
Second, the demo needs Ajhc 0.8.0.7 or later.

Using following boards are strongly recommend, so you can use gdb.

* stm32f3-discovery
* mbed NXP LPC1768

## Demo movies

* [Haskell code uses Haskell heap on Cortex-M4. - YouTube](http://www.youtube.com/watch?v=bKp-FC0aeFE)
* [Haskell code is running on Cortex-M3! - YouTube](http://www.youtube.com/watch?v=3R9sogReVHg)
* [Don't stop me now! #Ajhc #Haskell #Heap #CortexM4 #STM32 - YouTube](http://www.youtube.com/watch?v=8bEF2fr5b00)

## [stm32f3-discovery](http://www.st.com/web/en/catalog/tools/FM116/SC959/SS1532/PF254044)

![](https://raw.github.com/ajhc/demo-cortex-m3/master/img/stm32f3-discovery.jpg)

* CPU: STM32F303VCT6
* ROM: 256kB
* RAM: 48kB

## [mbed NXP LPC1768](https://mbed.org/handbook/mbed-NXP-LPC1768)

![](https://raw.github.com/ajhc/demo-cortex-m3/master/img/mbed-nxp-lpc1768.jpg)

* CPU: LPC1768
* ROM: 512kB
* RAM: 32kB

## [LPCXpresso Board for LPC1769](http://www.nxp.com/demoboard/OM13000.html)

![](https://raw.github.com/ajhc/demo-cortex-m3/master/img/lpcxpresso-lpc1769.jpg)

* CPU: LPC1769
* ROM: 512kB
* RAM: 64kB

## [stbee-mini](http://strawberry-linux.com/catalog/items?code=32105)

![](https://raw.github.com/ajhc/demo-cortex-m3/master/img/stbee-mini-v2.jpg)

* CPU: STM32F103CBT6
* ROM: 128kB
* RAM: 20kB

## Common requirements

### Install summon-arm-toolchain

    $ git clone https://github.com/vedderb/summon-arm-toolchain.git
    $ apt-get install flex bison libgmp3-dev libmpfr-dev libncurses5-dev \
      libmpc-dev autoconf texinfo build-essential libftdi-dev zlib1g-dev \
      git zlib1g-dev python-yaml
    $ cd summon-arm-toolchain/
    $ ./summon-arm-toolchain
    $ export PATH=$HOME/sat/bin:$PATH

### Install Ajhc

    $ sudo apt-get install make locales autoconf libreadline-dev \
      libwww-perl libconfig-yaml-perl graphviz haskell-platform drift pandoc \
      libghc-readline-dev libghc-utf8-string-dev libghc-hssyck-dev libghc-pandoc-dev
    $ git clone https://github.com/ajhc/ajhc.git
    $ cd ajhc
    $ git checkout arafura
    $ autoreconf -i
    $ ./configure
    $ make
    $ sudo make install
    $ ajhc --version |head -1
    ajhc 0.8.0.1 (-0)

## If you use stm32f3-discovery

### Install stlink

    $ sudo apt-get install libsgutils2-dev libusb-1.0-0-dev
    $ git clone https://github.com/texane/stlink.git
    $ cd stlink/
    $ ./autogen.sh
    $ ./configure
    $ make
    $ sudo make install

### How to build

    $ git clone https://github.com/ajhc/demo-cortex-m3
    $ cd demo-cortex-m3/stm32f3-discovery/
    $ make

### Write to flash

In one terminal, start the connection to the board.

    $ sudo st-util

In another terminal, connect to the debugger and flash program.

    $ make gdbwrite
    --snip--
    Loading section .isr_vector, size 0x188 lma 0x8000000
    Loading section .text, size 0x1ec4 lma 0x8000188
    Loading section .data, size 0x5c lma 0x800204c
    Start address 0x8001f89, load size 8360
    Transfer rate: 6 KB/sec, 2786 bytes/write.
    (gdb) c

## If you use mbed NXP LPC1768

### Upgrade firmware

http://mbed.org/handbook/Firmware-LPC1768-LPC11U24

Use rev 141212 or later firmware, to support CMSIS-DAP.

### Install pyOCD

http://mbed.org/blog/entry/Debugging-from-GDB-using-pyOCD/

    $ sudo apt-get install python libusb-1.0-0-dev
    $ git clone https://github.com/walac/pyusb.git
    $ cd pyusb
    $ sudo python setup.py install
    $ git clone https://github.com/mbedmicro/pyOCD.git
    $ cd pyOCD
    $ sudo python setup.py install

### How to build

    $ cd demo-cortex-m3/mbed-nxp-lpc1768
    $ cd samples/Haskell
    $ make

### Write to flash

In one terminal, start the connection to the board.

    $ cd demo-cortex-m3/mbed-nxp-lpc1768
    $ sudo python gdbserver4mbed.py

In another terminal, connect to the debugger and flash program.

    $ cd demo-cortex-m3/mbed-nxp-lpc1768
    $ ./BuildShell
    $ cd samples/Haskell
    $ make gdbwrite
    (gdb) c

## If you use LPCXpresso Board for LPC1769

    $ cd demo-cortex-m3/mbed-nxp-lpc1768
    $ ./BuildShell
    $ cd samples/Haskell
    $ make gdbwrite

### Get debug board

LPC-Link is not used with Linux Host.
A debug board is needed for writing firmware to LPCXpresso Board.
You can get more infomation from the following links.

* [Prototype to hardware | mbed](http://mbed.org/users/chris/notebook/prototype-to-hardware/)
* [LPCXpresso LPC1768登場 - Lynx-EyEDの電音鍵盤 新館](http://lynxeyed.hatenablog.com/entry/20100810/1281423414)

### Install lpc21isp

    $ svn co https://lpc21isp.svn.sourceforge.net/svnroot/lpc21isp
    $ cd lpc21isp/
    $ make
    $ sudo cp lpc21isp /usr/local/bin/

### How to build

    $ git clone https://github.com/ajhc/demo-cortex-m3.git
    $ cd demo-cortex-m3/lpcxpresso-lpc1769/blinky/
    $ make

### Write to flash

Connect up the LPCXpresso Board to PC with USB cable.
Pull nReset low to put the LPC1769 into reset.
Pull the ISP pin (P0.14) low.
Pull nReset high, LPC1769 will be boot with ISP mode.

Then, flash program with below command.

    $ make flash

Pull nReset low, and pull nReset high.

## If you use stbee-mini

### Install dfu-util

    $ git clone git://gitorious.org/~tormod/unofficial-clones/dfuse-dfu-util.git
    $ cd dfuse-dfu-util/
    $ ./autogen.sh
    $ ./configure
    $ make
    $ sudo make install

### How to build

    $ git clone https://github.com/ajhc/demo-cortex-m3.git
    $ cd demo-cortex-m3/stbee-mini/
    $ make

### Write to flash

Press and release the reset switch while holding down the user switch.
Then release the user switch.
The hardware will boot with DFU download mode.

Run below command on Linux box to flash program.

    $ make write2stbeemini

Press and release the reset switch to boot with normal mode.

## Porting the demo to a new platform

### Get base source codes written with C language

C language code is needed to launch Haskell code.
For example, https://github.com/jeremyherbert/stm32-templates.

### Write Haskell code

    $ mkdir hs_src
    $ vi hs_src/Main.hs

Note that the below idiom is used to access Memory-mapped I/O.

    $ vi hs_src/Main.hs
    --snip--
    foreign import ccall "c_extern.h &jhc_zeroAddress" c_jhc_zeroAddress32 :: Ptr Word32
    
    myPtr :: Ptr Word32
    myPtr = c_jhc_zeroAddress32 `plusPtr` myAddr
      where myAddr = 0x48001028
    
    writeMyPtr :: Word32 -> IO ()
    writeMyPtr = poke myPtr
    --snip--
    $ vi c_extern.h
    extern volatile void jhc_zeroAddress;

For example,
[demo-cortex-m3/stm32f3-discovery/hs_src/Main.hs](/stm32f3-discovery/hs_src/Main.hs).

### Call Haskell code from C language code

    $ vi src/main.c
    int main(void) {
    /* --snip-- */
          { /* Run Haskell code */
               int hsargc = 1;
               char *hsargv = "t";
               char **hsargvp = &hsargv;
               
               hs_init(&hsargc, &hsargvp);
               _amain();
               /* hs_exit(); */
          }
    /* --snip-- */

For example,
[demo-cortex-m3/stm32f3-discovery/src/main.c](/stm32f3-discovery/src/main.c).

### Create alloc.c if don't have malloc function

Some software architecture on tiny CPU often have no malloc function.
But Ajhc's garbage collector (jgc) needs the malloc.

For example,
[demo-cortex-m3/stm32f3-discovery/src/alloc.c](/stm32f3-discovery/src/alloc.c).

### Add the others as dummy function

Implement the functions that are used by Ajhc's RTS.

For example,
[demo-cortex-m3/stm32f3-discovery/src/dummy4jhc.c](/stm32f3-discovery/src/dummy4jhc.c).

### Modify Makefile on top directory for compiling the Haskell code

Covert Haskell code to C language with Ajhc,
and extract Ajhc's RTS source code on "jhc_custom/rts/src" directory.

    $ vi Makefile
    # --snip--
    JHCRTS_LIB=jhc_custom/rts
    JHCRTS_SRC=jhc_custom/rts/src
    SRCS += hs_main.c alloc.c dummy4jhc.c
    CFLAGS += -Wl,--defsym,jhc_zeroAddress=0
    
    all: $(TARGET)
    
    $(TARGET): $(SRCS) jhcrts
    	$(CC) $(CFLAGS) $(SRCS) -o $@ -L$(JHCRTS_LIB) -ljhcrts
    
    jhcrts: hs_main.c
    	$(MAKE) -C $(JHCRTS_LIB)
    
    hs_main.c: hs_src/Main.hs
    	ajhc -fffi --tdir=$(JHCRTS_SRC) -C -o $@ $<
    # --snip--

For example,
[demo-cortex-m3/stm32f3-discovery/Makefile](/stm32f3-discovery/Makefile).

### Create Makefile for compiling Ajhc's RTS

Create Makefile for compiling Ajhc's RTS extracted on "jhc_custom/rts/src" directory.
Detail of CFLAGS: [Ajhc User's Manual / Special defines to set cflags](http://ajhc.github.com/manual.html#special-defines-to-set-cflags).

    $ mkdir -p jhc_custom/rts
    $ vi jhc_custom/rts/Makefile
    # --snip--
    JHCRTS_SRC = src
    vpath %.c $(JHCRTS_SRC)/rts
    SRCS = gc_jgc.c jhc_rts.c stableptr.c rts_support.c
    OBJS = $(SRCS:.c=.o)
    
    CFLAGS += -I$(JHCRTS_SRC) -std=gnu99
    CFLAGS += -DNDEBUG -D_JHC_GC=_JHC_GC_JGC -D_JHC_STANDALONE=0
    CFLAGS += -D_JHC_ARM_STAY_IN_THUMB_MODE
    CFLAGS += -D_JHC_JGC_STACKGROW=128 -D_JHC_JGC_LIMITED_NUM_MEGABLOCK=1
    CFLAGS += -D_JHC_JGC_BLOCK_SHIFT=9 -D_JHC_JGC_MEGABLOCK_SHIFT=14
    CFLAGS += -D_JHC_JGC_GC_STACK_SHIFT=8 -D_JHC_JGC_LIMITED_NUM_GC_STACK=1
    CFLAGS += -D_JHC_JGC_NAIVEGC
    
    all: libjhcrts.a
    
    libjhcrts.a: $(OBJS)
    	$(AR) -r $@ $(OBJS)
    
    %.o : %.c
    	$(CC) $(CFLAGS) -c -o $@ $^
    # --snip--

For example,
[demo-cortex-m3/stm32f3-discovery/jhc_custom/rts/Makefile](/stm32f3-discovery/jhc_custom/rts/Makefile).

## Original source code

This demo is based on below original source code.

* https://github.com/mblythe86/stm32f3-discovery-basic-template
* http://strawberry-linux.com/pub/mini-demo.zip
* https://github.com/openspaceaarhus/PhotonSaw
* https://github.com/christianjann/microcli-stm32f0-discovery
* https://github.com/adamgreen/gcc4mbed
