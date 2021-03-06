# Working install on AWS / Ubuntu 14.0.4 system

// install basic requirements for Web Portal

sudo apt-get update

sudo apt-get install apache2 tomcat7 make python2.7 default-jdk unzip curl git

git clone https://github.com/specify/webportal-installer.git


// copy a zip file to your exports folder

// this could be grabbed from some other site using curl /wget

// or scp from command line

cp xxx.zip webportal-installer/specify-exports

cd webportal-installer

make clean && make

sudo make install && sudo invoke-rc.d tomcat7 restart


// install FTP for uploading new zip files

sudo apt-get install vsftpd

sudo vi /etc/vsftpd.conf


// CONFIGURE THESE SETTINGS IN vsftpd.conf

// note: be sure to open ports 20,21 and 1024-1048 on your firewall for PASV connections

anonymous_enable=NO

local_enable=YES

write_enable=YES

chroot_local_user=YES

local_root=/home/ubuntu/webportal-installer/specify_exports

allow_writeable_chroot=YES

pasv_enable=YES

pasv_min_port=1024

pasv_max_port=1048

pasv_address=YOUR_DOMAIN

pasv_addr_resolve=YES


// restart the ftp server

sudo service vsftpd restart


// create a new FTP user

sudo useradd -m rsabg -s /usr/sbin/nologin

sudo passwd rsabg


// simple update script update_webportal.sh

  #!/bin/sh

cd /home/ubuntu/webportal-installer

make && make update

// create a cron job

crontab -e

// example: update every 5 minutes

// */5 * * * * /home/ubuntu/update_portal.sh >> /home/ubuntu/update_portal.log 2>&1


// example setup Apache <=> Tomcat reverse proxy

sudo a2enmod proxy_http

sudo apachectl restart

cd /etc/apache2

vi sites-enabled/000-default.conf

// mod_proxy setup

ProxyRequests Off

ProxyPass / http://localhost:8080

ProxyPassReverse / http://localhost:8080

<Location "/">

  Order allow,deny
  
  Allow from all
  
</Location>




