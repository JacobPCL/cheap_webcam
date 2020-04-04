## Overview
Goal of this project is to make use of cheap, easily available laptop cameras and repurpose them as fully functional USB cameras.

In https://github.com/JacobPCL/cheap_webcam/blob/master/camera_list.md you can find list of tested cameras and in corresponding laptop model folder in cam_models you can find pinout. The same model of camera can have different PCB version thus different test point alignment so watch out.

## To Do
- [ ] design 3d cases for tested cameras
- [ ] add more cams

## How to reverse engineer camera pinout.

### You are doing it at your own risk so don’t damage your favorite PC - I'm not responsible for that.

Laptop camera pinout can differ. Number of pins and test pads is determined by presence of mic's and additional hardware. Usually there are 4 pins (GND, 3.3V, D+, D-) and 2 pins for mic.
Microphones in most cases are directly connected to soundcards which are built into laptop motherboard, despite that many camera chips (like SONIX SN9C280AJG in Inspiron 3135 cam) have built in ADC and audio codec for mic connection.

Ideally chose a laptop camera that have test pads to which you can solder to, otherwise you will probably need also the camera lead or a very steady hand.

To find necessary pins you will need a multimeter with diode test function and ideally a lab bench power supply to not fry your camera but if you are 100% sure 2x 1n4001 diodes and USB cable will be enough.

####  1. Finding GND
GND is the easiest pin to find, select continuity test on your multimeter, with one test lead touch a large GND field on your camera PCB or part of metal shielding and with the other test lead find a test pad or pin.

####  2. Finding D+ D-
Diodes used in camera chips USB data lines are usually paired thus have almost identical forward-biased diode voltage drop, you have to find pins witch have identical voltage drop.
Take negative test lead and connect it to GND, using positive test lead find out two pins/test pads that have identical voltage drop - those line will be your data lines D+ and D-.
Which one is + which - you will have to find by connecting it to a PC after you find last 3.3V pin.
If data pins are reversed computer will detect there is USB device but wont be able to get USB ID, if the order is correct device will be recognized.

#### 3. Finding 3.3V
This is the hardest one. The easiest way is to find large SMD capacitor near camera chip which one lead is connected to GND, this is called a decoupling capacitor, the other lead of that capacitor will be connected to 3.3V line. Touch the 3.3V capacitor lead with one test lead and using continuity check function find 3.3V pin/test pad.

![Image of decoupling cap](https://raw.githubusercontent.com/JacobPCL/cheap_webcam/master/images/de_cap.jpg)

#### 4. Connecting everything together
Firstly its recommended to to test GND and 3.3V pins by connecting them to lab bench power supply and setting overcurrent protection to 100mA.
Cameras which I tested initially took 10-15mA for 1-2s and then shutdown when they couldn’t establish USB connection on data lines.

When You are sure which pin is which you can connect your camera:
* data lines and GND directly
* power supply : we need to use two 1n4001 or other **silicone** diode in series to achieve 1,3-1,7V voltage drop in order to protect camera - it needs only 3.3V to work.

![Image of wire connection](https://raw.githubusercontent.com/JacobPCL/cheap_webcam/master/images/1n4001.jpg)

If everything is correct your OS should recognize your cam and you should be able to get image from it. 
