Title: Web publishing
Slug: web-publishing
Date: 31-12-2015

Writing HTML by hand is painful, so I write the articles for my web site in [Markdown](http://daringfireball.net/projects/markdown/syntax) format using the Sublime Text app on my MacBook, which is less painful.  It would be nice to have a side-by-side live-preview pane.  Even better would be an app that provides a WYSIWYG editor.  Inline-editing in the browser would be ideal!  I'll carry on dreaming though.

I run [Pelican](http://blog.getpelican.com) on my MacBook to convert the articles into HTML, which I can then preview offline in my web browser.  During the conversion Pelican applies a theme, which controls the visual appearance of the generated page.   I [designed my own theme](/web-design/).

When online, the articles are uploaded automatically using [ownCloud](http://localhost/client-server-sync/).  I then run Pelican on my server to generate the live site which is served to the general public by [my web server](/web-server/).

Arguably, I don't need to run Pelican on my server, as I could simply upload the locally-generated version of my web site.  But, I might like to try writing articles on my smartphone using a wireless keyboard in the future.





### Installation

To install Pelican on my server, I first had to install Pip, a package manager for Python apps.  More complications!  Why couldn't I just use apt-get direct

	sudo apt-get install python-pip

TODO: How did I install pip on my Macbook?  I used Homebrew!

Then I used Pip to install Pelican and the Markdown module:

	pip install pelican
	pip install Markdown

### Configuration

TODO: Pelican configuration files
TODO: Theme

### Execution

The following command starts Pelican and keeps it running.

	sudo pelican content -s publishconf.py -r

TODO: Automatically run Pelican on system boot
http://raspberrypi.stackexchange.com/questions/37696/automatically-run-pelican-on-system-boot/37714#37714


