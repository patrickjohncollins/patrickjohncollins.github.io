---
title: Server operating system
date: 30-12-2015
---

[My server](/server/) is running Raspbian, a derivative of Debian, one of the many flavours of Linux.  I knew nothing about Linux before I began setting up my server.  I had a fair amount of prior experience with Microsoft Windows and Apple OSX.  Is Linux any better than those two?  I worried that it would be complicated to set up.  It was.  Though there are many tutorials online, the diversity of options is staggering.  It seems there is no single right way of doing anything, rather there are hundreds of options and possibilities.  Do I have time to go through all of them?

Raspbian has been tailored specifically for the Raspberry PI, although I don't know exactly what this means.  I understand that Ubuntu also runs on the PI 2, and I might have preferred that, but I didn't have the courage to try and install it.  Furthermore, only the Snappy Core version is supported, which I understand makes installing programs a bit more complicated.

I've configured my server through the command line interface, which I hate.  Typing commands by hand reminds me of the bad days of MS DOS before Windows even existed.  I much prefer using graphical interfaces, ideally touch-sensitive, allowing me to poke at pictures with my fingers.  Perhaps I should install Webmin?  In the meantime I've been using the "Terminal" app on my Mac.

I typed the following command:

	ssh root@193.150.14.102

The *ssh* command opens a connection to the server.  After the connection is opened, any subsequent commands will be executed directly on the server.  *root* is the name of the administrator user account.  The at sign @ is followed by the server IPv4 address, which I received in [an email from my host](/server-hosting/#some-funny-numbers).  Then I entered the password from the email.  I entered the following command to check for any updates:

	sudo apt-get update

And then the following to apply those updates:

	sudo apt-get upgrade

The "sudo" command provides temporary administrative privileges.  The "apt-get" command communicates with remote package management servers.  I think that these servers are run by the Debian foundation, and host the latest versions of a wide variety of free software.

I entered the following commands to create a new user account.  The name "newuser" is just an example, I've decided not to expose my real user name.

	adduser newuser

I grant the user administrative privileges as follows:

	usermod -a -G sudo newuser

Then I disconnect and reconnect as the new user:

	logout
	ssh newuser@193.150.14.102

I decided to disable ssh access for the root user.  This is a precautionary measure taken to limit my exposure to hackers.  I opened the ssh configuration file in the nano text editor using the following command:

	sudo nano /etc/ssh/sshd_config

From there I moved the cursor with the arrow keys, and modified the value of the "PermitRootLogin" setting to "no". 

	PermitRootLogin no

Then I pressed Ctrl+X followed by return to save the changes.  I restarted the ssh service as follows:

	sudo service ssh restart

** TODO More security - ssh key pairs, firewalls, prevents dictionary attacks


I blindly followed [the Linode guide](https://www.linode.com/docs/security/securing-your-server/) to secure the server, but man, what a pain in the ass!  Can't they automate that process?

I also [enabled automatic updates](https://help.ubuntu.com/10.04/serverguide/automatic-updates.html), because that seems like a good way to stay secure.  Actually, that doesn't seem to work.
