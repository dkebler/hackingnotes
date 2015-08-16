# Sublime Text

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#code)

## Packager

[Sublime Package Control](https://sublime.wbond.net/)

[Sublime Package Control Install](https://sublime.wbond.net/installation)

Install a Package
/preferences/package control/install package
List Install Packages
/preferences/package control/list packages

### Base Packages

[Sidebar Enhancements](https://sublime.wbond.net/packages/SideBarEnhancements)
Browser Refresh (not needed for hugo or compass)
[Markdown Editing](https://sublime.wbond.net/packages/MarkdownEditing)
might have to set view/syntax/opens all with current extensions as/markdownediting/???  to get nice formatting
Delete Blank Lines
FileDiffs
rsub (for editing locally files on remote server)
https://github.com/Drarok/rsub
[Dependents](dependents.md)

ssh to the server and wget this 

```
sudo wget -O /usr/local/bin/rsub https://raw.github.com/aurora/rmate/master/rmate

sudo chmod +x /usr/local/bin/rsub

```
original post here.
http://www.lleess.com/2013/05/how-to-edit-remote-files-with-sublime.html

Possible sh file or put it in the config.

``` 
#!/bin/bash
ssh -R 52698:localhost:52698
ssh -o IdentitiesOnly=yes -i /home/david/.ssh/EC2Server.pem ubuntu@54.191.214.51
```

```
~/.ssh/config
Host 54.191.214.51
   RemoteForward 52698 localhost:52698
```




Windows/Linux:

    Ctrl+Alt+Backspace --> Delete Blank Lines
    Ctrl+Alt+Shift+Backspace --> Delete Surplus Blank Lines

OSX:

    Ctrl+Alt+Delete --> Delete Blank Lines
    Ctrl+Alt+Shift+Delete --> Delete Blank Lines


### Misc

ctnrl-j join lines  (http://sublimetexttips.com/7-handy-text-manipulation-tricks-sublime-text-2/)

### User Settings

For not saving state
```
  "hot_exit": false,
  "remember_open_files": false
```

### Languages

download a raw .dic file from there to the packages directory
(you can find out by doing preferences browse packages)
https://github.com/SublimeText/Dictionaries
click on the .dic file right click on the raw button and download

### My Snippets

fmt<tab>  {{% format xxxxx %}}
cfmt<tab> {{% /format %}}

img<tab>  {{% image filename="" caption="" position="" %}}

ibr<tab> {{% imagebreak %}}

lno<tab>  {{% linkout text="" url="" %}}  (not finished??)

For notes use NOTE-xx:  where xx is initials of person for that note.  use no initial for a generic note.  Could us bold-italics for notes.  Example

***NOTE-dk:this is example note***

to use the note snippet.   type nt then the <tab> key

## Find and Replace

### Regular Expressions - Regex


Finds a paragraph
^(.+?)\n\s*\n
Put something before and after the paragraph
{test}\n\1\n{end test}\n\n

 find paragaraphs
```
^(.+?)\n\s*\n
```

find paragraph starting with {{% image 
```
^(\{\{% image.*$)
```
wrap the english class format shortcode around it
```
{{% format english %}}\n\1\n{{% /format %}}
```

find paragraphs except those that start with {{%  which is a shortcode
```    
^(?!\{\{\%)(.*$)\n\s*\n
```

put format shortcoded around paragraph
```
{{% format english %}}\n\1\n{{% /format %}}\n\n
```

