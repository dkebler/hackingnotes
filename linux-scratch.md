## Build a minimal Router or Server using Debian Jessie

1. Get net install debian, install as uefi, use pv/vg/lv for root and swap
2. Set ssh going,  ssh-copy-id -i ~/.ssh/id_rsa.pub YOUR-REMOTE-HOST
Install git and clone aliases repo
*  add jessie backports.  

1 2 alt install mate 17

*  Install systemd  230.
** install backports of packages for latest
3. If Add xorg (for local windowing only) and openbox and nomachine if GUI is wanted
4. packages to install for openbox.... gdebi, gparted, gedit, pacfm, atom, libxscrnsaver for atom.  
https://medium.com/@mos3abof/how-to-install-firefox-on-debian-jessie-90fa135e9e9
*  run gksu-properties and change sudo
4. Install nomachine.  Edit server.cfg and set to nx-keys only for authentification (if wanted)
*  remove etc/network/interfaces
*  change eth0 to wan,   ip link set eth0 name wan  or
better edit /etc/udev/rules.d/70-persistent-net.rules

*  use etc/systemd/network for wan=eth0, lan=bridge, eth1 and wlan to bridge  (clone repo)
*  install hostapd for wlan interface and use bridge entry to lan
* clone iptables repo and link iptables service to /etc/systemd/service

install dnsmasq,  edit dnsmasq for dhcp
link etc/resolv.conf to rrun/systemd if not alreay done
