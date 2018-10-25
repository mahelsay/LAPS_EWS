Hello...! So in this post I’m going to show one of the many ways to successfully implement Microsoft LAPS in your environment.
This post is written for windows security professionals who are familiar with what Microsoft LAPS is.

In a nutshell it’s a Microsoft option that can be used to manage local administrator password across domain environment. More details are found here

Firstly let me start with the use case from security perspective or the challenge that face big organization when discussing how LAPS could be implemented.

Scenario:
Suppose for operational reasons or for nature of your organization’s business that local administrator password is shared across some desktop technicians
Now suppose you are a windows security professional and decided to implement Microsoft LPAS to control local administrator password on all end point windows machines.
Suppose your organization has big number of desktop machines and spread to multiple physical locations.
The challenge:
How would the help desk technician get local administrator password once control be only with LAPS? Options are:
a)      May be it is decided that they get access to the small tool called “LAPS UI” to get any password within their authorization ?
b)     May be it is decided that the technician has to call service desk to provide him with the requested machine’s password?

HOWEVER, given nature of many organization those two options might add extra load or might be seen as inefficient!!
So what other approach could be implemented to make it easy get the local administrator password in fast and easy way ??

Here we go:
By simply utilizing Exchange EWS we can build following logic (of course once idea is clear you can customize the logic by adding extras based on your information security needs)
1-     A technician urgently needs to login to one desktop machine with local administrator
2-     The technician will send just one simple email to a given mailbox (for example: laps@domain.com ) and put machine name in subject field
3-     Once that email lands to the laps mailbox (laps@domain.com) the below script that utilizes EWS will simply take the email subject as a variable and query active directory to retrieve administrator password for that machine then reply back to same “sender” with requested password immediately.

This makes it very easy to get any password (of course with restrictions in mind by who can request) at any time without the need to call service desk or call admins and in same time keeping flexible logging capabilities for tracking purposes like to know who requested for which machine at which time.
Following script can be used and run in many ways including a as a scheduled task
Pre-requisites to run successfully run the script:
1-     download and install the Exchange Web Services Managed API. from here
2-     Download laps and install laps management tools. from here
3-     Create dedicated mailbox to receive requests from technicians (in this script mailbox created called “LAPS”)
4-     Create a folder in that mailbox called “processed” where all processed requested emails will be moved to.

first we will import module and set reference objects like credentials and ewsservice as well as EWS URL to easily reference it later# LAPS_EWS
how to use EWS to serve Microsoft LAPS purpose ?


then after creating ews service object we start binding to LAPS mailbox and in particular to the inbox


then we create needed folder view using search criteria for folder "processed"



finally for each email message received it will do following series of actions:
- will retrieve local administrator password from active directory
- will send email back to same sender with password requested
- will move processed message to folder "processed"


you can feel free to play around the script to add whatever rules required specially for tracking...

this script was tested on servers 2016 and servers 2012 R2 when running as scheduled task

please feel free to leave a comment and contact me at mahelsay@gmail.com

