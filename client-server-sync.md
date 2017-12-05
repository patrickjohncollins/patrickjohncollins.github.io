Title: My data, on my server
Slug: client-server-sync
Date: 31-12-2015

I wanted to sync my data between my terminals (currently a MacBook Air and an iPhone) and [my server](/server/).  This provides me with a backup copy of my data that I can access from anywhere.

I decided to install ownCloud.  The standard Debian package management servers don't have the latest version of ownCloud, so I had to add a reference to the ownCloud package on the openSuse servers using the following commands:

	wget http://download.opensuse.org/repositories/isv:ownCloud:community/Debian_8.0/Release.key
	apt-key add - < Release.key 
	echo 'deb http://download.opensuse.org/repositories/isv:/ownCloud:/community/Debian_8.0/ /' >> /etc/apt/sources.list.d/owncloud.list 
	apt-get update

And then I could install ownCloud as follows:

	sudo apt-get install owncloud

I then launched my browser, and navigated to the address http://193.150.14.102/owncloud, where I created a user account.

I mapped the owncloud folder to my DNS subdomain cloud.pjcollins.org.

	cd /etc/apache2/sites-available
	sudo cp default owncloud

	sudo nano owncloud

Instead the element <VirtualHost *:80> I updated the following settings:

    ServerAdmin pjc@pjcollins.org
    DocumentRoot /var/www/owncloud

I also added these two settings:

    ServerName cloud.pjcollins.org

TODO: I think "cloud" is stupid.  Think of something better.

And I modified the directory element to point to the website directory:

	<Directory /var/www/owncloud/>

Enable the site:

	sudo a2ensite owncloud

Reload web server:

	sudo service apache2 reload

Then I opened my browser (Safari) and navigated to:
	
	http://cloud.pjcollins.org

I had to click on the button to add "cloud.pjcollins.org" as trusted domain.  I was redirected to the following address, where I was able to update the needed setting.

	http://193.150.14.102/index.php/settings/admin?trustDomain=cloud.pjcollins.org

I downloaded and installed the ownCloud desktop client for Mac OSX and pointed it to my new server.  Now files are automatically syncronized between client and server.  I can even edit files offline and have them sync when I reconnect.  Magic!

### Moving data to external storage

I decided to move all owncloud data to my [external USB storage](/expanded-usb-storage/).  First I had to stop the web server:

	sudo service apache2 stop

Then I edited the owncloud config, changing the data directory setting.

	sudo nano /var/www/owncloud/config/config.php
 	'datadirectory' => '/var/www/owncloud/data',
    'datadirectory' => '/media/usbhdd/owncloud/data',

Then I created the new data directory, moved the existing data across, and ensured that the web server has rights to the new directory.

    sudo mkdir /media/usbhdd/owncloud
	sudo mv /var/www/owncloud/data /media/usbhdd/owncloud/data
	sudo chown -R www-data:www-data /media/usbhdd/owncloud/data

I restarted the web server.

	sudo service apache2 start


