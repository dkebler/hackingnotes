#  Raspberry Pi - Ubuntu

grab ubuntu server rpi version here.
https://ubuntu-pi-flavour-maker.org/download/

or others here.

write to sd card (gnome disk failed used commandline)
xzcat /mnt/AllData/Downloads/ubuntu-16.04-preinstalled-server-armhf+raspi3.img.xz | sudo dd of=/dev/sde bs=32M

boot and set up ssh config entry.
```
Host ub
   user ubuntu
   hostname 192.168.0.xx
```

`ssh ub`

change pw to thx1138

copy clone-user.sh script to rpi and run copying ubnuntu to sysadmin using same password

???  ssh -T ub </home/david/AllData/hacking/bash-scripts/user/mk-sysadmin.sh  check for sudo
or
scp /home/david/AllData/hacking/bash-scripts/user/mk-sysadmin.sh ub:
ssh ub
sudo ./mk-sysadmin.sh

add avahi for ipless entry
`sudo apt-get install avahi-daemon`
change hostname file to sbc (single board computer)
sudo nano /etc/hostname


generate a public/private ,   
`ssh-keygen`   to sysadmin

add public key to authorized_keys in .ssh
ssh-copy-id -i ~/.ssh/sysadmin.pub sbc

`sudo apt-get install avahi-daemon`

reboot

use this config

```
Host sbc
   user sysadmin
   hostname sbc.local
   # hostname 192.168.0.xx
   IdentitiesOnly=yes
   IdentityFile ~/.ssh/sysadmin
```

remove ubuntu user
sudo userdel ubuntu
sudo rm -rf /home/ubuntu

from home
git clone https://github.com/dkebler/.kbd-aliases.git
move .bash_aliases up a directory

install mc   (midnight commander for text based filemanger)

update git via ppa
ppa git-core
update
install git

bashrc
# ignore https certs when cloning cause they are self-signed
export GIT_SSL_NO_VERIFY=1

load in credential store
git config --global credential.helper libsecret


**SAVE THIS AS BASE IMAGE NOW**

remove authorized_keys and copy over new key other than sysadmin
ssh-copy-id -i ~/.ssh/sysadmin.pub sbc
check ssh works
sudo nano /etc/ssh/sshd_config, remove password access


add permissions

### Software

put third party software in /opt
opt/   
sudo chown root:sysadmin /opt
sudo chmod 775 /opt

# Using Debian, as root, works with arm now
sudo -i
curl -sL https://deb.nodesource.com/setup_7.x | bash -
apt-get install -y nodejs

move globals
mkdir /opt/npm-global
npm config set prefix '/opt/npm-global'
# add path for moved npm globals to .profile
PATH="/opt/npm-global/bin:"$PATH


npmig n
npmig pm2

for node-gyp and other compilation
install globally
npmig node-gyp
gcc and make
install build-essential
install older python for node-gyp
install python2.7
make node-gyp use 2.7 with
node-gyp --python /usr/bin/python2.7
for existing project builds set npm variable
npm config set python /usr/bin/python2.7


install docker
$ curl -sSL get.docker.com | sh


## i2c access

install i2c-tools

















**Save Image before making headless gui**

for headless gui

install nomachine
https://www.nomachine.com/download/download&id=10
scp the file
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
