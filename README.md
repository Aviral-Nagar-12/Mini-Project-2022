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

# Project Evaluation

# How to Automate Your Curtains With Arduino…

This Arduino Curtain Automation project will let you automate your curtain blinds using just an Arduino and a stepper motor. I just moved to America pursuing my higher studies. Because my dorm is very dark, I wanted to brighten up my room when I wanted to. Since I would be there for only for a month, I couldn't make anything too complicated. So I decided to make myself this DIY Arduino Curtain Automation system. Using regular curtain blinds, I could control the amount of light in the room and suit it according to my needs.


# The design for this Arduino Curtain Automation system is quite simple and there are two ways to activate the curtains:

Using a sound sensor (mic) to control it using my claps (Step 2).

Using buttons to move the shades of the curtain (Step 3).

So letâ€™s get started with our DIY Arduino Curtain Automation system, follow the simple instructions to replicate this project.


## How Does the Arduino Curtain Automation Work?


The working of this DIY Arduino Curtain Automation system is very simple. The Arduino takes input from either the sound sensor (mic) or buttons. It then correspondingly controls the stepper motor through the Darlington array driver for the motor. The stepper motor is attached to the control stick of the curtain blinds and thus on rotation, opens/closes the blinds.

## Interfacing the Stepper Motor


The first step I did was testing the design for the Arduino curtain automation system. The motor is connected to the 4 wires of the driver. Depending on the direction of rotation for your curtain, you need to plug the driver accordingly.

Basically, the first button from the right will activate the motor to spin a particular degree (you can change in the code). Press the button 4 times and it will return to the original place since it will spin around 90 degrees during each press. The middle button will lock the circuit so that the first button cannot activate the motor. The LED will turn ON when the motor is locked. The final button will return the motor to the original place no matter where is it, at the time pressed. It's still a simple program, hope you guys understand. You can find the code below.

int pin[8]={2,3,4,5,6,7,8,9};
int steps[][4] = 
{
{HIGH,HIGH,LOW,LOW},
{HIGH,LOW,LOW,HIGH},
{LOW,LOW,HIGH,HIGH},
{LOW,HIGH,HIGH,LOW},
} ;

int numofroun=1; //Change accordingly to your needs

int current=1;
int type=3;
int place=0;

int lastLockState = LOW;   
long lastLockTime = 0;
int LockState;
int Lockreading;
bool lock=true;

int lastPauseState = LOW;   
long lastPauseTime = 0;
int PauseState;
int Pausereading;
bool Pauseled=false;
bool pause=false;

int lastReturnState = LOW;   
long lastReturnTime = 0;
int ReturnState;
int Returnreading;

void setup() {
for (int num=0; num<5; num++) pinMode(pin[num],OUTPUT);
for (int num=5; num<8; num++) pinMode(pin[num],INPUT);
}

void reset(){
for(int num=0;num<4;num++) digitalWrite(pin[num],LOW);
}

void stepper()
{
for (int num=0; num<4;num++) { digitalWrite(pin[num],steps[abs(type-current)][num]);} if(type==0) {++place;} if(type==3) {--place;} delay(2); } void button1() { Lockreading = digitalRead(pin[5]); if (Lockreading != lastLockState) { lastLockTime = millis(); } if ((millis() - lastLockTime) > 50)
{
if (Lockreading != LockState) {
LockState = Lockreading;
if (LockState == HIGH) {
lock=false;        
if ((place!=1536*numofroun)&&(place!=1024*numofroun)&&(place!=512*numofroun)) {type=abs(type-3);}
}
}
}
lastLockState = Lockreading;
}

void button2()
{
Pausereading = digitalRead(pin[6]);
if (Pausereading != lastPauseState) 
{
lastPauseTime = millis();
}
if ((millis() - lastPauseTime) > 50)
{
if (Pausereading != PauseState) {
PauseState = Pausereading;
if (PauseState == HIGH) {
Pauseled=!Pauseled;
pause=!pause;
if (Pauseled) {digitalWrite(pin[4],HIGH);}
if (!Pauseled) {digitalWrite(pin[4],LOW);}
}
}
}
lastPauseState = Pausereading;
}

