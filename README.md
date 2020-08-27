# 介绍

这是来自st官方github上的u-boot源码, 假如你想要st最新的, 你
可以使用如下命令下载最新的源码

```
~$ git clone https://github.com/STMicroelectronics/u-boot.git
```

官方源码编译文档
https://wiki.st.com/stm32mpu/nsfr_img_auth.php/b/b9/U-Boot.README.HOW_TO.txt

安装交叉编译器
=============
```
~$ sudo apt install gcc-arm-linux-gnueabi
```

编译
====
### 方法一
有三个编译选项, 具体可参考官方介绍, 我们这里只编译trusted
1. stm32mp15_basic_defconfig
2. stm32mp15_optee_defconfig
3. stm32mp15_trusted_defconfig

```
~$ source /opt/st/stm32mp1-demo-logicanalyser/2.6-snapshot/environment-setup-cortexa7t2hf-neon-vfpv4-ostl-linux-gnueabi
~$ make ARCH=arm stm32mp15_trusted_defconfig 
~$ make ARCH=arm CROSS_COMPILE=arm-ostl-linux-musl- DEVICE_TREE=stm32mp157c-dk2 all

```
### 方法二
```
~$ source /opt/st/stm32mp1-demo-logicanalyser/2.6-snapshot/environment-setup-cortexa7t2hf-neon-vfpv4-ostl-linux-gnueabi
//修改makefile.sdk
//选择编译链，这里选择arm-ostl-linux-musl-
EXTRA_OEMAKE=CROSS_COMPILE=arm-ostl-linux-musl- CC="arm-ostl-linux-musl-gcc $(KCFLAGS)" V=1  PYTHON=nativepython  
//选择defconfig，这里选择stm32mp15_trusted_defconfig
UBOOT_CONFIGS ?=   stm32mp15_trusted_defconfig,trusted,u-boot.stm32  
//选择设备树，这里选择stm32mp157a-lierda-8e512d
DEVICE_TREE ?=   stm32mp157a-lierda-8e512d
~$ make -f ../Makefile.sdk  all
```

烧写
====
```
~$ sudo dd if=u-boot-stm32mp157c-dk2-trusted.stm32 of=/dev/sdc3 bs=1M conv=fdatasync status=progress
```
