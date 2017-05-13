## Build a minimal Router or Server using Debian Jessie

1. Get net install debian, install as uefi, use pv/vg/lv for root and swap
2. Set ssh going,  ssh-copy-id -i ~/.ssh/id_rsa.pub YOUR-REMOTE-HOST
Install git and clone aliases repo
*  add jessie backports.  
*  Install systemd  230.
** install backports of packages for latest
3. If Add xorg (for local windowing only) and openbox and nomachine if GUI is wanted
4. packages to install for openbox.... gdebi, gparted, gedit, pacfm, atom, libxscrnsaver for atom.  
https://medium.com/@mos3abof/how-to-install-firefox-on-debian-jessie-90fa135e9e9
*  run gksu-properties and change sudo
4. Install nomachine.  Edit server.cfg and set to nx-keys only for authentification (if wanted)
*  remove etc/network/interfaces
*  use etc/systemd/network for wlan=eth0, lan=bridge, eth0 to bridge  (clone repo)
*  install hostapd for wlan interface and use bridge entry to lan
* clone iptables repo and link iptables service to /etc/systemd/service

5.  install shorewall from deb file in testing (core first) https://packages.debian.org/stretch/shorewall
