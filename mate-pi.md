#  Raspberry Pi - Ubuntu

grab ubuntu mate rpi version here.
https://ubuntu-mate.org/download/

write to sd card using gnome disk or
xzcat /mnt/AllData/Downloads/ubuntu-16.04-preinstalled-server-armhf+raspi3.img.xz | sudo dd of=/dev/sde bs=32M

on boot user `sysadmin` and hostname/computername `sbc` and `thx1138` for pw

sudo nano /etc/ssh/sshd_config, enable password access
sudo service ssh restart
or
sudo systemctl restart ssh
check with
sudo systemctl status ssh

now try
ssh sbc

with this config file entry
```
Host sbc
   user sysadmin
   hostname sbc.local
   IdentitiesOnly=yes
   IdentityFile ~/.ssh/sysadmin
```

sudo apt-get install git
git config --global user.name "SBC System Admin"
git config --global user.email "sysadmin@sbc.org"
git clone https://github.com/dkebler/.kbd-aliases.git
move .bash_aliases up a directory
mv .bash_aliases.off ../.bash_aliases

nano .bashrc
add
export EDITOR=nano

generate a public/private ,   
`ssh-keygen`   to sysadmin

add public key to authorized_keys in .ssh from local with
`ssh-copy-id -i ~/.ssh/sysadmin.pub sbc`

now try
ssh sbc
should work without password, if so
change to no password using
`essh`

now update git via ppa
`ppa git-core`
`update`
`install git`

load in credential store
`git config --global credential.helper store`

bashrc
# ignore https certs when cloning cause they are self-signed
export GIT_SSL_NO_VERIFY=1

install mc   (midnight commander for text based filemanger)

make headless
`sudo systemctl set-default multi-user.target`
`reboot`
login
try `startx`
should be able to ssh as well

use browser to install nomachine. arm7
edit server.cfg
sudo editor /usr/NX/etc/server.cfg
CreateDisplay 1
DisplayOwner "sysadmin"
DisplayGeometry "1920x1080"

Make a custom player connection to sbc
disable server broadcast in server task app.

if getting connection refused 111 then edit local ~/.nx/config/player.cfg  to
  <option key="Discover other NoMachine servers in the network" value="false" />

for key authentification add sysadmin.pub
scp ~/.ssh/sysadmin.pub  sbc:/home/sysadmin/.nx/config/authorized.crt
sudo /etc/NX/nxserver --restart
try key based session.
remove pw login
`AcceptedAuthenticationMethods NX-private-key`
in server.cfg.

reboot to check it all out.

then save image as base

### Software

### Prepare /opt
put third party software in /opt
opt/   
sudo chown root:sysadmin /opt
sudo chmod 775 /opt

### Prepare root for use
add these to root's .bashrc
```
for f in /home/sysadmin/.kbd-aliases/*; do
if [ ${f: -4} != ".off" ] && [ $(basename $f) != "README.md" ] ; then
  # echo 'loading alises '$f
. $f
fi
done

#default editor >editor
export EDITOR=nano

# ignore https certs when cloning cause they are self-signed
export GIT_SSL_NO_VERIFY=1
```

### NODE
sudo -i
curl -sL https://deb.nodesource.com/setup_7.x | bash -
installa nodejs

move globals
mkdir /opt/npm-global
npm config set prefix '/opt/npm-global'
# add path for moved npm globals to .bashrc
export PATH="/opt/npm-global/bin:"$PATH

npmig n
npmig pm2


### IDE
visualcodesudio, https://code.headmelted.com/#linux-header,
manual install download arm.deb
if need be  install libxssl1 if missing (was not on 16.04)


### i2c
if need be
installa i2c-tools

edit
/boot/config.text
dtparam=i2c_arm=on
dtparam=i2c_arm_baudrate=10000
remove i2c-dev and bcm2307 from /etc/modules-load.d/rpi2.conf

Add sysadmin to i2c group
sudo usermod -a -G i2c sysadmin

### git-cola
installa git-cola then remove files from user/bin and replace with links from /bin of repo cloned to /opt
in /opt    git clone https://github.com/git-cola/git-cola.git

subrepo
git clone https://github.com/ingydotnet/git-subrepo.git
source /path/to/git-subrepo/.rc


installa synaptic


### node-gyp

for node-gyp and other compilation
gcc and make
installa build-essential
installa older python for node-gyp
installa python2.7
make node-gyp use 2.7 with
node-gyp --python /usr/bin/python2.7
for existing project builds set npm variable
npm config set python /usr/bin/python2.7


### docker

installa docker
$ curl -sSL get.docker.com | sh
