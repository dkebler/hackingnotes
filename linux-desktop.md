

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

## kill

First, open up your GNOME menu and go to System > Preferences> Keyboard Shortcuts.

Second, click “Add” and a window should come up with 2 text boxes, labeled “Name” and “Command.” In Name, type “Force Quit” and in the Command box, type “xkill” and press Apply.

Scroll down to Custom Shortcuts in the Keyboard Shortcuts window and you’ll see Force Quit on the left side and Disabled on the right side. Click on Disabled once, and press a combination of keys on your keyboard. I used the Super (Windows logo) key and Escape, but you can use whatever you want, just as long as it doesn’t conflict any of your other shortcuts. Now Disabled will be changed to whatever keys you pressed.

Close out of the Keyboard Shortcuts window and you’re done! When ever you press your key combination, your cursor will change to crosshairs, or a skull and bones, or at least something different than just the arrow. Whatever window you click on will be forced to close, killing all of it’s processes.

If you find yourself to have accidentally pressed your force quit keys, then just right click and it’ll close out of the force quit mode.