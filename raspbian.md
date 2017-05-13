#  Raspberry Pi - Raspbian

grab light image from here and copy using dd (disks) to sd card
https://www.raspberrypi.org/downloads/raspbian/

from raspi-config turn on ssh and i2c
and/or this for i2c https://github.com/fivdi/i2c-bus/blob/master/doc/raspberry-pi-i2c.md


add public key to authorized_keys in .ssh
 ssh-copy-id -i ~/.ssh/<public>.pub <hostname from ssh config file>

install git and clone aliases

add backports
https://github.com/superjamie/lazyweb/wiki/Raspberry-Pi-Debian-Backports

install docker
$ curl -sSL get.docker.com | sh

# Using Debian, as root, works with arm now
curl -sL https://deb.nodesource.com/setup_7.x | bash -
apt-get install -y nodejs

npmig n
npmig pm2

installb lxde-core lxappearance
<!-- openbox lxpanel obconf obmenu -->
don't need xorg is headless

install nomachine
https://www.nomachine.com/download/download&id=10
sudo dpkg -i nomachine_5.2.21_1_armhf.deb
remove client, secure server with key only access,   server.cfg

filemanager: nautilus??, midnight commander for terminal/ssh
terminal: sakura
visualcodesudio, https://code.headmelted.com/#linux-header, download arm.deb
browser: chromium,  http://kusti8.me/RPi-chromium/
gnome-disk-utility, xarchiver
synaptic, gdebi
install git-cola then remove files from user/bin and replace with links from /bin of repo cloned to /opt

run gksu-properties and change to mode to "sudo"

<!-- kill and flush the panel and then start with clean menu in ~/.config/menu
```
killall lxpanel
find ~/.cache/menus -name '*' -type f -print0 | xargs -0 rm`
lxpanel
``` -->

user/share/applications   chown and chmod 644 to sysadmin
opt/   chown and chmod 775 opt to sysadmin


conky, parcellite ?
