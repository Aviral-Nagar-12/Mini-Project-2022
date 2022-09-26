# Mini-Project-2022

# A Makefile for Arduino Sketches Build Status.

This is a very simple Makefile which knows how to build Arduino sketches. It defines entire workflows for compiling code, flashing it to Arduino and even communicating through Serial monitor. You don't need to change anything in the Arduino sketches.

# Features.

Very robust
Highly customizable
Supports all official AVR-based Arduino boards
Supports official ARM-based Arduino boards using Atmel SAM chip family and includes on-device debugging targets.
Supports chipKIT
Supports Teensy 3.x (via Teensyduino)
Supports Robotis OpenCR 1.0
Works on all three major OS (Mac, Linux, Windows)
Auto detects serial baud rate and libraries used
Support for *.ino and *.pde sketches as well as raw *.c and *.cpp
Support for Arduino Software versions 0.x, 1.0.x, 1.5.x and 1.6.x except 1.6.2. We recommend 1.6.3 or above version of Arduino IDE.
Automatic dependency tracking. Referred libraries are automatically included in the build process. Changes in *.h files lead to recompilation of sources which include them.



# Installation.

Through package

Using apt-get (or aptitude)

If you're using FreeBSD, Debian, Raspbian or Ubuntu, you can find this in the arduino-mk package which can be installed using apt-get or aptitude.

sudo apt-get install arduino-mk
homebrew (or linuxbrew)

If you're using homebrew (or linuxbrew) then you can find this in the arduino-mk package which can be installed using the following commands.

Also make sure you have the necessary dependencies installed. Refer to the Requirements section below to install the dependencies.

# Add tap.
$ brew tap sudar/arduino-mk

# To install the last stable release.
$ brew install arduino-mk

# To install the development version.
$ brew install --HEAD arduino-mk
Arch Linux

Arch Linux users can use the unofficial AUR package arduino-mk. It can be installed with [AUR] helper using the following command.

yay -S arduino-mk
Fedora

Fedora Linux users can use our packaging instructions here to build an RPM.

From source

# Download the latest release.
Or clone it from Github using the command git clone git@github.com:sudar/Arduino-Makefile.git
Check the usage section in this readme about setting usage options
Requirements

# Arduino IDE.

You need to have the Arduino IDE. You can either install it through the installer or download the distribution zip file and extract it.

# pySerial.

The Makefile also delegates resetting the board to a short Python program. You'll need to install pySerial to use it though.

On most systems you should be able to install it using either pip3 or easy_install3.

pip3 install pyserial

# Or if you prefer easy_install.

easy_install3 -U pyserial
If you prefer to install it as a package, then you can do that as well.

On Debian or Ubuntu:

apt-get install python3-serial
On Fedora:

dnf install python3-pyserial
On openSUSE:

zypper install python3-serial
On Arch:

sudo pacman -S python-pyserial
On macOS using Homebrew (one can install to System Python but this is not recommend or good practice):

brew install python
pip3 install pyserial
On Windows:

You need to install Cygwin and its packages for Make, Perl, Python3 and the following Serial library.

# Assuming you included Python in your Cygwin installation:

download PySerial source package from https://pypi.python.org/pypi/pyserial
extract downloaded package running tar xvzf dowloaded_package_name.tar.gz
navigate to extracted package folder
build and install Python module:
python3 setup.py build
python3 setup.py install
Alternatively, if you have setup Cygwin to use a Windows Python installation, simply install using pip:

pip3 install pyserial
Arduino-Makefile should automatically detect the Python installation type and use the correct device port binding.

# Usage.

Download a copy of this repo somewhere to your system or install it through a package by following the above installation instruction.

Sample makefiles are provided in the examples/ directory. E.g. Makefile-example demonstrates some of the more advanced options, whilst Blink demonstrates the minimal settings required for various boards like the Uno, Nano, Mega, Teensy, ATtiny etc.

# Mac.

On the Mac with IDE 1.0 you might want to set:

    ARDUINO_DIR   = /Applications/Arduino.app/Contents/Resources/Java
    ARDMK_DIR     = /usr/local
    AVR_TOOLS_DIR = /usr
    MONITOR_PORT  = /dev/ttyACM0
    BOARD_TAG     = mega2560
