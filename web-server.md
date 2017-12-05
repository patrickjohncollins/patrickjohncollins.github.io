Title: Web server
Slug: web-server
Date: 30-12-2015

I installed Apache on [my server](/server/) as follows:

	sudo apt-get install apache2

I pointed my web browser (Safari) at the address http://193.150.14.102 and I saw a page saying "It works!" which was reassuring.  For information, the page displayed is located in /var/www/index.html.

I understand that installing Apache creates a new user group called "www-data".  I decided to add myself to that group:

	usermod -a -G www-data newuser

I decided to create a new folder for my website:

	cd /var/www
	mkdir website

Then I gave ownership of the folder to the www-data user/group and allowed writes:

	sudo chown -R www-data:www-data /var/www/website
	sudo chmod 775 /var/www/website

I [acquired an encryption certificate](/network-encryption/).  I enabled the SSL module in Apache:

	sudo a2enmod ssl

I created a new configuration file for my site in order to map my domain name pjcollins.org and to ensure that SSL is active:

	cd /etc/apache2/sites-available
	sudo cp default-ssl website-ssl
	sudo nano website-ssl

Inside the element <VirtualHost _default_:443> I updated the values of the following settings:

    ServerAdmin pjc@pjcollins.org
    DocumentRoot /var/www/website

I also added these two settings:

    ServerName pjcollins.org
    ServerAlias www.pjcollins.org

And I modified the directory element to point to the website directory:

	<Directory /var/www/website/>

Further down, I entered the following values:

    SSLCertificateFile    /media/usbhdd/owncloud/data/pjc/files/Documents/ssl/pjcollins.includesprivatekey.pem
    SSLCertificateChainFile /media/usbhdd/owncloud/data/pjc/files/Documents/ssl/GandiStandardSSLCA2.pem

Save & exit.  Then I created a second configuration file to automatically redirect any HTTP requests to HTTPS:

	sudo cp default website
	sudo nano website

I removed everything and replaced with the following configuration:

	<VirtualHost *:80>
        ServerName pjcollins.org
        Redirect permanent / https://pjcollins.org/
	</VirtualHost>

Save & exit.  Then I restarted Apache.

Enable the sites:

	sudo a2ensite website
	sudo a2ensite website-ssl

Disable the default site:

	sudo a2dissite default
	sudo a2dissite default-ssl

Reload web server:

	sudo service apache2 reload