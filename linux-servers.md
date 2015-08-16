
## Getting started

First I created a bare bones 14.10 Ubuntu instance and saved that as an image so that one could launch a blank server quickly for configuration.

Easy way to login is to use a bash script.

```
#!/bin/bash
#next line is for setting up rsub tunnel back to sublime on local machine
ssh -R 52698:localhost:52698
#now login to server
ssh -o IdentitiesOnly=yes -i /home/<localusername>/.ssh/<keypairusedwithawsintances>.pem ubuntu@54.148.49.11
#use line below so you can observe any issues.
read -p "Press [Enter] key to conintue"
```

One could even put most of this in a config file in the .ssh directory.


### Editing Files

on can use nano but the super easy way is using rsub.  Add rsub plugin to sublime text (see sublime notes)

To more easily edit files (using sublime text)  I added rsub package to sublime and to the server.

https://github.com/Drarok/rsub and see also the [Sublime](sublime.md) page.  

Then ssh to the server and wget this 

```
sudo wget -O /usr/local/bin/rsub https://raw.github.com/aurora/rmate/master/rmate

sudo chmod +x /usr/local/bin/rsub

```
original post here.
http://www.lleess.com/2013/05/how-to-edit-remote-files-with-sublime.html

It's a full on bash script so doesn't need either python nor ruby!
https://github.com/aurora/rmate


### SSH setup

See the [secure shell](linux-ssh.md) page for details about remotely connecting to other linux machines

### Mail

Basic install includes mailx, mailq, exim4 (MTA), and sendmail.

For outbound mail sent via gmail smtp.  

1. Will need to open outbound port 587 
2. Add this to a file `~/.mailrc`
```
set smtp-use-starttls
set smtp="smtp://smtp.gmail.com:587"
set smtp-auth=login
set smtp-auth-user="someuser@gmail.com"
set smtp-auth-password="someuser's gmail password"
set from="A Name<eaddress@domain.com>"
set ssl-verify=ignore
```

3. For the gmail account your are using to forward mail turn on less secure apps at that gmail account https://www.google.com/settings/security/lesssecureapps
4. Confiure exim4 with this wizard. Be sure to choose "internet site" `sudo dpkg-reconfigure exim4-config`  See link below for details.  Or edit the etc/exim4/update-exim4.conf.conf and then `service exim4 restart`
5. Comment out dns entry in /etc/nsswitch.conf

__Useful commands__

Pipe some text to email
` echo "Some Body Text" | mailx -v -s "Subject" "tosomeone@address.com"`

See what is in the queue that didn't get sent
`mailq`
Send what is in the queue
`sendmail -v -q`


Something stuck in the queue.  Fastest solution to remove all emails in exim queue (less than 5sec) :
do these commands :
````
cd /var/spool
mv exim exim.old
mkdir -p exim/input
mkdir -p exim/msglog
mkdir -p exim/db
chown -R mail:mail exim
/sbin/service exim4 restart
````



Useful links
https://activespark.wordpress.com/2008/07/21/exim-spool-file-is-locked-another-process-is-handling-this-message/

http://www.cyberciti.biz/faq/exim-remove-all-messages-from-the-mail-queue/

http://www.unix.com/unix-for-advanced-and-expert-users/36937-problem-mailx-can-execute-but-email-not-received.html

http://www.cyberciti.biz/tips/force-sendmail-to-deliver-a-message-in-sendmails-mail-queue.html

https://www.digitalocean.com/community/tutorials/how-to-install-the-send-only-mail-server-exim-on-ubuntu-12-04

