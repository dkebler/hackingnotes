
### SSH setup

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

