# Non-planar Slic3r in WSL for Windows 10
Notes on configuring the non-planar slic3r variant in Windows Subsystem for Linux (WSL)

This document describes how to get the non-planar fork of Slic3r working on a Windows 10 machine using the Windows Subsystem for Linux (WSL). WSL allows many Linux applications to run using a number of different Linux distributions. Here, we use the Ubuntu distribution available in the Windows Store, simply to ensure a consistent starting point.

WSL does not readily support graphics applications natively, so we use an "XServer" running in the Windows environment to allow Slic3r to operate in GUI mode. Once more, for a consistent starting point, this document uses VcXsrv as the X server, but in principle, any X server would yield the same result.

## Why?
An implementation of non-planar slicing was recently contributed to Slic3r. Non-planar slicing exploits continuous 3-axis movement of contemporary 3D printers to reduce the layer-step artefacts of most slicing software, resulting in more natural surfaces.

However, this implementation needs to be built and compiled in order to run it, hence these instructions for those who are keen to try, but less familiar with these tasks.

## How?
Described here is how to enable and install a Linux-based operating system into a current version of Windows 10 and then install and configure slic3r within it. It will display it's graphical UI inside the window of an XServer application.

### Step 1 - Install an XServer
Although any compliant XServer should work, I've tried and tested using the VcXsrv implementation. Download it from here: https://sourceforge.net/projects/vcxsrv/. It should install like almost any other piece of software, but make sure that you grant it access to the local network (if asked).
When I grabbed it, the page looked like this:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(20).png )

When it's downloaded, just run the installer:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(21).png )

I ran with the defaults, but no reason you can't decided where you want it to go:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(22).png )

Nothing else to do on this front, we'll come back to it later.

### Step 2 - Enable the WSL feature in Windows 10
There's a few screens to bounce through here, but nothing too complicated, just clicking links and boxes.

Firstly, launch the control panel:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(3).png )
Choose "Apps", but note that depending on your screen size, it may appear in a different place and then "Programs and Features"
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(4).png )
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(5).png )
Select the link over on the left that says "Turn Windows features on or off" and this should pop up:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(6).png )
Select the option for "Windows Subsystem for Linux" (if not already selected):
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(7).png )

Things will whirr and eventually you'll be asked to restart. Probably not a bad idea:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(8).png )

After the restart, you now have WSL activated, but no Linux installed just yet.

### Step 3 - Install a Linux distribution
Linux operating systems come in many flavours, and some compatible ones are available in the Windows Store. As described above, I've used the Ubuntu distribution as a reference. Other distributions can be made to work, but the following instructions may be incomplete for them.

Launch the Windows Store app on your Windows 10 computer. Search for "Ubuntu" and install it. As of the time of writing this, it was about a 200MB download, so download/install times will vary.
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(9).png )

After it's installed, press the "Launch" button:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(10).png )
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(11).png )

The first time you launch it, you will be asked to nominate a user name and a password. I strongly advocate that you remember these :-) You will need to enter the password for commands that appear below.
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(12).png )
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(13).png )

Congratulations, you now have a working Linux subsystem in Windows 10!

You'll have a console-looking window with a "$" prompt. That's your command shell and here you can type (or cut-and-paste) the commands below. Follow the steps carefully and you should succeed in getting a running non-planar slicer.

After installing and starting Ubuntu in WSL, it's important to refresh its catalogue of available software. Other steps will fail if this isn't done.
```
sudo apt-get update
```
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(15).png )

There shouldn't be anything that looks like an error message. If there is, best hit up your favourite search engine.

### Step 4 - Download pre-requisite components

This next command downloads all of the pre-requisite software components that will allow the build to proceed. This step will take a few minutes, depending on the speed of your computer and Internet connection.

```
sudo apt-get install gcc cmake build-essential libgtk2.0-dev libwxgtk3.0-dev libwx-perl libmodule-build-perl git cpanminus libextutils-cppguess-perl libboost-all-dev libxmu-dev liblocal-lib-perl wx-common libopengl-perl libwx-glcanvas-perl libtbb-dev libxmu-dev freeglut3-dev libwxgtk-media3.0-dev libboost-thread-dev libboost-system-dev libboost-filesystem-dev libcurl4-openssl-dev libextutils-makemaker-cpanfile-perl
```

This can take some time (10-20 minutes) so don't stress and this is a reasonable time to do something else. Also, a decent amount of data will get downloaded. In my case, close to 1GB:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(17).png )

Once it's downloaded, the updates will be applied automatically. If you get a prompt like this, saying "Yes" is a good option:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(18).png )

### Step 4 - Download the slicer software

Now, we grab the code for the slicer itself. This is done using git, which talks to the repository on github:
```
git clone https://github.com/Zip-o-mat/Slic3r.git
```
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(24).png )

### Step 5 - Prepare the build

This will have unpacked a directory tree into the directory where you started. The build now begins in a series of discrete steps:
```
cd Slic3r/
perl Build.PL
perl Build.PL --gui
```
The above steps will take a while and a number of warnings will likely be displayed, but don't despair!
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(26).png )
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(29).png )

### Step 6 - Final build
So far, we've prepared the libraries and environment for the last few build steps.
```
mkdir build
cd build
cmake ..
```
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(30).png )
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(32).png )
Don't be surprised to see quite a few warning messages again, but these are normal.

Here comes the heavy lifting, where the code is turned into usable programs:

```
export PERL5LIB=~/Slic3r/local-lib/lib/perl5
make
```
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(34).png )

At the end, you hopefully see something like this:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(35).png )

The code should now be compiled and just about ready to run!

### Step 7 - Launch the X Server

VcXsrv usually puts an "XLaunch" shortcut somewhere, so start that. You'll get a series of popups asking configuration questions. With a bit more experience, feel free to play with these, but for now, the following options work pretty well:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(36).png )
This means just run one window on your desktop for multiple apps to share.
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(37).png )
This just says you'll decide in a little while what to run.
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(38).png )
Just take the defaults here
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(39).png )
Press "Finish" and you're done.
You may (depending on your Windows configuration) get a popup like this:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(40).png )
Best "allow access", otherwise software won't be able to talk to it.

Let's install a window manager (in the X Window System, the movement of windows is a separate piece of functionality).
```
sudo apt-get install mwm
```
This next command tells the programs where to send their output (it's inherent that programs can work across networked displays)
```
export DISPLAY=localhost:0
```
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(41).png )

Launch the window manager in the background:
```
mwm&
```
Move up one directory level and launch the slicer:
```
cd ..
perl slic3r.pl --gui
```
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(42).png )

All being well, Slicr3 should appear in the window running the XServer:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(43).png )

yay.

### A word about files

All of your normal Windows files are available from the WSL environment.

Each drive on your PC is automatically placed into the WSL filesystem tree under "/mnt/<drive>".

So, to load a file from your "Downloads" directory, you can navigate to "/mnt/c/users/<yourname>/Downloads"
  
You can read and write files this way, so loading STL files and writing gcode is simple.

### Slic3r configuration

You'll need to setup Slic3r the first time it runs. Get the bed size, nozzle size and filament size correct, otherwise you will have problems. I won't go into printer-specific issues here, since are well covered elsewhere.

I'm no expert on the non-planar configuration, but the settings are found in the "Print settings" tab. Here's an example screenshot:
![image](https://github.com/grandaspanna/Non-planar-Slic3r-in-WSL-for-WIndows-10/blob/master/images/Screenshot%20(52).png )

### Wrap-up

I hope this has been useful!
