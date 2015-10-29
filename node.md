# Node JS

## Installation

ubuntu-debian install from github repo bash script
https://github.com/nodesource/distributions/tree/master/deb

https://github.com/joyent/node/wiki/installing-node.js-via-package-manager#debian-and-ubuntu-based-linux-distributions


Setup with Ubuntu:

    curl --silent --location https://deb.nodesource.com/setup_4.x | sudo bash -

Then install with Ubuntu:

    sudo apt-get install --yes nodejs

Setup with Debian (as root):

    apt-get install curl
    curl --silent --location https://deb.nodesource.com/setup_4.x | bash -

Then install with Debian (as root):

    apt-get install --yes nodejs

Then upgrade npm to latest - see releases
https://github.com/npm/npm/releases
must use release number as of 10/2015 3.x is still in pre-release.

    npm install -g npm@3.x.x

## Update-Upgrade

load the n program used to upgrade node

    sudo npm cache clean -f
    sudo npm install -g n
    sudo n latest

upgrade npm as above 

    npm install -g npm@3.x.x


## Reverse Proxy

my package of choice is Redbird

Use PM2 to get your code to run persistently.  

### Windows
https://nssm.cc/description
https://github.com/Unitech/PM2/issues/1165