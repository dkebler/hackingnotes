# Node JS

## Installation

ubuntu-debian install from github repo bash script
https://github.com/nodesource/distributions/tree/master/deb

https://github.com/joyent/node/wiki/installing-node.js-via-package-manager#debian-and-ubuntu-based-linux-distributions

Check for latest node versions here
https://github.com/nodejs/node/blob/master/CHANGELOG.md

As root!  Setup with Ubuntu:

    curl --silent --location https://deb.nodesource.com/setup_5.x | sudo bash -

Then install with Ubuntu:

    apt-get install --yes nodejs

Setup with Debian (as root):

    apt-get install curl
    curl --silent --location https://deb.nodesource.com/setup_5.x | bash -

Then install with Debian (as root):

    apt-get install --yes nodejs

Then upgrade npm to latest - see releases
https://github.com/npm/npm/releases
must use release number as of 10/2015 3.x is still in pre-release.  (Do NOT use upgrade until 3.x out of pre-release)

    npm install -g npm@3.x.x

--------------

if you have git already installed try installing `n` from the get go.  (as root of course)

     git clone https://github.com/tj/n.git
     cd n
     make install
     n latest
     node -v

now add this to your .bashrc file.  npm won't work without it.

    export NODE_PATH=$NODE_PATH:/usr/local/lib/node_modules

then try seeing if npm will respond

      npm -v

if you are having trouble installing npm or want the latest try this.

curl -L https://www.npmjs.com/install.sh | sh



## Update-Upgrade

load the n program used to upgrade node

    sudo npm cache clean -f
    sudo npm install -g n
    
then to see the versions available
    
    sudo n ls

or look here.  

then install latest or stable or specific version    
    
    sudo n latest
    sudo n stable
    sudo n 5.x.x

For npm here are the current releases here https://github.com/npm/npm/releases or try these

    npm dist-tags ls npm
    npm view npm@latest version
    npm view npm@next version

prior to 3.4.1 as the latest release be explicit

    sudo npm install -g npm@3.x.x

with 3.4.1 and above as the latest release you should get away with a simple.

    sudo npm install -g npm

to get the latest (which of course is not the "latest" which is really "next")

    sudo npm install -g npm@next


if npm install happens to get trashed.

First delete the npm directory likely in ```/usr/local/lib/node_modules``` if you are running *nix.   I had to do this or the reinstall kept throwing errors.

Then get the script mentioned on the github repo readme
https://github.com/npm/npm
https://www.npmjs.com/install.sh

The curl download and run didn't work for me I had to download the script and then run it.

--------

for manual install of node try this

http://stackoverflow.com/questions/23082242/how-to-install-nodejs-0-10-26-from-binaries-in-ubuntu/23084499#23084499

## Reverse Proxy

my package of choice is Redbird

Use PM2 to get your code to run persistently.  

### Windows
https://nssm.cc/description
https://github.com/Unitech/PM2/issues/1165