
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
4. Confiure exim4 with this wizard. Be sure to choose "internet site" `sudo dpkg-reconfigure exim4-config`  See link below for details.  Or edit the etc/exim4/update-exim4.conf.conf and then `service exim4 restart`  Example below for local only outbound mail from server processes on server.
    ````
    # /etc/exim4/update-exim4.conf.conf
    #
    # Edit this file and /etc/mailname by hand and execute update-exim4.conf
    # yourself or use 'dpkg-reconfigure exim4-config'
    #
    # Please note that this is _not_ a dpkg-conffile and that automatic changes
    # to this file might happen. The code handling this will honor your local
    # changes, so this is usually fine, but will break local schemes that mess
    # around with multiple versions of the file.
    #
    # update-exim4.conf uses this file to determine variable values to generate
    # exim configuration macros for the configuration file.
    #
    # Most settings found in here do have corresponding questions in the
    # Debconf configuration, but not all of them.
    #
    # This is a Debian specific file

    dc_eximconfig_configtype='internet'
    dc_other_hostnames='aws-cloud-kebler'
    dc_local_interfaces='127.0.0.1 ; ::1'
    dc_readhost=''
    dc_relay_domains=''
    dc_minimaldns='false'
    dc_relay_nets=''
    dc_smarthost=''
    CFILEMODE='644'
    dc_use_split_config='false'
    dc_hide_mailname=''
    dc_mailname_in_oh='true'
    dc_localdelivery='mail_spool'
    ````

5. Comment out dns entry in /etc/nsswitch.conf

__Useful commands__

Pipe some text to email
` echo "Some Body Text" | mailx -s "Subject" "tosomeone@address.com"`

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

## Time

see the settings `timedatectl`
set the time zone `dpkg-reconfigure tzdata`

https://www.digitalocean.com/community/tutorials/how-to-set-up-time-synchronization-on-ubuntu-12-04

You can download it from apt-get.

sudo apt-get install ntp

Step Two— Configure the NTP Servers

Once the program is installed, open up the configuration file:

sudo nano /etc/ntp.conf

Find the section within the configuration that lists the NTP Pool Project servers. The section will look like this:

server 0.ubuntu.pool.ntp.org
server 1.ubuntu.pool.ntp.org
server 2.ubuntu.pool.ntp.org
server 3.ubuntu.pool.ntp.org

Each line then refers to a set of hourly-changing random servers that provide your server with the correct time. The servers that are set up are located all around the world, and you can see the details of the volunteer servers that provide the time with the

 ntpq -p

command. You should see something like the following:

     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
-mail.fspproduct 209.51.161.238   2 u   50  128  377    1.852    2.768   0.672
*higgins.chrtf.o 18.26.4.105      2 u  113  128  377   14.579   -0.408   2.817
+mdnworldwide.co 108.71.253.18    2 u   33  128  377   47.309   -0.572   1.033
-xen1.rack911.co 209.51.161.238   2 u   44  128  377   87.449   -5.716   0.605
+europium.canoni 193.79.237.14    2 u  127  128  377   75.755   -2.797   0.718

Although these servers will accomplish the task of setting and maintaining server time, you set your time much more effectively by limiting the ntp to the ones in your region (europe, north-america, oceania or asia), or even to the ones in your country, for example in America:

add
 server us.pool.ntp.org

You can find the list international country codes (although not all of the countries have codes) here

Once all of the information is in the configuration file, restart ntp:

sudo service ntp restart

## SSL

Create key and CSR using openssl

Once you get back your crt and ca-bundle put this in ssl config for the site in apache2


NB:   ca-bundle is the cert chain back to the root authority

      <IfModule mod_ssl.c>
      <VirtualHost *:443>
      ServerName cloud.kebler.net

      DocumentRoot /var/www/owncloud

      SSLEngine on
      SSLCertificateFile /etc/apache2/ssl/cloud.kebler.net.crt
      SSLCertificateKeyFile /etc/apache2/ssl/cloud.kebler.net.key
      SSLCertificateChainFile /etc/apache2/ssl/cloud.kebler.net.ca-bundle
      <FilesMatch "\.(cgi|shtml|phtml|php)$">
       SSLOptions +StdEnvVars
      </FilesMatch>
      <Directory /usr/lib/cgi-bin>
       SSLOptions +StdEnvVars
      </Directory>
      BrowserMatch "MSIE [2-6]" \nokeepalive ssl-unclean-shutdown \downgrade-1.0 force-response-1.0
      # MSIE 7 and newer should be able to use keepalive
      BrowserMatch "MSIE [17-9]" ssl-unclean-shutdown


Add a cert to AWS IAM if need be with AWS CLI like this

      aws iam upload-server-certificate --profile RSK --server-certificate-name cloud.kebler.net --certificate-body file://cloud.kebler.net.crt --private-key file://cloud.kebler.net.key --certificate-chain file://cloud.kebler.net.ca-bundle


      aws iam get-server-certificate --profile RSK --server-certificate-name cloud.kebler.net

      Returns
      {
          "ServerCertificateMetadata": {
              "ServerCertificateId": "ASCAIFAOF7ETNBYKEOMSU",
              "Expiration": "2018-12-16T23:59:59Z",
              "UploadDate": "2016-01-23T20:10:42.637Z",
              "Arn": "arn:aws:iam::245206923545:server-certificate/cloud.kebler.net",
              "Path": "/",
              "ServerCertificateName": "cloud.kebler.net"
          }
      }
