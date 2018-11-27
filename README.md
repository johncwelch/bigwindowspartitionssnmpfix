# bigwindowspartitionssnmpfix
This is a modified nagios storage plugin to deal with counter wrapping issues on big disk partitions when using SNMP

I tried to comment things out reasonably well. Basically, the issue we hit is that because Microsoft has "issues" with SNMP, they haven't updated their crap since 2000, mayyybe 2003. They want everyone using WinRM/WMI, which has a bad habit of crashing servers if you don't follow a complex series of steps exactly correctly.

Go MS.

Anyway, MS uses a signed 32-bit integer for disk space reporting. Right, you see the problem when you're talking about multi-TB partitions: suddenly things wrap and you have negative disk space. 

This hack to the snmp_storage.pl plugin attempts to fix that by applying the solution proposed here: https://support.solarwinds.com/Success_Center/Network_Performance_Monitor_(NPM)/Knowledgebase_Articles/SNMP_returns_a_negative_value_when_polling_huge_disk_space_on_Windows

in my testing, it seemed to work. Again, read the comments, it's not a lot of code i added, but it's not nothing either. It's hacky as hell, and yes, we're going to start testing WinRM/WMI. Because a heavy, HTTP-based protocol that increases data and server use by orders of magnitude solves SO MANY PROBLEMS with a TWO-DECADES-OLD SNMP IMPLEMENTATION THAT WOULDN'T EXIST IF MS WOULD JUST STAY UP TO DATE.

sigh. I wish I still drank.
