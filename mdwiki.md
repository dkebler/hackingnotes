

## Using MDwiki 

MDwiki is a quick javascript based website that is __completely__ javascript based and thus does not need a server.

For details see the [MDwiki website](http://dynalon.github.io/mdwiki/#!index.md)

Use any plain text editor to edit the .md files in the folder.  [Sublime text editor version 3](http://www.sublimetext.com/) is a great choice. 

There is a canned menu structure found in the navigation.md file.  If you change the names of choices and associated files and rename them as well you can customize and change to suit.

The Site name on the nav bar is set at the top of the navigation.md file

The website title used in the browser is set in the config.json file

index.html is the actual MDwiki program and contains all the javascript to render and load the markdown (.md) pages.

You can pick a canned bootswatch theme and set the default in the navigation.md file.   It is possible to do custom styling but that involves a custom theme.  (see mdwiki site)

index.md is the "home" page and is rendered and served by default when index.html is loaded (which should be the default file name on your server or be changed to match...e.g. default.htm)

The MDwiki site has some basic markdown syntax but a search of the internet will find you many resources and cheat sheets.

config.json holds the few site configurations

when you want to deploy it's as simple as moving a copy of the directory to whereever you want to serve it.
Or if you are using git when editing your site (which is recommneded) you can push the master branch to where you deploy.


## MDWiki Quickstart Template

This a directory that can be cloned or downloaded to get going quickly with a mdwiki based web.
you just drop this folder somewhere where it can be served to a browser and then edit away on markdown files.

The details of what and how are found on the homepage once you get it up and running or just look in the index.md file

#### Serving it up

The default filename is index.html so if that is the default for your server just navigate to the directory and you'll be up and running.  If have it just locally on your machine can just drop the full file path to the directory into your browser address to see the site.  

Otherwise it can be served.

Here you can [see the template site](http://david.kebler.net.s3.amazonaws.com/mdwiki-template/index.html#!index.md) served from an S3 Bucket

__IIS Server__

If you are running a Windows IIS there a few cavets.  
See this page http://dynalon.github.io/mdwiki/#!tutorials/iis/iis.md

plus this bit of info on json files and how to make the needed mime types global to the server.
https://github.com/Dynalon/mdwiki/issues/189