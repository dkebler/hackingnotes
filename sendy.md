
# Sendy
** web application for bulk email campaigns **

## Cron Job Setup



Sharing this for others. I was having woes getting sendy to actually send a campaign even though the little blue box disappeared, the cron job was active! but that doesn't mean it is actually working

Here was my issue.

The cron line advice they give in the popup dialog box is a bit vague (misleading) ``` */5 * * * * php "and more stuff here"```

It's this part php that was the issue. It assumes that an alias in the path will launch the installed copy of php and this might not be true on your system.

First Make sure you have the php cli loaded (version 5 in my case) ``` sudo apt-get install php5-cli ```

then find out where it is ``` :/$ whereis php5 ```

then edit your crontab ``` sudo crontab -e ```

copy and paste the entire example cron job from the popup dialog. ``` */5 * * * * php "your full sendy install path"/scheduled.php > /dev/null 2>& ```

change php to the first path listed from whereis php5 in my case ``` /usr/bin/php5 ```

That did it....no more endless preparing.

-----------
Ruby interface to Sendy
https://github.com/cmer/sendyr
