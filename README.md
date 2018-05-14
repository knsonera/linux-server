# Linux Server Configuration Project

## How to access the server via SSH:
IP: 34.217.35.161
Port: 2200

## How to access the web app:
Please use this link: http://uzhvieva.com/

## Configuration changes:
### .ssh/authorized_keys
- added rsa key to .ssh/authorized_keys file
- changed permissions of the file (sudo chmod 600)
### /etc/ssh/sshd_config 
- default SSH port changed to 2200 (Port 2200)
- password authentication disabled (PasswordAuthentication no)
- root login disabled (PermitRootLogin no)
### /etc/sudoers.d/grader
- added grader file to sudoers.d
- gave root access to grader user (grader ALL=(ALL) NOPASSWD:ALL)
### UFW
- set the defaults used by UFW
- allowed connections via 2200, 80, 123 ports
- disabled connections via 22 port
### /etc/apache2/sites-available/helper.conf
- added configuration file for my web app:
```
<VirtualHost *:80>
                ServerName 34.217.35.161
                WSGIScriptAlias / /var/www/helper/helper.wsgi
                <Directory /var/www/helper/helper/>
                        Order allow,deny
                        Allow from all
                </Directory>
                Alias /static /var/www/helper/helper/static
                <Directory /var/www/helper/helper/static/>
                        Order allow,deny
                        Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- disabled default config (sudo a2dissite 000-default.conf)
- enabled my web app (sudo a2ensite helper.conf)
- restarted apache2 service
### application files
- added helper directory to /var/www/
- added helper.wsgi file to helper app
```
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/helper/")

from helper import views
application = views.app
```
## Additional software and libraries:
- python
- python-sys
- python-random
- python-string
- python-json
- python-requests
- python-virtualenv
- python-pip
- python-flask
- python-flask-sqlalchemy
- python-httplib2
- python-itsdangerous
- python-oauth2client
- python-passlib
- python-psycopg2
- python-sqlalchemy
- python3
- apache2
- git
- postgresql-9.5
- sqlite3
