// Working install on AWS / Ubuntu 14.0.4 system

// important references:

// https://github.com/specify/web-asset-server

// https://github.com/specify/webportal-installer


// install basic requirements for Web Portal

sudo apt-get update

sudo apt-get install apache2

sudo apt-get install tomcat7

sudo apt-get install make python2.7 default-jdk unzip curl git

sudo apt-get install virtualbox-guest-utils (VirtualBox only)

git clone https://github.com/specify/webportal-installer.git


// copy a zip file to your exports folder

// this could be grabbed from some other site using curl /wget

// or scp from command line

cp xxx.zip webportal-installer/specify-exports

cd webportal-installer

make clean && make


// install FTP for uploading new zip files

sudo apt-get update

sudo apt-get install vsftpd

sudo vi /etc/vsftpd.conf


// CONFIGURE THESE SETTINGS IN vsftpd.conf

// note: be sure to open ports 20,21 and 1024-1048 on your firewall

// anonymous_enable=NO

// local_enable=YES

// write_enable=YES

// chroot_local_user=YES

// local_root=/home/ubuntu/webportal-installer/specify_exports

// pasv_enable=YES

// pasv_min_port=1024

// pasv_max_port=1048

// pasv_address=rsaherbarium.org

// pasv_addr_resolve=YES


// restart the ftp server

sudo service vsftpd restart


// create a new FTP user

sudo useradd -m rsabg -s /usr/sbin/nologin

sudo passwd rsabg

// 'jaowndi6'


// simple update script update_webportal.sh

// #!/bin/sh

// cd /home/ubuntu/webportal-installer

// make && make update

// create a cron job


crontab -e

// example: update every 5 minutes

// */5 * * * * /home/ubuntu/update_portal.sh >> /home/ubuntu/update_portal.log 2>&1
