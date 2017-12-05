---
title: The little server than could
date: 2015-12-29
---

### In the beginning

One fine day I decided to set up a personal server, to host my website, send and receive mail, run a mailing list, sync my contacts, calendars, notes and documents, provide instant messaging and video conferencing, and really anything else that I might think of.

### Rationale

I could get these services for free and with the greatest ease from the likes of Google, Dropbox, Skype, Facebook, etc.  Except it turns out the government spies have backdoors into all those services.  Remember, you've nothing to hide, for you've done nothing wrong, right?  Right?  Also, I don't like advertising.  And I don't like these companies selling my personal data to third parties.

Besides, it's also an interesting learning experience as well.  I've been figuring it out as I go, so you'll have to forgive my rambling.

### Go go gadget PI

I thought about renting a virtual private server, but instead I chose to buy a [Raspberry PI 2](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/) with an 8GB SD Card, at a cost of €60.  I thought that was dirt-cheap for a server.  It is also [very cheap to host](/server-hosting/).

The Raspberry PI 2 is not a powerful machine, but as I don't anticipate heavy traffic I think it should be enough for my needs.  I [extended the storage](/expanded-usb-storage/) by adding a USB memory stick.

There are lots of things about the PI that I don't understand.  Between the v1 and v2 the CPU architecture has changed from ARM6 to ARM7.  What does that mean?  Do I care?  I'm not sure.  I think that ARM7 provides better compatibility with a lot of existing software.  Between the v1 and v2 the RAM has increased from 500Mb to 1Gb which I suppose is an improvement.  It never hurts to have more RAM.

### Fiddling the knobs

Speaking of software, I set up the [Linux operating system](/server-operating-system/), the [Apache web server](/web-server/), [Pelican](/web-publishing/) for web publishing, [ownCloud](/client-server-sync/) for document syncing and [GitLab](/version-control/) for source code version control.  Also, [Transmission](/file-sharing/) for file sharing.