# Linux OS

## Set Static IP

For manual wired connections be sure to delete any extra line in the address window or the save button will remain greyed.


for listing DNS servers from DHCP
>nmcli dev list iface eth0 | grep IP4


## Aliases

make the command line easier

First make sure your .bashrc file has this.  (assuming rsub has been loaded)
```
rsub ~/.bashrc
```

```
# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

Then add .bash_aliases  file
```
rsub ~/.bash_aliases
```

Then add any aliases you want

reload is a good one because once you changes you must reload the .bash files.  Of course the first time you must do it manually :-).

```
alias install="sudo apt-get install"
alias poweroff="sudo shutdown -h now"
alias restart="sudo shutdown -r now"
alias srsub="sudo rsub"
alias cd..='cd ..'
alias cp='cp -i'
alias d='ls'
alias df='df -h -x supermount'
alias du='du -h'
alias egrep='egrep --color'
alias fgrep='fgrep --color'
alias grep='grep --color'
alias l='ls'
alias la='ls -a'
alias ll='ls -l'
alias lla='ls -l -a'
alias ls='ls -F --color=auto'
alias lsd='ls -d */'
alias md='mkdir'
alias mv='mv -i'
alias p='cd -'
alias rd='rmdir'
alias rm='rm -i'
alias path="echo $PATH"

# Edit this file
 alias ea="rsub ~/.bash_aliases"

# Will scrub and reload any alias in .bashrc too
alias reloadrc="unalias -a && source ~/.bashrc"
# Will scrub all and reload only aliases in .bash_aliaes
alias reloada="unalias -a && source ~/.bash_aliases && compgen -a"
```

if you delete any you must either remove it manually or restart your shell.  sourcing the bash_aliases file will not remove that alias from the current session even though it have been removed from the file.  The reload alias above removes all then reloads them via the .bashrc script.  Must be done manually when adding the reload alias for the first time.

## Users

edit etc/skel  directory so that you have any current .bashrc file there
might as well include the .bash_aliases file there now.

edit /etc/adduser.conf for defaults for adding a new user.

Create a new group if need be, then add a user, then add them to sudo group if need be.  All must be done as sudo

```
sudo groupadd <name>
sudo useradd -G <groupname>  <username>

sudo usermod -G sudo -a <username>
```

http://www.tecmint.com/add-users-in-linux/

http://www.tecmint.com/usermod-command-examples/

How to so any script (including alias) for a login shell
http://askubuntu.com/questions/610052/how-can-i-preset-aliases-for-all-users

## Easy Kill Program

if using gnome, can use xkill, set it up as a keyboard shortcut,  envoke shortcut, click on offending program....killed.

http://community.linuxmint.com/tutorial/view/30

## Starup

will tell you what starup you are using `/sbin/init --version`

process editor, must install `sudo sysv-rc-conf`

for upstart
 `echo manual > /etc/init/<service>.override; chmod 644 /etc/init/<service>.override`
 delete the override file to go back to manual

# Groups and Users

List all in a group
`getent group <groupname>;`
