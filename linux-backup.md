# Backup

## Local-Desktop

Use Backintime
http://backintime.le-web.org/

Deja Dup
https://launchpad.net/deja-dup
http://www.howtogeek.com/108869/how-to-back-up-ubuntu-the-easy-way-with-dj-dup/




## Servers

### Duplicity

http://duplicity.nongnu.org/
Install via this ppa
https://launchpad.net/~duplicity-team/+archive/ubuntu/ppa
http://duplicity.nongnu.org/duplicity.1.html#toc

Backup Script
duplicity-backup.sh
https://github.com/zertrin/duplicity-backup


Useful Links

http://scie.nti.st/2013/4/13/using-duplicity-for-full-server-backup-on-ubuntu-12-dot-04/

http://old.blog.phusion.nl/2013/11/11/duplicity-s3-easy-cheap-encrypted-automated-full-disk-backups-for-your-servers/


## Backup Scripts

Install latest Duplicity (from ppa)
add this to a file in sources.list.d  
File: duplicity-team-ppa-trusty.list
````
deb http://ppa.launchpad.net/duplicity-team/ppa/ubuntu trusty main
# deb-src http://ppa.launchpad.net/duplicity-team/ppa/ubuntu trusty main
````

Create IAM user and policies plus backup bucket(s).

Install AWS-CLI, 
install python if need be.  need 2.7.6+
install pip then aws-cli
apt-get install python-pip
pip install awscli  (pip install --upgrade awscli
pip install --upgrade awscli
)
Install S3cmd (for duplicity-backup at least for now)
pip install s3cmd

Install boto 2.x
pip install boto

run s3cmd --configure   and add the access and secret keys
Do same, add credentials to ~/.aws/

copy scripts directory your /root/backup-scripts
cd to directory
Run ./install.sh which will make symbolic links to cron.d for backup cron job and for dupbu and ec2-snapshot to usr/local/bin
````
ln -s /root/backup-scripts/bin/ec2-snapshot.sh /usr/local/bin/ec2snap
ln -s /root/backup-scripts/bin/dupbu.sh /usr/local/bin/dupbu
ln -s /root/backup-scripts/backup.cron /etc/cron.d/backup
````

Create and IAM policy.  Attach policy below to a new or exisiting IAM account.  
Set up the .aws credentials and config files.  Can use default profile or named profile.
http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html

This policy will allow access to backup bucket and to ec2 snapshot commands
replace with your bucket name (can be more than one)

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ec2:DescribeVolumes",
                "ec2:CreateSnapshot",
                "ec2:DescribeSnapshots",
                "ec2:DeleteSnapshot",
                "ec2:CreateTags",
                "ec2:DescribeTags"
            ],
            "Effect": "Allow",
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": "s3:ListAllMyBuckets",
            "Resource": "arn:aws:s3:::*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::server.kebler.net",
                "arn:aws:s3:::cloud-data.kebler.net"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:AbortMultipartUpload",
                "s3:DeleteObjectVersion",
                "s3:GetObjectAcl",
                "s3:GetObjectVersion",
                "s3:GetObjectVersionAcl",
                "s3:PutObjectAcl",
                "s3:PutObjectAclVersion"
            ],
            "Resource": [
                "arn:aws:s3:::server.kebler.net/*",
                "arn:aws:s3:::cloud-data.kebler.net/*"
            ]
        }
    ]
}