void button3()
{
Returnreading = digitalRead(pin[7]);
if (Returnreading != lastReturnState) 
{
lastReturnTime = millis();
}
if ((millis() - lastReturnTime) > 50)
{
if (Returnreading != ReturnState) {
ReturnState = Returnreading;
if (ReturnState == HIGH) {
type=3;
while (place>0)
{
for (int num=0; num<4;num++) { 
digitalWrite(pin[num],steps[3-current][num]);}
--place;
if (current==3) {current=0;}
else ++current;
delay(2);
}
reset();
}
}
}
lastReturnState = Returnreading;
}

void loop() {
if (lock==true) {button2();button3();}
if (!pause)
{
if (lock==true) {button1();}
if (lock==false) {stepper();}
if ((place==2048)or(place==0)or(((place==1536*numofroun)or(place==1024*numofroun)or(place==512*numofroun))&&(type==3)))
{lock=true;reset();}
if (current==3) {current=0;}
else ++current;
}
}
Setting Up the Arduino Curtain Automation System


After testing the stepper motor, you can use the connection diagram above to make the final prototype. After I finished making mine, I simply changed the input (button) into a sound sensor. You can find the code below.

int pin[8]={2,3,4,5,6,7,8,9};
int steps[][4] = 
{
{HIGH,HIGH,LOW,LOW},
{HIGH,LOW,LOW,HIGH},
{LOW,LOW,HIGH,HIGH},
{LOW,HIGH,HIGH,LOW},
} ;

float numofroun=4.5; //Change accordingly to your needs

int current=1;
int type=3;
int place=0;

int claps = 0;
long detectionSpanInitial = 0;
long detectionSpan = 0;
long spancondition;
bool spanconditioncheck=false;

bool lock=true;

int lastPauseState = LOW;   
long lastPauseTime = 0;
int PauseState;
int Pausereading;
bool Pauseled=false;
bool pause=false;

int lastReturnState = LOW;   
long lastReturnTime = 0;
int ReturnState;
int Returnreading;

void setup() {
for (int num=0; num<5; num++) pinMode(pin[num],OUTPUT);
for (int num=5; num<8; num++) pinMode(pin[num],INPUT);
}

void reset(){
for(int num=0;num<4;num++) digitalWrite(pin[num],LOW);
}


void stepper()
{
for (int num=0; num<4;num++) { digitalWrite(pin[num],steps[abs(type-current)][num]);} if(type==0) {++place;} if(type==3) {--place;} delay(2); } void sound() { int sensorState = digitalRead(pin[5]); if (sensorState == 0){ if (claps == 0){ detectionSpanInitial = detectionSpan = millis(); claps++; } else if (claps > 0 && millis()-detectionSpan >= 50){
detectionSpan = millis();
claps++;
}
}
if (millis()-detectionSpanInitial >= 400)
{
if (claps == 2)
{
lock=false;        
if ((place!=1024*numofroun)&&(place!=512*numofroun)) {type=abs(type-3);}
spancondition=millis();
}
claps = 0;
}
}

void button1()
{
Pausereading = digitalRead(pin[6]);
if (Pausereading != lastPauseState) 
{
lastPauseTime = millis();
}
if ((millis() - lastPauseTime) > 50)
{
if (Pausereading != PauseState) {
PauseState = Pausereading;
if (PauseState == HIGH) {
Pauseled=!Pauseled;
pause=!pause;
spancondition=millis();
if (Pauseled) {digitalWrite(pin[4],HIGH);}
if (!Pauseled) {digitalWrite(pin[4],LOW);}
}
}
}
lastPauseState = Pausereading;
}

