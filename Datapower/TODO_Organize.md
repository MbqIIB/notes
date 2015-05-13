#### Datapower Performance Monitoring and Tuning
* [WebSphere DataPower SOA Appliance performance tuning](http://www.ibm.com/developerworks/library/ws-dpperformance/)
* [Monitoring WebSphere DataPower SOA Appliances](http://www.ibm.com/developerworks/websphere/library/techarticles/1003_rasmussen/1003_rasmussen.html)
* [Resource management and analysis best practices for WebSphere DataPower](http://www.ibm.com/developerworks/websphere/library/techarticles/1307_rasmussen/1307_rasmussen.html)
* [Datapower Throttle settings](http://stackoverflow.com/questions/27826161/what-is-the-throttle-settings-used-for-in-datapower-appliance)
* [Troubleshooting DataPower Appliances Technical Exchange](http://www-01.ibm.com/support/docview.wss?uid=swg27035675&aid=1)

#### Datapower error report, failure notification, internal state, FFDC:
Q/How to enable FFDC? Answer: by enabling 'upload error report' settings
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
