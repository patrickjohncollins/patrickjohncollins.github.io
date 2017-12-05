Title: Secure those sockets
Slug: network-encryption
Date: 30-12-2015

Is it a good idea to encrypt my data when stored or sent over the wire?  Would it be a good idea for encryption to be turned on by default?  Do I really have secrets to hide?  I need to think some more about this.

Nevertheless, I decided to acquire a certificate.  I got one for free from my [domain registrar](/domain/), although it is only valid for a year, after which I will have to pay.  The process was quite complicated: generating a private key, submitting it for signing, receiving the signed copy, uploading it to [my server](/server/).  I should have written it all down.

Currently I have the certificate files and the chain files in the ssl folder inside my owncloud documents.  I included the certificate in my [web server](/web-server/) configuration.

I've been eyeing the [Let's Encrypt](http://letsencrypt.org) project for a while.  I understand that their certificates are free, and also the process looks simpler.  Unfortunately I wasn't able to install the client.

I understand that I am running Debian 7 (Wheezy), for which there is no package available.  So I ran the script instead: 

	wget https://dl.eff.org/certbot-auto
	chmod a+x certbot-auto

	./certbot-auto

I saw the following error message:

	No libaugeas0 version is available that's new enough to run the Certbot apache plugin...

Which lead me to [this thread](https://community.letsencrypt.org/t/letsencrypt-auto-just-checks-dependencies-nothing-more/7819).  Apparently I'm meant to install Python 2.7 or use --debug or some such nonesense.  I tire of this shit for today.
