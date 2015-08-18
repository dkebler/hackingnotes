## Apache Server

directory password access 
.htaccess for password protected directory

    AuthUserFile path/to/.htpasswd
    AuthName "Members Only"
    AuthType Basic
    require valid-user