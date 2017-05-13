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


## npm link
Late to the party but after reading several posts including this one and a few head bashing/testing hours later I feel compelled to share my take on a local repo under version control being used as a package in a node project.

Do this all from the root of your project:

 ```npm link /path/to/your/local/package/```  

 ```npm link <"name:">```  "name:" key in your local package's package.json

 ```npm install --save /path/to/your/local/package/```

the last one you will not find in the npm documentation on using "link" but it's critical

*note*:  you can use relative paths  (e.g. ```../<localpackagefoldername>```  if sharing a parent folder)

**important**:  your local package package.json MUST have a package "name:" key and a "main:" key pointing to an entry point js file otherwise this all fails.

now you can use  ```require('name')```  in your code and later if you publish to npm you don't have to change anything expect the line in project package.json which would be just as easy to delete and ```npm install``` as edit.

If you add a package to your local module with ```npm install``` then do the same in your project and it will be added to the project's node_modules.  If you do an ```npm uninstall``` in your local package then do an ```npm prune``` in your project

*note*: if you have run ```npm install``` in your local package root then node_modules was created there for that package and now when you ```npm install``` in your project you'll get warnings like this you can ignore ```skippingAction Module is inside a symlinked module: not running remove```.  If that bugs you then delete the node_modules folder in your local package if you are not running it separately.

Starting with npm 3 node_modules are now flattened so doing it this way means your local package dependencies within the project are flattened too!  Further, you can't just put your full git repo into node_modules as npm will despise the .git folder and of course you'll be nesting your node_modules.  

**My Big Trick**:  Sure you can have your local package repo in a directory separate from your project but if you make it a git submodule in a subdirectory of your project you get the best of both worlds, flattened dependencies in node_modules and combined pushes of the project and package (submodule) at the same time.  Plus it makes those link command paths trivial.

**Tip**:  If you go the separate directory (no submodule) route then use your ide editor (e.g. atom) to add the local package folder to your project tree for easy editing with your project.  Too, if you go this route it's up to you to commit and push changes for the local package since it's not a submodule.

Probably the only caveat I can think of at this time is to be sure that you have dependencies entries in your local package even if they are in the project's package.json otherwise if someone uses the local package somewhere else (on it's own) it will be missing dependencies for ```npm install```
