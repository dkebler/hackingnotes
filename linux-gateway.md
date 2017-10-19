## Build a minimal Router or Server

load ubuntu mate 17 9 (see mate install instructions), and  
Type the following command to edit /etc/hostname using nano

to change machine name
sudo nano /etc/hostname
sudo nano /etc/hosts
sudo reboot

to turn off desktop at boot
`systemctl set-default -f multi-user.target`  to turn off desktop at boot

2. Set ssh going,  ssh-copy-id -i ~/.ssh/id_rsa.pub YOUR-REMOTE-HOST
Install git and clone aliases repo
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get update

then

* download and Install nomachine.  Edit /usr/NX/etc/server.cfg and set to nx-keys only for authentification, copy key
turn off broadcast.
Add the public SSH key on the server
https://www.nomachine.com/AR02L00785
1. Navigate to the <user's home>/.nx/config directory.
2. You should find there the authorized.crt file. Create this file if it doesn't exist.
3. Append your SSH public key at the end of the authorized.crt file.
4. Save changes.

change the port to 6450


*  remove etc/network/interfaces
*  change xxx to wan,   via udev
make  /etc/udev/rules.10-rename-wan.rules
with
```
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", \
  ATTR{address}=="00:e0:b4:18:cd:50",           \
  ATTR{type}=="1",NAME="wan"
  ```
*  use etc/systemd/network for wan=eth0, create lan=bridge  then attach eth1 and wlan to lan=bridge  (clone repo)
and edit files for actual interface names
*  install hostapd for wlan interface and use bridge entry to lan
*  install dnsmasq,  edit dnsmasq for dhcp
*  link run/systemd/resolve/resolv.conf  etc/resolv.conf to  if not alreay done
 uncomment in /etc/default/dnsmasq
IGNORE_RESOLVCONF=yes
make sure dnsmasq.conf has either
resolv-file=/etc/resolv.conf
if using link or
resolv-file=run/systemd/resolve/resolv.conf
make sure dnsmasq service has no errors
put in [unit] section of /etc/systemd/system/multi-user.target.wants/dnsmasq.service
Before=systemd-resolved

* clone iptables repo and link iptables.service to /etc/systemd/system/
enable service with systemd
sde iptables   

make headless
`sudo systemctl set-default multi-user.target`
`reboot`
login
try `startx`
should be able to ssh as well



==============

or Server using Debian Jessie

1. Get net install debian, install as uefi, ** install backports of packages for latest
2. Set ssh going,  ssh-copy-id -i ~/.ssh/id_rsa.pub YOUR-REMOTE-HOST
Install git and clone aliases repo
*  add jessie backports.  
*  Install systemd  230.
3. If Add xorg (for local windowing only) and openbox and nomachine if GUI is wanted
4. packages to install for openbox.... gdebi, gparted, gedit, pacfm, atom, libxscrnsaver for atom.  
https://medium.com/@mos3abof/how-to-install-firefox-on-debian-jessie-90fa135e9e9
*  run gksu-properties and change sudo
===========


or just