On the Mac with IDE 1.5+ it's like above but with

    ARDUINO_DIR   = /Applications/Arduino.app/Contents/Java
# Linux.

You can either declare following variables in your project's makefile or set them as environmental variables.

    ARDUINO_DIR – Directory where Arduino is installed
    ARDMK_DIR – Directory where you have copied the makefile
    AVR_TOOLS_DIR – Directory where avr tools are installed
Keep in mind, that Arduino 1.5.x+ comes with it's own copy of avr tools which you can leverage in your build process here.

Example of ~/.bashrc file:

    export ARDUINO_DIR=/home/sudar/apps/arduino-1.0.5
    export ARDMK_DIR=/home/sudar/Dropbox/code/Arduino-Makefile
    export AVR_TOOLS_DIR=/usr/include
Example of the project's make file:

    BOARD_TAG     = mega2560
    MONITOR_PORT  = /dev/ttyACM0
# Windows.

On Windows (using Cygwin), you might want to set:

    # Symbolic link to Arduino installation directory - see below
    ARDUINO_DIR   = C:/Arduino
    ARDMK_DIR     = path/to/mkfile
    MONITOR_PORT  = com3
    BOARD_TAG     = mega2560
## NOTE: Use forward slash not backslash and there should be no spaces or special characters in the Windows paths (due to Win/Unix crossover). The paths should not be cygdrive paths.

On Windows (using MSYS and PuTTY), you might want to set the following extra parameters:

    MONITOR_CMD   = putty
    MONITOR_PARAMS = 8,1,n,N
On Arduino 1.5+ installs, you should set the architecture to either avr or sam and if using a submenu CPU type, then also set that:

    ARCHITECTURE  = avr
    BOARD_TAG     = atmegang
    BOARD_SUB     = atmega168
# Symbolic Link.

It is recommended in Windows that you create a symbolic link to avoid problems with file naming conventions on Windows; unless one installs to a non-default location. For example, if your your Arduino directory is in:

C:\Program Files (x86)\Arduino
You will get problems with the special characters on the directory name. More details about this can be found in issue #94

To create a symbolic link, you can use the command “mklink” on Windows, e.g.

mklink /d C:\Arduino C:\Program Files (x86)\Arduino
Alternatively if you've setup Cygwin hard symbolic links (CYGWIN=winsymlinks:native):

ln -s /cygdrive/c/Program Files\ \(x86\)/Arduino/ C:/Arduino
After which, the variables should be:

    ARDUINO_DIR=C:/Arduino
Instead of:

    ARDUINO_DIR=C:/Program\ Files\ \(x86\)/Arduino
Useful Variables

The list of all variables that can be overridden is available at arduino-mk-vars.md file.

