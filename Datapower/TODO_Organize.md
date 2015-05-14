#### Datapower Performance Monitoring and Tuning
* [WebSphere DataPower SOA Appliance performance tuning](http://www.ibm.com/developerworks/library/ws-dpperformance/)
* [Monitoring WebSphere DataPower SOA Appliances](http://www.ibm.com/developerworks/websphere/library/techarticles/1003_rasmussen/1003_rasmussen.html)
* [Resource management and analysis best practices for WebSphere DataPower](http://www.ibm.com/developerworks/websphere/library/techarticles/1307_rasmussen/1307_rasmussen.html)
* [Datapower Throttle settings](http://stackoverflow.com/questions/27826161/what-is-the-throttle-settings-used-for-in-datapower-appliance)
* [Troubleshooting DataPower Appliances Technical Exchange](http://www-01.ibm.com/support/docview.wss?uid=swg27035675&aid=1)

#### Datapower error report, failure notification, internal state, FFDC:
* Access the FFDC options by enabling the 'Upload Error Report' option:
http://www-01.ibm.com/support/docview.wss?uid=swg21445832
I a nutshell, failure notification settings controls what is captured as part of error report and where to send/upload the error report.
What triggers an error report: 
1- When an error condition happens in datapower: memory throttling restart, ...etc these are configured in the failure notification
2- Configured FFDC event
* [Troubleshooting DataPower Appliances Technical Exchange](http://www-01.ibm.com/support/docview.wss?uid=swg27035675&aid=1)
* [FFDC](http://www-01.ibm.com/support/docview.wss?uid=swg21445832)
* [Failure Notification and FFDC](https://books.google.com.sa/books?id=zKnEAgAAQBAJ&pg=PA88&lpg=PA88&dq=datapower+%22FFDC%22+%22error+report%22&source=bl&ots=lNX22QPhSq&sig=rppsyxC0QPk5RiBj804TT6CHwNY&hl=en&sa=X&ei=RgBHVZLHMs38aJ3xgLAM&ved=0CFQQ6AEwCQ#v=onepage&q=datapower%20%22FFDC%22%20%22error%20report%22&f=false)
"Building on failure notification, FFDC can allow the appliance to capture information related to dignostics during runtime based on certain FFDC events"
* [Failure Notification configuration](https://www-01.ibm.com/support/knowledgecenter/SS9H2Y_6.0.1/com.ibm.dp.xi.doc/failurenotification.html?lang=en)
* ftp://ftp.software.ibm.com/software/iea/content/com.ibm.iea.wdatapower/wdatapower/1.0/xa35/381DataPowerRASEnhancements.pdf

#### Datapower internal state
* http://www-01.ibm.com/support/docview.wss?uid=swg21587451

#### Datapower XMLNames
* http://www-01.ibm.com/support/docview.wss?uid=swg21409683

####
* http://www-01.ibm.com/support/docview.wss?uid=swg21496334
* http://www-01.ibm.com/support/docview.wss?uid=swg21674507


#### Datapower and WSRR Integration
* [Enforcing Service Level Agreements using WebSphere DataPower, Part 1: Applying the SLA Control File pattern](http://www.ibm.com/developerworks/websphere/library/techarticles/1204_dearmas/1204_dearmas.html)
* [Enforcing Service Level Agreements using WebSphere DataPower, Part 2: Integrating with a WSRR service model pattern](http://www.ibm.com/developerworks/websphere/library/techarticles/1308_dearmas2/1308_dearmas2.html)
* [Enforcing Service Level Agreements using WebSphere DataPower, Part 3: Testing the integration with WSRR](http://www.ibm.com/developerworks/websphere/library/techarticles/1308_dearmas3/1308_dearmas3.html)


#### Datapower filesystem
* The 'Action' menu changes depending on the folder and file.
* It seems that you can only create and delete folder from file management on the 'local' folder only.  Verify?
* However, in different forms of Web-GUI (for creating objects) where destination folder is mandatory, you can specify a folder that does not exist in 'temporary' and it will be created as well as the object inside it.  For example: secure backup destination.
* As a destination URL not all folders can be used as valid URLs.  So far, only 'local:///' and 'temporary:///' works. Verify? 
* http://www-01.ibm.com/support/docview.wss?uid=swg21496334
* http://www-01.ibm.com/support/docview.wss?uid=swg21250655
* 'image' folder is mounted under 'temporary/image'.
'export' folder is mounted under 'temporary/export'
'logtemp' folder is mounted under 'temporary/log'
All content under the above folders is deleted after reboot because all files under 'temporary' folder are deleted


http://www-01.ibm.com/support/docview.wss?uid=swg21244384
http://www-01.ibm.com/support/docview.wss?uid=swg21257115


http://www.ibm.com/developerworks/websphere/library/techarticles/1009_furbee/1009_furbee.html
http://www.ibm.com/developerworks/websphere/library/techarticles/1006_majikes/1006_majikes.html

ftp://ftp.software.ibm.com/software/iea/content/com.ibm.iea.wdatapower/wdatapower/1.0/xa35/381DisasterRecoveryMode.pdf

http://www-01.ibm.com/support/docview.wss?uid=swg21508393
http://www-01.ibm.com/support/docview.wss?uid=swg21239595
http://www-01.ibm.com/support/docview.wss?uid=swg24014405
http://www-01.ibm.com/support/docview.wss?uid=swg27015333
http://www-01.ibm.com/support/docview.wss?uid=swg21244384

* If datapower appliance is Quiesced, all domains op-state will be down except 'default' domain
which is going to remain up. However, the 'Quiesce State' of the default domain will be 'Quiesced'.
Quiesce is a runtime state and is not persisted.  If device is restarted all domains will in up state again.
ftp://ftp.software.ibm.com/software/iea/content/com.ibm.iea.wdatapower/wdatapower/1.0/xi50/381DataPowerQuiesce.pdf



category mgmt will log the op state of the MQ as a DP object

category mq is what is happening with MQ

You can view log categories in Web-GUI: 'Admimnistration -> Miscellaneous -> Configure Log Categories'

Datapower HSM:
http://www-01.ibm.com/support/docview.wss?uid=swg21412060
https://books.google.com.sa/books?id=zKnEAgAAQBAJ&pg=PA61&lpg=PA61&dq=datapower+HSM&source=bl&ots=lNX3_TJdTo&sig=QohD0m5Ub6JUgjVIl9SwHut4SkA&hl=en&sa=X&ei=P4RQVabrEYG9UqX6gZgM&ved=0CCQQ6AEwATgK#v=onepage&q=datapower%20HSM&f=false


reinitialize: reflashes the firmware.
You can reflash any image at any version.
You should have the image file in 'image' folder. 
The command does not take a URL but just the image file name
after reinitialization, the admin account password is reset to 'admin'.  Upon first login, it prompts for new password.
http://www-01.ibm.com/support/docview.wss?uid=swg21244384


Initial installation wizard:
can be run with 'startup' command in the 'Global configuration mode'
Through the startup wizard you cannot set 'disaster recovery mode' or 'common criteria compability'
Both can only be set after firmware reinitialization by using 'reinitialize' to reflash the firmware


http://www-01.ibm.com/support/docview.wss?uid=swg21256195

* Datapower configuration is stored as cli commands in config files.  When datapower domain is reloaded or device rebooted, you will find that these commands run part of the configuration in "cli-log" to apply the persisted configuration into the running configuration.
* The actions that you make through Web-GUI are also run as cli commands.  You find the logs of these commands in "cli-log"


Audit log:
* audit log provides history of all administrative activities that occurs in the appliance.  So, only changes to objects that are considered administrative are logged.  example: create user, create domain, network services (ntp, dns, ssh, web-gui), throttle settings, xml management interface, ...etc
* Since all of these administrative objects are in the default domain, if you restart the default domain, you will find audit logs about the application of these administrative objects settings into the running configuration from the persisted configuration.
* When changes of running configuration are saved, you will find something similar to this audit log: 
Creating file "config:/temp_00274"
* The length of the audit log is restricted to approximately 256 KB with one rotation. The audit log (audit-log) and its rotation (audit-log-1) are stored in the "audit:" directory.
* You cannot access the "audit:" directory with the File Management utility. You cannot modify the entries that are written to the audit log.
* https://books.google.com.sa/books?id=6ZrEAgAAQBAJ&pg=PA25&lpg=PA25&dq=datapower+%22audit+log%22&source=bl&ots=Ond9yilAp9&sig=n1Cvb9mNek_YArXusfGdlBzv_II&hl=en&sa=X&ei=oXVUVa_3IcSBUZu-gRg&ved=0CEQQ6AEwBQ#v=onepage&q=datapower%20%22audit%20log%22&f=false
* Audit events can also be subscribed to in a log target.  The default log target of the 'default system log' subscrives to all event categories at 'error' level except 'mgmt' events which it subscribes to at 'notice' level.  Audit events are published at 'information' level and this why audit events do not appear in the default system log.  However, if you modify the subscriptions of the default log target to include 'audit' event category at 'information' or 'debug' level, you will see the same audit events that you find in the audit log.
