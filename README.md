# OpenCV-Detection
# Setting up Raspberry Pi
1. Using 'Rasbperry Pi Imager', install Raspberry Pi OS onto the SD Card.
2. Access the Raspberry Pi through Wi-Fi via SSH
3. Run the following commands:
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python-pip
sudo apt-get install python-dev
sudo pip install future
sudo apt-get install screen
sudo apt-get install python-wxgtk4.*
sudo apt-get install libxml 
sudo apt-get install libxml2-dev
sudo apt-get install libxslt1-dev
pip install lxml
sudo pip3 install pyserial
sudo pip3 install dronekit
sudo pip3 install geopy
sudo pip3 install MAVProxy
```
4. Set up serial connection and type the following in SSH:
```
sudo raspi-config
```
5. Change the folowing settings:

a) Go to interface settings

b) Enable Legacy camera

c) Enable SSH

d) Enable VNC

e) Go to serial

f) When prompted, select yes to 'Would you like a login shell to be accessible over serial?'

g) When prompted, select yes to 'Would you like the serial port hardware to be enabled?'.

h) Reboot the Raspberry Pi when you are done.

i) The Raspberry Pi’s serial port will now be usable on ```/dev/ttyAMA0```.