BOARD_TAG - Type of board, for a list see boards.txt or make show_boards
MONITOR_PORT - The port where your Arduino is plugged in, usually /dev/ttyACM0 or /dev/ttyUSB0 in Linux or Mac OS X and com3, com4, etc. in Windows.
ARDUINO_DIR - Path to Arduino installation. Using Windows with Cygwin, this path must use Unix / and not Windows \ (eg "C:/Arduino" not "C:\Arduino).
ARDMK_DIR - Path where the *.mk are present. If you installed the package, then it is usually /usr/share/arduino. On Windows, this should be a path without spaces and no special characters, it can be a cygdrive path if necessary and must use / not \.
AVR_TOOLS_DIR - Path where the avr tools chain binaries are present. If you are going to use the binaries that came with Arduino installation, then you don't have to set it. Otherwise set it relative and not absolute.
Including Libraries

You can specify space separated list of libraries that are needed for your sketch in the variable ARDUINO_LIBS.

	ARDUINO_LIBS = Wire SoftwareSerial
The libraries will be searched for in the following places in the following order.

/libraries directory inside your sketchbook directory. Sketchbook directory will be auto detected from your Arduino preference file. You can also manually set it through ARDUINO_SKETCHBOOK.
/libraries directory inside your Arduino directory, which is read from ARDUINO_DIR.
The libraries inside user directories will take precedence over libraries present in Arduino core directory.

The makefile can autodetect the libraries that are included from your sketch and can include them automatically. But it can't detect libraries that are included from other libraries. (see issue #93)

avrdude

To upload compiled files, avrdude is used. This Makefile tries to find avrdude and its config (avrdude.conf) below ARDUINO_DIR. If you like to use the one installed on your system instead of the one which came with Arduino, you can try to set the variables AVRDUDE and AVRDUDE_CONF. On a typical Linux system these could be set to

      AVRDUDE      = /usr/bin/avrdude
      AVRDUDE_CONF = /etc/avrdude.conf



# Automatic Curtain Opener.

The automatic curtain opener allows curtain to be opened and closed: 
* Remotely 
* At specific times 
* Automatically at dawn and dusk 

Thus allowing users to:
* Wake up naturally using the light of the sun 
* Appear at home, when away travelling

This real-time project utilises a Raspberry Pi 3, light and proximity sensors, a motorised pulley system, custom PCB boards, and a web GUI. 

More details about the hardware and software can be found in the [Wiki](https://github.com/LaurenJA/automatic-curtain/wiki.

### System Overview 

Below is an simple overview of how the deployed sensors and motor could be positioned on the curtain pole. 


### Build Instructions 

The code related to this project can be found in the code folder.

#### Requirements 
CMake version 3.10 

Python version 3.6

Django version 2.11.1

#### Build 
Compile the project 
``` 
make
```

To run the program
``` 
sudo ./server 
```

To open the folder of the web app(which is named as mysite in this project).
```
cd ...
```

Then run the server.
```
python3 manage.py runserver
```

### Project Evaluation

#### Sampling rate
Under the Auto model, the status will be checked every 0.2 seconds, so the sampling rate should be 5 times per seconds.
#### Bus Protocol
The interface of the light sensor:OPT3001 and proximity sensor:APDS9960 are I2C, so use the I2C bus protocol.
#### Low level implementation: kernel or user space and why?
User space. As this can provided fast enough sampling and processing, without having to modeify the kernel. 
#### Data flow from hardware to GUI to output with data formats, latencies,processing and conversions.
Not provided
#### Buffering of data: how many samples?
The buffering of data is 8k, which is enough for this project.
#### How does the buffering impact on the realtime performance and how much latency is introduced?
For the large amount of data, the buffering of data should be higher, however,for our project, the ultra-small amout of date has no influence on realtime performance.
#### How many threads are needed and/or how can the load be distributed to allow a responsive application?
There are two threads, one is for communication with GUI, and one if to achieve function of the command.
#### Structure of the software in classes, associated unit tests to turn it into reliable software
The required software is provided within the code folder. This can be compiled and executed on the Raspberry pi by using the following command: make ./server within the code directory. And the test code also can be found in the test folder.
Overview of the software can be found in the wiki
#### Team structure: which roles in a team of 3 and how equal contributions are guaranteed?
The team members and roles are as follow:
Lauren Astell: Hardware (including PCB Design)
Jing Wang: Code of light sensor,proximity sensor,motor controller and main function implementaion and test.
Jianan Kan:GUI Implementation
#### How much time is allocated to hard, software and debugging and how is this interleaved?
Every memeber will do their specific work, and control the process by themselves. The final work achieved by Lauren.
#### Which version control software is used? How are releases / milestones done and numbered?
GIT was used. Versions are number vX.Y, where X is a major change and Y is a minor. 
The project is currently at realease v1.0
#### What is the release strategy / publication / publicity? How is that measured and deemed to be successful?
To release the project, a release was created on the GitHub so any user that wishes to create this project themselves can download it. 
####  Measurement of success of the application in the context of the problem given and evaluation of it.

#### How to implement the GUI? Permitted refresh rate and latency resulted from it.
To use the web page as the GUI, the web page is coded in django. Everyone who get the website can control the curtain in the same network. The instruction can be sent by GUI.
