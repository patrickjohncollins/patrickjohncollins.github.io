Title: My name and some funny numbers
Date: 29-12-2015
Slug: domain

### My name

I registered my domain *pjcollins.org* through the registrar [Gandi](http://www.gandi.net).  I chose Gandi for no good reason other than I liked their slogan: "No Bullshit".  I do wish more companies would have such ballsy slogans.  Their website is simple to use.  

I suppose I was quite lucky to find an unregistered domain based on my [rather common name](https://www.google.com/search?q=patrick+john+collins).  Though I am an individual, not an organization, I chose a *.org* domain because I couldn't find another [generic top-level domain](https://en.wikipedia.org/wiki/Generic_top-level_domain) that suited me.  I was tickled pink by the *.guru* and *.ninja* domains, yet potentially confusing and furthermore unjustifiably expensive compared to *.org*.

I have to pay €14 a year, every year, forever.  I thought about paying for 10 years up front, but that's enough time to forget about the matter completely.  Should I forget then I'd forfeit my domain and any links to my site would be broken.  I assume the price will increase over time.  I wonder how this domain could be kept alive after my death.

Considering that there are more than 10 million .org domains registered, that brings in a potential *€140M / year*.  I suppose there is some administration work done by the likes of [ICANN](https://www.icann.org), the [Public Interest Registry](http://pir.org), and the resellers like Gandi, but seriously, are they really providing that much value?  I suspect we are being taken for a ride.

### Some funny numbers

Once registered, a domain must be hosted.  I chose to host my domain with Gandi, as they provide the service at no extra cost.  I wasn't sure if there would be any advantage to hosting on [my server](/server/).  Gandi also provide a tool to edit the zone file.  This is where things get very confusing.  Quite bizarre frankly.  A records?  MX records?  CNAME records?  It's enough to turn your [head inside-out](http://tools.ietf.org/html/rfc882)!

I set up a zone file as follows :

Name|Type|Value
---|---|---
@|A|193.150.14.102
*|A|193.150.14.102
@|AAAA|2001:67c:24f4:c602::49:1
*|AAAA|2001:67c:24f4:c602::49:1

The type "A" records associate my domain with the [IPv4 address of my server](/server-hosting/#some-funny-numbers).  The type "AAAA" records do the same, but for the IPv6 address.  I understand that in the future the IPv6 addresses are slowly going to replace IPv4 because the [v4 address space is exhausted](https://en.wikipedia.org/wiki/IPv4_address_exhaustion).  I am under the impression that the * acts as a wildcard so justaboutanything.pjcollins.org will point to the same IP address.  The @ represents the bare address pjcollins.org.