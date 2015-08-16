## EC2 Virtual Machines

### List of Tasks to execute on base Ubuntu Machine

* Note: Base machine has Temp-EC2-Key and user "ubuntu" as a default.
* Set up entry in config file for easy ssh access.
* Copy aliases to script in etc/profile.d, might want other scripts in here that get executed on login terminal
* Overwrite skel directory files with pertinent versions so new users get various bash files by default
* create admin user and copy over a public key to authorized key file.
* If direct root access wanted write  over authorized key file in root account with a key
* Install node.js and npm
* Install git





     #adduser sadmin

```
server adminitrator username: sadmin
password: sadmin0z
Key(s) : temp-sadmin(.pub)
```

config file in .ssh
```
#login for AWS base machine (IP may change not using elastic IP)
Host base
# needed for using rsub with sublime
   RemoteForward 52698 localhost:52698
   user sadmin  
# change IP to IP of your running instance
   hostname 54.68.170.205
   IdentitiesOnly=yes 
# change this to your personal private key once added to authorized keys   
   IdentityFile ~/.ssh/temp-sadmin
```

Todo:
Add your public key to sadmin's authorized_keys file (delete the temporary one)
Change your config file.


    cat ~/.ssh/<your private key> | ssh sadmin@base 'umask 077; cat >>.ssh/authorized_keys'
 
check that you can an now ssh, then you can remove the temp-sadmin key.  




## Building Ubuntu Server

started with ubuntu 14.10 image
