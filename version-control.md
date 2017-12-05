Title: Version control
Slug: version-control
Date: 2016-03-15

I wanted to my own source code version control system, so I decided to install [GitLab](https://gitlab.com) on my [server](/server/).

I followed the [installation instructions](https://about.gitlab.com/downloads/#raspberrypi2), and executed the following commands:

	sudo apt-get install curl openssh-server ca-certificates postfix apt-transport-https
	curl https://packages.gitlab.com/gpg.key | sudo apt-key add -
	sudo curl -sS https://packages.gitlab.com/install/repositories/gitlab/raspberry-pi2/script.deb.sh | sudo bash
	sudo apt-get install gitlab-ce

I then edited the following configuration file: 

	sudo nano /etc/gitlab/gitlab.rb

I added a setting for the [external URL](http://doc.gitlab.com/omnibus/settings/configuration.html#configuring-the-external-url-for-gitlab) and another to [move to the data directory](http://doc.gitlab.com/omnibus/settings/configuration.html#storing-git-data-in-an-alternative-directory) to my [USB memory stick](/expanded-usb-storage/).  I also [disabled the bundled Nginx web server](http://doc.gitlab.com/omnibus/settings/nginx.html#using-a-non-bundled-web-server) because I am [already running Apache](/web-server/).

	external_url "http://gitlab.pjcollins.org"
	git_data_dir "/media/usbhdd/git-data"
	nginx['enable'] = false
	web_server['external_users'] = ['www-data']
	gitlab_workhorse['listen_network'] = "tcp"
	gitlab_workhorse['listen_addr'] = "127.0.0.1:8181"

To enable Gitlab in Apache I had to retrieve the correct settings file from the [GitLab recipes folder](https://gitlab.com/gitlab-org/gitlab-recipes/tree/master/web-server/apache).  I wasn't sure what version of Apache I was running so:

	apachectl -V
	Server version: Apache/2.2.22 (Debian)

I therefore copied the contents of the file "[gitlab-omnibus-apache22.conf](https://gitlab.com/gitlab-org/gitlab-recipes/blob/master/web-server/apache/gitlab-omnibus-apache22.conf)" and replaced "YOUR_SERVER_FQDN" with "gitlab.pjcollins.org".  I'm not sure what "omnibus" is, perhaps some kind of packaging tool?  I don't even want to know.

I created a new site in Apache:

	cd /etc/apache2/sites-available
	sudo cp default gitlab
	sudo nano gitlab

I deleted all the lines and pasted in the edited setting files.  I activated the new site:

	sudo a2ensite gitlab

I had to enable the Apache proxy module:

	sudo a2enmod proxy_http

I reloaded Apache:
	
	sudo service apache2 reload

I then applied the changes to Gitlab:

	sudo gitlab-ctl reconfigure


