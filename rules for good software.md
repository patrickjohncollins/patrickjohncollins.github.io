Title: Rules for good software
Date: 2010-02-23

All software is doomed to die one day, but good software lives a lot longer than bad software.  
Globalization is not something that can be retrofitted; it has to be designed for from the start.  
That said, designing for globalization from the start is easy; just create a resource file and reference all your strings from that file.  
Designing for globalization will also have an effect upon your user interface design, for labels must be wide enough to accommodate longer translations.  
Be wary of date and number formats; rely on system-provided parsing functions, never roll your own.  
Always work in UTC. 
Format times for display in the local time zone.  
Always store the currency alongside an amount field.  
Use referential integrity aggressively in your database.  
Every table should have a primary key, and that key should be an auto-incrementing integer.  
Alphanumeric codes are often easier for users to remember, but should not be used as primary keys.  
All lookups should have their own tables, never be tempted to put them all in a single table.  
On a system that handles multiple independent clients, give each client a copy of the database, don't try and squeeze them into the same database.  
Log all errors to a text file.  
Outsource user identification; use a pre-existing component; never roll it yourself.  
Try and break the system into modules.  
Each module should have a version number.  
Use semantic versioning.  
Modules communicate with each other using a protocol.  
The protocol should have a version number too.  
The first message sent between modules should contain the version numbers to establish mutual compatibility.  
Modules should know if they are incompatible, and should instantly raise the alarm.  
Always fight to maintain backward compatibility.
Keep a document updated with the list of all modules, and all the versions that have been released into the wild.  
This document should also contain a diagram of module dependency.  
Deciding the scope of modules is hard.  
Ensure that each module can be tested in isolation.  
Tests should be repeatable.  
Wherever possible, functions should be deterministic. Don't create a function that modifies the system state and then returns a result. Create two functions, one that modifies the state, one that returns a result, and then call each in turn.  
Prune aggressively. Any unneeded code should be summarily deleted. Your source control system has an archive if you need to get it back.  
Users will report bugs. Not all bugs reported by users are in fact bugs. A bug is when the system does something it was not designed to do, or if the system does not do something that it was designed to do. There is an element of subjectivity in the previous statement.  
Fix bugs aggressively. They will pile up and crush you in the long term.  
Creating a simple user interface is hard.  