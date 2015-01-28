
# OS Installs

[Windows Install](hugo-win.md)
[OSX Install](osx-win.md)


# Compass/Foundation Install

Ruby must be installed first, then the gem installer (if not with ruby)

Then install compass and foundation gems http://compass-style.org/install/

    $ gem update --system
    $ gem install compass
    $ gem install zurb-foundation

Then install foundation within your hugo project root directory with

foundation new framework
cd framework
compass compile

which creates a framework subdirectory

now edit the `config.rb` file within that framework directory compass outputs 

    css_dir = "../static/framework/css"
"

`app.scss` is created by default in the framework/sass directory.  Change it's name to `foundation.scss` to it can be imported.  Create a file `sitestyle.scss in the same directory using this code as a minimum.

    /* Load Pallete */
    @import "palette";

    /* bring in defaults including foundation */
    @import "app";

    /* Import Custom Settings */
    /*  This file allow you to customize any foundation css */
    @import "custom";
    /*
    /* End Custom Settings */


To have compass build your css from sass upon changes invoke this from the framework directory  (best is to use a script)

    compass watch

In the hugo static directory there should be a framework/css directory.
To this add a framework/js directory.  Here you'll need to copy and javascript files the framework needs from the 


bower-installer puts required css and other stuff into static directory.
This needs to be run if new packages are added or updated via bower.  Editing
the bower.json file for each package is an option for undesired behavior
https://github.com/blittle/bower-installer