void button2()
{
Returnreading = digitalRead(pin[7]);
if (Returnreading != lastReturnState) 
{
lastReturnTime = millis();
}
if ((millis() - lastReturnTime) > 50)
{
if (Returnreading != ReturnState) {
ReturnState = Returnreading;
if (ReturnState == HIGH) {
type=3;
while (place>0)
{
for (int num=0; num<4;num++) { digitalWrite(pin[num],steps[3-current][num]);} --place; if (current==3) {current=0;} else ++current; delay(2); } reset(); spancondition=millis(); } } } lastReturnState = Returnreading; } void loop() { if (lock==true) {button1();button2();} if (!pause) { if ((lock==true)&&(millis()-spancondition>1000)) {sound();}
if (lock==false) {stepper();spanconditioncheck=false; }
if ((place==2048*numofroun)or(place==0)or(((place==1024*numofroun)or(place==512*numofroun))&&(type==3))){
lock=true;
reset();
if (!spanconditioncheck){
spancondition=millis();
spanconditioncheck=true;
}
}
if (current==3) {current=0;}
else ++current;
}
}

I used foam board to make the holder for the motor (1, 2) and curtain handle (3) on my Arduino curtain automation system. Because my dorm doesn't allow modding the wall, I later used double sided  tape to stick it in place (1).


The pause button is there so that if your room gets too loud, you can lock it so that the blinds won't go crazy. Note that you might have  to adjust the potentiometer on the sound sensor to adjust the sensitivity.


Alternate Version Using Buttons


If you don't like sound control, you can simply use a button. In this Arduino curtain automation design, there are only two buttons: activate and return (since the pause isn't needed). The activate is the same as before; the reset button will return the curtain back to the original place while activating the LED. You can find the code for this version here.

int pin[8]={2,3,4,5,6,7,8};
int steps[][4] = 
{
{HIGH,HIGH,LOW,LOW},
{HIGH,LOW,LOW,HIGH},
{LOW,LOW,HIGH,HIGH},
{LOW,HIGH,HIGH,LOW},
} ;
int current=1;
int type=3;
int place=0;

int lastLockState = LOW;   
long lastLockTime = 0;
int LockState;
int Lockreading;
bool lock=true;

int lastReturnState = LOW;   
long lastReturnTime = 0;
int ReturnState;
int Returnreading;

void setup() {
for (int num=0; num<5; num++) pinMode(pin[num],OUTPUT);
for (int num=5; num<7; num++) pinMode(pin[num],INPUT);
}

void reset(){
for(int num=0;num<4;num++) digitalWrite(pin[num],LOW);
}

void stepper()
{
for (int num=0; num<4;num++) { digitalWrite(pin[num],steps[abs(type-current)][num]);} if(type==0) {++place;} if(type==3) {--place;} delay(2); } void button1() { Lockreading = digitalRead(pin[5]); if (Lockreading != lastLockState) { lastLockTime = millis(); } if ((millis() - lastLockTime) > 50)
{
if (Lockreading != LockState) {
LockState = Lockreading;
if (LockState == HIGH) {
lock=false;        
if ((place!=1024*4)&&(place!=512*4)) {type=abs(type-3);}
}
}
}
lastLockState = Lockreading;
}

void button2()
{
Returnreading = digitalRead(pin[6]);
if (Returnreading != lastReturnState) 
{
lastReturnTime = millis();
}
if ((millis() - lastReturnTime) > 50)
{
if (Returnreading != ReturnState) {
ReturnState = Returnreading;
if (ReturnState == HIGH) {
type=3;
digitalWrite(pin[4],HIGH);
while (place>0)
{
for (int num=0; num<4;num++) { 
digitalWrite(pin[num],steps[3-current][num]);}
--place;
if (current==3) {current=0;}
else ++current;
delay(2);
}
digitalWrite(pin[4],LOW);
reset();
}
}
}
lastReturnState = Returnreading;
}

void loop() {
if (lock==true) {button1();button2();}
if (lock==false) {stepper();}
if ((place==2048*4)or(place==0)or(((place==1024*4)or(place==512*4))and(type==3))){lock=true;reset();}
if (current==3) {current=0;}
else ++current;
}




I hope that you enjoyed my Automated Curtain Automation project. I hope to keep posting more of my projects even though I get quite busy with college. Have fun automating your curtains!

