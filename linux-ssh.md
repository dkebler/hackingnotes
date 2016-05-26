
# Secure Shell accesss

## SSH Server Setup

if an ssh server is not available then install one.  if one is installed it may be set up for password accesss which is how you can enter initially,  but then you want to use a keypair and turn off the password.

to change over the password default login to one using public/private keys
edit this

sudo rsub /etc/ssh/sshd_config    (assumes using rsub and sublime for editing)

Then change these lines to
```
# Disable protocol 1 RSA key based authentication
RSAAuthentication no
# Protocol 2 public key based authentication
PubkeyAuthentication yes

# Change to no to disable tunnelled clear text passwords
PasswordAuthentication no
```
don't set the authorized_keys location, it will be the .ssh directory of the user you used to ssh.  (mostly likely user ubuntu).  Then restart the server

```
sudo service ssh restart
```
if you need to temporary have login access set the pass


http://hacktux.com/passwordless/ssh


 Installation of the OpenSSH client and server applications is simple. To install the OpenSSH client applications on your Ubuntu system, use this command at a terminal prompt:
```
sudo apt-get install openssh-client
```
To install the OpenSSH server application, and related support files, use this command at a terminal prompt:
```
sudo apt-get install openssh-server
```
The openssh-server package can also be selected to install during the Server Edition installation process.



This is easy way to copy a public key to server.  Should go right into authorized_keys file

```
ssh-copy-id -i ~/.ssh/<name>.pub <Host Name from Config file>
```

or if ssh-copy-id is not available try this

```
cat ~/.ssh/<keyfilename>.pub | ssh user@<host name from config file> 'umask 077; cat >>.ssh/authorized_keys'
```

Set up a config file on client for easy access
http://www.cyberciti.biz/faq/create-ssh-config-file-on-linux-unix/
http://linux.die.net/man/5/ssh_config
Example

```
# command to launch is  $ssh vmus
Host vmus
#login user on server
   user ubuntu  
# IP address is machine specific or machine may have domain name - change it.
   hostname 192.168.0.15
# Next line deals with too many key files in client ssh directory
   IdentitiesOnly=yes
#  This one points to specific identity file,
# not needed if client doesn't have too many private key files   
   IdentityFile ~/.ssh/dkebler
```

Here is info on using ssh-agent
http://rabexc.org/posts/using-ssh-agent
https://github.com/ccontavalli/ssh-ident

## Set up for SFTP for website folder access

1.  Add a group `webmaster`
    Edit /etc/ssh/sshd_config and add an entry like this.
        #Subsystem sftp /usr/lib/openssh/sftp-server
        Subsystem sftp internal-sftp

        Match group webmaster
                ChrootDirectory /var/www
                ForceCommand internal-sftp -l VERBOSE -f LOCAL6
                PasswordAuthentication no
                RSAAuthentication yes
                PubkeyAuthentication yes
                AuthorizedKeysFile /home/sftp_users/www/authorized_keys
                X11Forwarding no
                AllowTcpForwarding no
    restart the server            
2.  - add a `home/sftp_users/www` folder and put an `authorized_keys` file
    - chown that file to root:webmaster can set permissions to 640
3.  - Add a new user make their home directory /var/www and add them to the webmaster group
              useradd username -d /var/www -G webmaster
    - Add them to the webmaster group and whatever group you set up for particular website (subdirectory) access.  (e.g. poh)
             groupadd poh
             usermod username -a -G poh
    - add that person's pub key to that authorized_keys file  (copy file to /home/sftp_users/www/ ) then from prompt `cat pubkeyfilename >> authorized_keys`                 
4.  - in the `/var/www` directory to give them write access (they WILL have read only access to ALL folders under /var/www) to a particular folder chown like this e.g  root:poh.   
    - Now set the permissions for that directory to 775  (careful if you hammer the everyone read/excute permission there will be now browser access to the site!)
5.  Set them up a config file for their .ssh directory to make it easy to access like this.

          Host poh
          user jimshoe
          hostname bigcoolstuff.com
          IdentitiesOnly=yes
          IdentityFile ~/.ssh/jimshoeprivatekey    
6.  From the command prompt they can type `sftp poh`   or in nemo "connect to server" enter host `poh` select ssh (port 22)     
7.  Besides the website write access you will grant, make a temp folder with chown to root:webmaster with 775 where everyone can keep stuff temporarily outside the actual website folder    
