# Udacity-Linux-Server

## Access

* IP Address - ```34.233.187.206```
* [Catalog App Project URL](http://www.mag4417online.com/)
* [Neighborhood Map Project URL](http://www.mag4417online.com/map/)
* [Portfolio Project URL](http://www.mag4417online.com/portfolio/)

## Installed Software/Packages

`history | grep "apt-get install"`

* finger
* apache2
* libapache2-mod-wsgi
* libapache2-mod-security2
* postgresql
* postgresql-contrib
* git

## Configurations

* Disabled `root` remote login by adding `PermitRootLogin no` to `sshd_config`.
* Added user `grader` and added as `sudoer`.
* Only allow connections for `SSH` on port 2200, `HTTP` on port 80, and `NTP` on port 123.
* Enforced key-based `SSH` authentication by setting `PasswordAuthentication no` in `ssh_config`.
* Created `catalog` user with permissions for `catalog` database.
* Cloned `https://github.com/sm9378/catalog.git` to `/var/www/catalog`.
* Created `catalog.wsgi` at `/var/www/catalog/catalog.wsgi`:

  ```
  import sys
  import logging
  sys.path.insert(0, '/var/www/catalog/')
  from catalog import app as application
  application.secret_key = 'super_secret_key'
  ```
  
* Created `catalog.conf` at `/etc/apache2/sites-available/catalog.conf`:
 
  ```
  <VirtualHost *:80>
	        ServerName www.mag4417online.com
	        ServerAdmin mag4417@bellsouth.net
	        WSGIScriptAlias / /var/www/catalog/catalog.wsgi
	        <Directory /var/www/catalog/catalog/>
		              Order allow,deny
		              Allow from all
	        </Directory>
	        Alias /static /var/www/catalog/catalog/static
	        <Directory /var/www/catalog/catalog/static/>
		              Order allow,deny
		              Allow from all
	        </Directory>
	        Alias /map /var/www/map/html
	        <Directory /var/www/map/html/>
		              Order allow,deny
		              Allow from all
	        </Directory>
	        Alias /portfolio /var/www/portfolio/html
	        <Directory /var/www/portfolio/html/>
		              Order allow,deny
		              Allow from all
	        </Directory>
	        ErrorLog ${APACHE_LOG_DIR}/error.log
	        LogLevel warn
	        CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
  ```

* Added files to `/var/www/map/html/` to serve the Neighborhood Map Project page.
* Added files to `/var/www/portfolio/html/` to serve the Portfolio Project page.

## Resources

* [Google Code Archive](https://code.google.com/archive/p/modwsgi/wikis/QuickConfigurationGuide.wiki#Mounting_At_Root_Of_Site) - Configuration guide for mod_wsgi
* [Udacity](https://classroom.udacity.com/nanodegrees/nd004/parts/ab002e9a-b26c-43a4-8460-dc4c4b11c379) - Deploying to Linux Servers Course
* [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-use-roles-and-manage-grant-permissions-in-postgresql-on-a-vps--2) - Article on PostgreSQL Roles and Permissions

  
