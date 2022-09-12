<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/am335x.PNG" width="650">  <br><br>


_Make sure your Build Host meets the following requirements:_
<br><br>



<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/SYSreq.PNG" width="800">  <br><br>


## _Install essential host packages on the host machine_



```
$ sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool

```



## _Use Git to Clone Poky_

```
git clone git://git.yoctoproject.org/poky
```
* Initialize the Build Environment
```
cd ~/poky

source oe-init-build-env

```
## _Use Git to Clone meta-ti_

* In general, layers are repositories that contain related sets of instructions and configurations that tell the Yocto Project what to do.Many hardware layers are available. _meta-ti_ is one such layer.

```
cd poky

git clone https://git.yoctoproject.org/meta-ti.git
```

* Move **meta-ti-bsp** directory from **meta-ti** to **poky** directory.The _meta-ti-bsp_ contains all the metadata needed to support hardware from _Texas Instruments_

- _Note_: Since _meta-ti-bsp_ layer depends on the _meta-arm_ layer, we have to clone that too.

```
git clone https://git.yoctoproject.org/meta-arm.git

```

## _Configuring files for the build_
<br>

### _Add the Layer to the Layer Configuration File_
```
cd ~/poky/build/conf
```
* Open _bblayers.conf_ file

```
gedit bblayers.conf
```
* Include the path of the BSP(Board Support Package) as shown.<br><br>

<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/bblayers.PNG" width="600">  <br><br>





### _Change the Configuration to Build for a Specific Machine_

* Open the _local.conf_ file for setting the MACHINE Variable.
The MACHINE variable in the _local.conf_ file specifies the machine for the build. Set the MACHINE variable to _am335x-evm_.

<br><br>

<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/local.PNG" width="600">  <br><br>


## _Start the Build_



* Building
```
cd ~/poky/build

bitbake core-image-minimal
```


- * **Error 1**

<br><br>

<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/error1.PNG" width="800">  <br><br>

- * **Solution**

```
pip install lzip

cd ~/poky/meta-ti-bsp/recipes-bsp/u-boot

gedit u-boot-ti.inc

```

Change _lzop_ to _lzip_

<br><br>

<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/lzopsolved.PNG" width="700">  <br><br>


* After building, the _image files_ can be found in 
```
cd tmp/deploy/images/am335x-evm

ls 
```

```
```


.dtb+zImage --> fitimage (.its and makefit.sh)

fitimage+rootfs --> ubi image (config.ini+ubinize.sh)

```
```



- _Note_: Use the following command to clean/delete the generated output _images_ without effecting the intermediate steps.
```
bitbake -c cleanall core-image-minimal
```

- _Note_: Use the following command to recompile the generated kernel with the original kernel source.
```
bitbake -C compile virtual/kernel
```


**Error:**
<br><br>

<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/e_kc.png" width="1000">  <br><br>

**Solution:**
```
cd /media/godsonthomas/Disk-2/poky/build/tmp/work-shared/am335x-evm/kernel-source
```
```
make mrproper
```
```
bitbake -C compile virtual/kernel
```
-----

## _Compiling external Kernel Source using_ _arm-poky-linux-gnueabi-_

- **go to the linux kernel source directory**

```
1. make distclean

2. export CROSS_COMPILE=/media/godsonthomas/Disk-2/poky/build/tmp/work/am335x_evm-poky-linux-gnueabi/linux-ti-staging/5.10.109+gitAUTOINC+9cff62efac-r22b/recipe-sysroot-native/usr/bin/arm-poky-linux-gnueabi/arm-poky-linux-gnueabi-

3. export ARCH=arm

4. make menuconfig

5. make zImage
```

The generated kernel can be found in:

_arch/arm/boot/zImage_

Error: 
<br><br>

<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/yylloc.png" width="800">  <br><br>

Solution:

edit the file _./linux-rtk/scripts/dtc/dtc-lexer-lex.c
Find the line 'YYLTYPE yylloc' and change it to 'extern YYLTYPE yylloc'_

<br><br>

<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/yylloc_solved.png" width="600">  <br><br>




Error: 
<br><br>

<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/certificate.png" width="800">  <br><br>

Solution:

```
gedit .config
```
Change the file as shown:

<br><br>

<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/certi_solved.png" width="600">  <br><br>



### _Creating the dtb file for generating fit image_

```
make dtbs
```

```
```

### Converting .dtb to .dts

```
dtc -I dtb "/pns/sdm_a/zzz.dtb" >/tmp/zzz.dts
```

### Converting .dts to .dtb
```
dtc -I dts "/tmp/zzz.dts" >/pns/sdm_a/zzz.dtb
```


```
```



<br><br>

<img src="https://github.com/Godson-Thomas/Yocto-am335x/blob/master/exp.jpg" width="600">  <br><br>
-------------------