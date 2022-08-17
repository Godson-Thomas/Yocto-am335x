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


* The Image files can be found in 
```
cd tmp/deploy/images/am335x-evm

ls 
```