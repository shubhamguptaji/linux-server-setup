# IP Address
 http://35.200.166.236

# Softwares Installed
- pip - sudo apt-get install python-pip
- flask - sudo -H pip install flask
- httplib2 - sudo -H pip install httplib2
- oauth2client - sudo -H pip install oauth2client
- psycopg2 - sudo -H pip install psycopg2
- sqlalchemy - sudo -H pip install sqlalchemy
- sqlalchemy_utilities - sudo -H pip install sqlalchemy_utils
- python dependencies sudo -H pip install libpq-dev python-dev
- postgres - sudo -H install postgresql postgresql-contrib
- Apache - sudo -H install apache2
- Mod-WSGI - sudo -H install libapache2-mod-wsgi
- Python virtual enviroment - sudo -H pip install virtualenv venv

# Configurations Made
1. Grant sudo access to grader
  * Create a new file grader in `\etc\sudoers.d`
  *  Write `grader ALL=(ALL) NOPASSWD:ALL` in the grader file.
  * Save the file
2. Disable remote login of root user
  * Run `sudo nano /etc/ssh/sshd_config`
  * Set `PermitRootLogin` to `no`
  * Run `sudo service ssh restart`
3. Force Key Based Authentication
  * Run `sudo nano /etc/ssh/sshd_config`
  * Set `PassowordAuthentication` to `no`
  * Set `UsePAM` to `no`
  * Run `sudo service ssh restart`
4. Enable Mod-WSGI
  * `sudo a2enmod wsgi`
5. Create Database
  * `create user guptaji with encrypted password <password>``
  * `create database shopdata`
  * `grant all on database shopdata to guptaji`
  * `\c shopdata`
  * `revoke all on schema public from public`
  * `grant all on schema public to guptaji`
6. Change SSH Port
  * Edit the `/etc/ssh/sshd_config` file.
  * Change the `Portno` to `2200`.
  * Save the file.
  * To restart ssh or sshd run `sudo service ssh restart` and `sudo service sshd restart`
7. Configure Firewall
  * `sudo ufw allow 2200/tcp`
  * `sudo ufw allow www`
  * `sudo ufw allow ntp`
  *  `sudo ufw enable`
8. Configure WSGI file
  * Run the command - `sudo nano /var/www/shop/shop.wsgi`
  * Add the following lines of code
  ~~~
  import logging
  logging.basicConfig(stream=sys.stderr)
  sys.path.insert(0,'/var/www/shop/')  
  from project import app as application
  ~~~
9. Setup Virtual Environment
  * ` cd venv/lib/python2.7`
  * `source venv/bin/activate`
  * `sudo service apache2 restart`
  * `deactivate`
10. Edit Apache Configuration File
  * Run the command - `sudo nano /etc/apache2/sites-available/000-default.conf`
  * Make the following changes and then run `sudo service apache2 restart`
  * ~~~
    ServerName 35.200.166.236
    ServerAdmin shubham2604gupta@gmail.com
    WSGIDaemonProcess shop python-path=/var/www/shop:/var/www/shop/venv/lib/python2.7/site-packages
    WSGIProcessGroup shop
    WSGIScriptAlias / /var/www/shop/shop.wsgi  
    <Directory /var/www/shop/>
            Order allow,deny
            Allow from all
    </Directory>
    Alias /static /var/www/shop/static
    <Directory /var/www/shop/static/>
            Order allow,deny
            Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    ~~~
