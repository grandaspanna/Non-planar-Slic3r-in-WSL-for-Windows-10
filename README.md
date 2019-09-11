# Non-planar Slic3r in WSL for Windows 10
Notes on configuring the non-planar slic3r variant in Windows Subsystem for Linux (WSL)

This document describes how to get the non-planar fork of Slic3r working on a Windows 10 machine using the Windows Subsystem for Linux (WSL). WSL allows many Linux applications to run using a number of different Linux distributions. Here, we use the Ubuntu distribution available in the Windows Store, simply to ensure a consistent starting point.

WSL does not readily support graphics applications natively, so we use an "XServer" running in the Windows environment to allow Slic3r to operate in GUI mode. Once more, for a consistent starting point, this document uses VcXsrv as the X server, but in principle, any X server would yield the same result.

## Why?
An implementation of non-planar slicing was recently contributed to Slic3r. Non-planar slicing exploits continuous 3-axis movement of contemporary 3D printers to reduce the layer-step artefacts of most slicing software, resulting in more natural surfaces.

However, this implementation is 

https://sourceforge.net/projects/vcxsrv/

After installing and starting Ubuntu in WSL, it's important to refresh its catalogue of available software. Other steps will fail if this isn't done.
```
sudo apt-get update
```
This next command downloads all of the pre-requisite software components that will allow the build to proceed. This step will take a few minutes, depending on the speed of your computer and Internet connection.

```
sudo apt-get install gcc cmake build-essential libgtk2.0-dev libwxgtk3.0-dev libwx-perl libmodule-build-perl git cpanminus libextutils-cppguess-perl libboost-all-dev libxmu-dev liblocal-lib-perl wx-common libopengl-perl libwx-glcanvas-perl libtbb-dev libxmu-dev freeglut3-dev libwxgtk-media3.0-dev libboost-thread-dev libboost-system-dev libboost-filesystem-dev libcurl4-openssl-dev libextutils-makemaker-cpanfile-perl
```
Now, we grab the code for the slicer itself:
```
git clone https://github.com/Zip-o-mat/Slic3r.git
```

The build now begins in a series of discrete steps:
```
cd Slic3r/
perl Build.PL
perl Build.PL --gui
```
The above steps will take a while and a number of warnings will likely be displayed, but don't despair!

So far, we've prepared the libraries and environment for the last few build steps.
```
mkdir build
cd build
cmake ..
```
Don't be surprised to see quite a few warning messages again, but these are normal.
```
export PERL5LIB=~/Slic3r/local-lib/lib/perl5
make
```
The code should now be compiled and just about ready to run!

Let's install a window manager (in the X Window System, the movement of windows is a separate piece of functionality).
```
sudo apt-get install mwm
```
This next command tells the programs where to send their output (it's inherent that programs can work across networked displays)
```
export DISPLAY=localhost:0
```

Launch the window manager in the background:
```
mwm&
```
Move up one directory level and launch the slicer:
```
cd ..
perl slic3r.pl --gui
```
