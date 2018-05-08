---
layout: post
title: 'Guide - cross-compile Qt for Raspberry Pi 3'
key: 20180508
category: Qt
tags:
- Guide
- Qt
- RPi3
---

Cross-compile Qt-Rpi3 教學
Qt ver. **5.10.1**
Raspbian **2018-04-18-Stretch**
Ubuntu ver. **16.01**

<!--more-->

## Reference
### Official Guide

- [Raspberry Pi Beginners Guide](https://wiki.qt.io/Raspberry_Pi_Beginners_Guide)
- [RaspberryPi2EGLFS](https://wiki.qt.io/RaspberryPi2EGLFS)
- [Native Build of Qt5 on a Raspberry Pi](http://wiki.qt.io/Native_Build_of_Qt5_on_a_Raspberry_Pi)
- [Qt for Embedded Linux](http://doc.qt.io/qt-5/embedded-linux.html)

_ _ _

###Other Guide

- [Cross-compilation guide for Qt 5.9.4 and RPi](https://www.raspberrypi.org/forums/viewtopic.php?f=67&t=204529)
- [How to cross compile QT for Raspberry Pi 3 on Linux (Ubuntu) for Beginners!](https://medium.com/@amirmann/how-to-cross-compile-qt-for-raspberry-pi-3-on-linux-ubuntu-for-beginners-75acf2a078c)
- [Guide To Cross Compile Qt 5.4 for the Raspberry Pi](https://exploreembedded.tumblr.com/)
- [Configuring Qt Creator for the Raspberry Pi](https://www.ics.com/blog/configuring-qt-creator-raspberry-pi)

_ _ _

## Preparation
**[硬體]**
PC x1
Raspberry Pi x1
5v 2A 電源 for RPi x1
USB to TTL x1
HDMI or HDMI to VGA cable x1
Touchscreen (Inculde power) x1

**軟體**
RASPBIAN
下載 Full版 IMAGE [舊版的](http://downloads.raspberrypi.org/raspbian/images/) or [最新版](https://www.raspberrypi.org/downloads/raspbian/). 然後燒錄至SD Card. (**@Lite版沒測試過，有看到成功案例**)
- [Linux燒錄教學](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)
- [Windows燒錄教學](https://www.botsheet.com/cht/raspberry-pi-tutorial-install-raspbian-windows/)
> 如果沒有螢幕，第一次BOOT前，最好先開啟SSH -- [手動開啟SSH](https://www.raspberrypi.org/blog/a-security-update-for-raspbian-pixel/)。

Ubuntu
下載 [16.04](https://www.ubuntu-tw.org/modules/tinyd0/)

> 如果在Windows底下可以用虛擬機器 ([Victualbox](https://www.virtualbox.org/))
> 最好先掛好共享資料夾 ，Guest Additions 經常掛載失敗，需額外安裝元件
```github
sudo apt-get install virtualbox-guest-dkms
```

## Step by step
### Prepare Raspberry Pi
**[on RPi]** 裝上SDCard ,網路線 ,USB to TTL ,插電

**[on RPi]** Edit sources list in /etc/apt/sources.list and uncomment the *deb-src* line
```github
sudo nano /etc/apt/sources.list
```
uncomment = 拿掉下面這行的#字符號
```bash
#deb-src http://raspbian.raspberrypi.org/raspbian/ stretch main contrib non-free rpi
```

**[on RPi]** (optional) 執行 raspi-config :
1. Change it to boot to the console instead of X.
2. Change the GPU memory to 256 MB.


```github
sudo raspi-config
```
**[on RPi]** 系統更新
```github
sudo apt-get update
sudo apt-get dist-upgrade
sudo reboot
```
**[on RPi]** 韌體更新 [(額外要注意的BUG)](https://bugreports.qt.io/browse/QTBUG-62216)，後續會手動處理symlinks，可忽略。
```github
sudo rpi-update
sudo reboot
```
**[on RPi]** (optional)安裝遠端連線，方便測試 ([相關教學](http://atceiling.blogspot.tw/2017/03/raspberry-pi-vncteamviewrdp.html))
```github
$ sudo apt-get install xrdp
```

**[on RPi]** Install dependencies
```github
sudo apt-get build-dep qt4-x11
sudo apt-get build-dep libqt5gui5
sudo apt-get install libudev-dev libinput-dev libts-dev libxcb-xinerama0-dev libxcb-xinerama0
```

**[on RPi]** Prepare our target directory
```github
sudo mkdir /usr/local/qt5pi
sudo chown pi:pi /usr/local/qt5pi**
```
***==If ready, dont shutdown your raspberry pi !!==***

- - -


###Prepare Linux host
**[on host PC] **Update
```github
sudo apt-get update
sudo apt-get -y upgrade
```

**[on host PC] **Install dependencies
```github
sudo apt-get install git bison python gperf
```

**[on host PC] ** Installing QT Creator 5.10.1 on your Linux .
( [官網下載](https://www.qt.io/download) )

**[on host PC] **Generate ssh public key
```github
ssh-keygen -t rsa -C pi@raspberrypi.local -N "" -f ~/.ssh/id_rsa
```
```github
cat ~/.ssh/id_rsa.pub | ssh -o StrictHostKeyChecking=no pi@raspberrypi.local "mkdir -p .ssh && chmod 700 .ssh && cat >> .ssh/authorized_keys"
```

_ _ _

**[on host PC] ** Create our working directory:
```
mkdir ~/raspi
cd ~/raspi
git clone https://github.com/raspberrypi/tools
```
**[on host PC] ** Get a toolchain:
```
git clone https://github.com/raspberrypi/tools
```
Add toolchain binary directory to PATH. Open .bashrc and add line at end of file
```
nano ~/.bashrc
```
把下面這行加入到.bashrc這個文件的最下面
```bash
export PATH=$PATH:/raspi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin
```
> To activate this changes, you have to restart the terminal.
If you use a 32Bit host, use this toolchain path: /raspi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian/bin


用which測試你的環境變數是否正確載入Toolchan位置
```
 which arm-linux-gnueabihf-gcc
```
___
**[on host PC]** Create a sysroot. Using rsync we can properly keep things synchronized in the future as well.
Replace **raspberrypi.local** with the IP address of the Pi.
```
mkdir sysroot sysroot/usr sysroot/opt
rsync -avz pi@raspberrypi.local:/lib sysroot
rsync -avz pi@raspberrypi.local:/usr/include sysroot/usr
rsync -avz pi@raspberrypi.local:/usr/lib sysroot/usr
rsync -avz pi@raspberrypi.local:/opt/vc sysroot/opt
```
這邊會花很多時間，可以先去泡個茶。
___

**[on host PC]** Create correct symlinks to "missing" libraries
```
mv sysroot/usr/lib/arm-linux-gnueabihf/libEGL.so.1.0.0 sysroot/usr/lib/arm-linux-gnueabihf/libEGL.so.1.0.0_backup
ln -s sysroot/opt/vc/lib/libEGL.so sysroot/usr/lib/arm-linux-gnueabihf/libEGL.so.1.0.0
```
```
mv sysroot/usr/lib/arm-linux-gnueabihf/libGLESv2.so.2.0.0 sysroot/usr/lib/arm-linux-gnueabihf/libGLESv2.so.2.0.0_backup
ln -s sysroot/opt/vc/lib/libGLESv2.so sysroot/usr/lib/arm-linux-gnueabihf/libGLESv2.so.2.0.0
```
```
ln -s sysroot/opt/vc/lib/libEGL.so sysroot/opt/vc/lib/libEGL.so.1
```
```
ln -s sysroot/opt/vc/lib/libGLESv2.so sysroot/opt/vc/lib/libGLESv2.so.2
```
___
**[on host PC] **Adjust symlinks to be relative. Use provided script, because the old fixQualifiedLibraryPaths is not working properly:
```
wget https://raw.githubusercontent.com/riscv/riscv-poky/priv-1.10/scripts/sysroot-relativelinks.py
chmod +x sysroot-relativelinks.py
./sysroot-relativelinks.py sysroot
```
___
> Get qtbase and configure Qt. The target directory is /usr/local/qt5pi on the Pi, the host tools like qmake will go to ~/raspi/qt5, while make install will target ~/raspi/qt5pi (this is what we will sync to the device).
Don't forget to adjust paths if you changed that. For some reason the ~/ in the paths may not work, if this the case just use full paths.

> You need to change < qt-version> with a proper Qt version (for example 5.6, or 5.9.1; note that version 5.9.1 is a tag not a branch, so you may want to create a local branch with it)

> You need to change < rpi-version> with a proper Raspberry Pi version. Use: linux\-rasp\-pi\-g\+\+ for RPi, linux\-rasp\-pi2\-g\+\+ for RPi2 and linux-rasp-pi3-g\+\+ for RPi3.

**[on host PC] ** Get Qt sources and untar Qt sources
```
wget https://download.qt.io/official_releases/qt/5.10/5.10.1/single/qt-everywhere-src-5.10.1.tar.xz
```
```
tar xf qt-everywhere-src-5.10.1.tar.xz
```

**[on host PC] ** Create build dir and configure source
如果後續make有問題或要重編，直接砍掉qt5buid就好‧
```
mkdir qt5build
cd qt5build
```

```
../qt-everywhere-src-5.10.1/configure -opengl es2 -device linux-rasp-pi3-g++ -device-option CROSS_COMPILE=arm-linux-gnueabihf- -sysroot ~/raspi/qt5pi/sysroot -prefix /usr/local/qt5pi -opensource -confirm-license -nomake examples -make libs -no-use-gold-linker -no-icu -skip qtwebengine -skip wayland -skip qtscript -v
```
Sadly QtWebengine dont compile on Raspberry Pi 1 (maybe also Pi 2), so we have to skip it. Also skip QtScript, its deprecated.

**[on host PC] **Make
```
make
make install
```

*==到這邊都沒問題的話，恭喜您突破第一道難關~==*


###Upload to raspberry pi and update link
**[on host PC] **Upload to raspberry pi
```
cd ~/raspi
rsync -avz sysroot/usr/local/qt5pi pi@raspberrypi.local:/usr/local
```
**[on RPi]** Update the device to let the linker find the Qt libs:
```
echo /usr/local/qt5pi/lib | sudo tee /etc/ld.so.conf.d/qt5pi.conf
sudo ldconfig
```
**[on RPi] **Fix the EGL/GLES libraries. The device may have the Mesa version of libEGL and libGLESv2 in /usr/lib/arm-linux-gnueabihf, resulting Qt apps picking these instead of the real thing from /opt/vc/lib. This may be fine for X11 desktop apps not caring about OpenGL performance but is totally useless for windowing system-less, fullscreen embedded apps. You may want to save the originals somewhere, just in case. Make sure you're in "/home/pi" aka "~" when you run these commands:
```
sudo mv /usr/lib/arm-linux-gnueabihf/libEGL.so.1.0.0 /usr/lib/arm-linux-gnueabihf/libEGL.so.1.0.0_backup
sudo mv /usr/lib/arm-linux-gnueabihf/libGLESv2.so.2.0.0 /usr/lib/arm-linux-gnueabihf/libGLESv2.so.2.0.0_backup
sudo ln -s /opt/vc/lib/libEGL.so /usr/lib/arm-linux-gnueabihf/libEGL.so.1.0.0
sudo ln -s /opt/vc/lib/libGLESv2.so /usr/lib/arm-linux-gnueabihf/libGLESv2.so.2.0.0
```
Please make sure to also add missing symbolic links:
```
sudo ln -s /opt/vc/lib/libEGL.so /opt/vc/lib/libEGL.so.1
sudo ln -s /opt/vc/lib/libGLESv2.so /opt/vc/lib/libGLESv2.so.2
```

##Qt configuration
Tools->Options...->Devices->Add
Generic Linux Device
Edit Hostname, authentication type key, username and add private key ( ~/.ssh/id_rsa )

Tools->Build&Run->Debuggers->Add
Edit name and path
```
 ~/raspi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf-gdb
```
Tools->Compilers->Add->GCC->C
Edit name and path
```
~/raspi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf-gcc
```
Tools->Compilers->Add->GCC->C++
Edit name and path
```
~/raspi/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf-g++ )
```
Tools->Qt Versions->Add
Choose
```
~/raspi/sysroot/usr/local/qt5pi/bin/qmake
```

Tools->Kits->Add
Edit name
Device type: Generic Linux Device
Device: Choose prior created raspberry pi device
Sysroot: Choose /opt/qt5pi/sysroot
C and C++: Choose prior created raspberry pi compilers
Debugger: Choose prior created raspberry pi debugger
Qt version: Choose prior created raspberry pi qt version

***==Getting Started==***
---
