# Non-planar-Slic3r-in-WSL-for-WIndows-10
Notes on configuring the non-planar slic3r variant in Windows Subsystem for Linux (WSL)

This document describes how to get the non-planar fork of Slic3r working on a Windows 10 machine using the Windows Subsystem for Linux (WSL). WSL allows many Linux applications to run using a number of different Linux distributions. Here, we use the Ubuntu distribution available in the Windows Store, simply to ensure a consistent starting point.

WSL does not readily support graphics applications natively, so we use an "XServer" running in the Windows environment to allow Slic3r to operate in GUI mode. Once more, for a consistent starting point, this document uses VcXsrv as the X server, but in principle, any X server would yield the same result.

Why?
An implementation of non-planar slicing was recently contributed to Slic3r. Non-planar slicing exploits continuous 3-axis movement of contemporary 3D printers to reduce the layer-step artefacts of most slicing software, resulting in more natural surfaces.

However, this implementation is 

https://sourceforge.net/projects/vcxsrv/


sudo apt-get install gcc cmake build-essential libgtk2.0-dev libwxgtk3.0-dev libwx-perl libmodule-build-perl git cpanminus libextutils-cppguess-perl libboost-all-dev libxmu-dev liblocal-lib-perl wx-common libopengl-perl libwx-glcanvas-perl libtbb-dev libxmu-dev freeglut3-dev libwxgtk-media3.0-dev libboost-thread-dev libboost-system-dev libboost-filesystem-dev libcurl4-openssl-dev libextutils-makemaker-cpanfile-perl

git clone https://github.com/Zip-o-mat/Slic3r.git

