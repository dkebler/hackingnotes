

## always on top

http://superuser.com/questions/622937/always-on-top-shortcut-key-in-linux-mint
I've solved the problem by writing a very simple script to do it using wmctrl, which is a program that can send commands to a X Window Manager from the command line. If wmctrl is not installed on your machine install it with the command below, you can check if it is already installed with which wmctrl which will show the path if it is already installed or show nothing if it isn't installed, AFAIK it is not installed by default in Linux Mint.
```
sudo apt-get install wmctrl
```
Then create a new (very simple) script which should work with any X Window Manager:
```
#!/bin/bash

# Script: window-toggle-always-on-top.sh
#
# Script to toggle the always on top setting of the active window. It is 
# designed to work by being assigned to a system hot key.

wmctrl -r :ACTIVE: -b toggle,above
exit 0
```
Once the script is created open the Linux Mint keyboard settings, go to 'Custom Shortcuts', and add a custom shortcut,