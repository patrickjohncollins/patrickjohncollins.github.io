---
title: Business information systems
date: 2020-04-28
---

I have developed business information systems over the past twenty years in South Africa, Ireland and France, for companies operating in the fields of real estate, transportation, telecommunication, insurance, stock trading, debt recovery, and food production.  *WHAT DO THEY DO?*

A business information system is composed of a database, a set of business rules, numerous forms, a plethora of statistics, and quite likely one or more data exchanges with external systems.  The system is the purview of developers, database administrators, network administrators and data analysts.  These technical specialists form the IT department.  The inner workings of the system is beyond the comprehension of the business people who drive the requirements.  The better organized companies track and prioritize features and anomalies.  The system evolves and changes over the years, new functionality is added, existing functionality falls into obsolescence.  Overly-complicated rules are translated into even more complicated code.  The business experiences turnover of personnel, both business people and IT specialists depart and are replaced.  The understanding of how or why the information system works is slowly lost.  The inner workings are now beyond the comprehension of most of the specialists as well.  

I often find myself in the role of an archeologist, entering a crumbling tomb armed with fragmentary documentation, trying to decifer the hieroglyphs, getting lost in secret passageways, stepping on booby traps.  Most are too afraid of the snakes and spiders, but I get in there with a flamethrower in one hand and chainsaw in the other.  If it breaks someone will scream.  Sometimes it will be months later and the damage will be done.  I advise those in charge of the state of affairs.

But the company marches on, encoding ever more business processes into the system, and the system begins to control the company instead of the other way around.  The underlying technological ecosystem also continues to evolve and the company fails to keep up.  The system, now quite obsolete, becomes a noose that slowly strangles the company.  Once the situation is too far gone the company resorts to an even more expensive rip-and-replace in a bid to free themselves.  And so the cycle repeats.

This may sound excessive, but I have seen it happen many times.  


The information systems that I have worked on have all been developed using Microsoft technology, the Windows operating system, the SQL Server relational database, the IIS web server, the .NET Framework and the C# programming language.  Generally, users access the system through their web browsers, thus side-stepping the difficulty of managing client deployment and updates.  Client-side logic is delivered on-demand in the form of Javascript code.  With the advent of tablets and smartphones these user interfaces have been adapted to smaller screens and touch input.  The hosting of the system has been slowly shifting from in-house to the cloud, freeing companies from the need to maintain their own servers and benefiting from higher levels of availability than they could easily acheive in their own data centers.


For much of my work these tools feel too low-level.

HTML `<select>` element.  Loading the contents by calling a REST API on the server.  Coding that API.  Making a database query.


A real estate agent wants to list a property for sale.  

A property may not be listed for sale without the energetic performance.  This isn't just a business requirement, it is a legal requirement.




Software has many applications.  Its used to decode the human genome, to control satellites and probes in outside, to video edit the next Hollywood blockbuster, to provide the immersive experience of a hit video game.  But these are niche applications.  Most software professionals are employed in the design and maintenance of business information systems.







Software is everywhere, in science, engineering, art.  The digital platform that analyse human genes, the system embedded in a satellite detecting real-time changes in the Earth's surface, the video editing suite used to make the next Hollywood blockbuster, the sound studio used to composing the next hit in electronic track, the next amazingly immerse video game.  But these are just niches.  The primary use of software is to run business.  It's not as sexy, but that's where the most work is, for the vast majority of software professionals are employed in the design and maintenance of business information systems.



This might sound extreme, but I have seen this many times in my career.  Short-term productivity gains are not carefully weighed against long-term costs.  Many companies embark on overly-ambitious projects.  The system needs to be kept simple, maintainable, understandable.  When the system is too far gone companies resort to even more expensive rip-and-replace projects in a bid to free themselves from the noose.

Of course it's not all doom and gloom.  By embracing software, companies can increase their comptetive advantage in the marketplace through gains in efficiency.







Most of the artifacts of the system are actually technology-independant.  The data schema, the rules, the forms, the queries and more, could theoretically be decoupled from the subsystem.  It seems to me that we spend too much time operating at too low a level.  



There is a lot of talk about micro-services, of dividing the system into independent pieces, which sounds good in theory, but in practice software systems expand the same way cities do: one house at a time, one street at a time, eating up the surrounding landscape.


A new sub-system is needed, specifically tailored for the needs of business and information systems.


Identifiers
Mesures
Text

Storing chunks of JSON or XML in text fields is insanity.

Archiving
User identification
Change-tracking
Visibility permissions
Modification permissions
Queries
Calculated measures
Filters


Reliablility
Geographic redundancy
Encryption






Identifiers

Your insurance policy has a number.  It could be a reference.

Fully qualified identifiers.

Identifiers are not measures.


Text and numbers.

Mesurements.

Thankfully, most of the world has adopted the very rational International System of Units, but the USA still retains customary units.

When defining a mesure, the system, unit and precision should be specified.  By example "distance": SI km.  We are storing kilometers, not meters.


Let's take dates as an example.  We generally express dates as a year, month and day.  By convention, the day actually starts at different times across the globe.  When I store a date, lets say "28 april 2020" in the information system, what is actually stored might in fact be "28 april 2020 00:00:00 GMT".  Then when the system later retreives the date the system may automatically translate the time on the server to the time zone of the client and the date may actually show up on the screen as "27 april 2020".  I cite this example, because it has happened to me more than once, and it is infuriating.  If I need the precision of time, then I will specify it, often enough I don't and I would prefer that the system didn't take those kinds of decisions for me.  The underlying reason for this is that most dates are in fact stored as a number of seconds since a specific epoch, commonly 00:00:00 UTC on 1 January 1970.  Based on this implementation it is impossible to store the date without the precision of the time.  A better implementation would allow me to simply store the number of days.  Perhaps this might seem unnecessary.  Let's take a concrete example: date of birth.  This is a value we record over and over again.  I happen to know that I was born at approximately 3:30 GMT, but that information is never asked for.  Indeed, most people probably don't even know what their time of birth was.  Either way, recording their time of birth as midnight GMT is in fact incorrect.  The time portion is simply unknown.  The Gregorian calendar is used by probably almost all business in the world, and the epoch is based on the years since the supposed incarnation of Jesus.  

The epoch is arbitrary.
Years and days are very real.
Months are arbitrary.
Q1,Q2,Q3,Q4 are very commonly used in reporting.



Money values.  Currency.  

SI

