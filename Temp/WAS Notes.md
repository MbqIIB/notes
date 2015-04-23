WebSphere Application Server:

* Authentication aliases:
It is created in "security.xml" under the cell.  Authentication aliases have no scope.  If an authentication alias is changed, you need to restart the servers after
sync for the new values to be loaded.

* Scheduler in server cluster:
Server1
[1/12/15 18:55:46:014 AST] 00000004 LeaseAlarm    I   SCHD0133I: Scheduler WSRRScheduler (sched/WSRRScheduler) has acquired the lease and will run all tasks on this application server.

Server2
[1/12/15 18:55:46:029 AST] 00000005 LeaseAlarm    I   SCHD0134I: Scheduler WSRRScheduler (sched/WSRRScheduler) has lost its lease and will no longer run tasks on this application server.  Tasks will now run on server <null>.

* When federating a node to deployment manager or synchrnizing, the machine clocks have to be in sync with little time difference.  Otherwise, the sync fails.
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/tsec_esecarun.html?cp=SSAW57_8.5.5%2F1-8-2-33-0-2-1


*  Logs:
  * manageprofiles.sh : WAS_HOME/logs/manageprofiles/xxx.log
  * addNode.sh : PROFILE_HOME/logs/addNode.sh
  
  
* When creating a new messaging engine in a WAS server, the server has to be restarted to be able to start the messaging engine.
* Installation and updates of enterprise applications happen in deployment manager.  It could happen that deployment manager does verification of the application being installed and expects certain dependencies to be available (e.g. in a shared library) and fails the installation because of it (classdefnotfound errors).  This is noticed in WAS SCA BLA applications. This happen because shared library is not available for deployment manager.  In this case, as a workaround, the libraries have to be copied to <WAS_HOME>/lib/ext folder of deployment managers.

* Changing setting in existing data source or jms resources requires a server restart for the changes to take effect.
http://stackoverflow.com/questions/4562561/websphere-application-server-v7-configurations-without-restart
* Transaction service configuration changes require server restart for changes to take effect except for this set at 'Runtime' tab.
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/tjta_settlog.html?cp=SSAW57_8.5.5%2F1-3-9-6-2-0&lang=en


* WAS thin client does do dependency injection.  WAS Java EE client container does dependency inject at client side.

??Look into Hot deployment and dynamic reloading
http://www.ibm.com/developerworks/websphere/techjournal/0510_apte/0510_apte.html
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/trun_app_upgrade.html


JMS Sibus connection:
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/tjn0033_.html?lang=en
If provider endpoint is not set, then default value is assumed:
* createConnection without username and password: 
* createConnection with username and password: localhost:7286:BootstrapSecureMessaging

The client is going to contact localhost on port 7286 and going to get 'java.net.ConnectException: Connection refused'

??Sibus service: bootstrap server.  configuration reload enabled
??messaging engine inbound transports

http://www.hermesjms.com


===================


WebSphere Application Server:

* Authentication aliases:
It is created in "security.xml" under the cell.  Authentication aliases have no scope.  If an authentication alias is changed, you need to restart the servers after
sync for the new values to be loaded.

http://www-01.ibm.com/support/docview.wss?uid=swg21680088

* Scheduler in server cluster:
Server1
[1/12/15 18:55:46:014 AST] 00000004 LeaseAlarm    I   SCHD0133I: Scheduler WSRRScheduler (sched/WSRRScheduler) has acquired the lease and will run all tasks on this application server.

Server2
[1/12/15 18:55:46:029 AST] 00000005 LeaseAlarm    I   SCHD0134I: Scheduler WSRRScheduler (sched/WSRRScheduler) has lost its lease and will no longer run tasks on this application server.  Tasks will now run on server <null>.

* When federating a node to deployment manager or synchrnizing, the machine clocks have to be in sync with little time difference.  Otherwise, the sync fails.
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/tsec_esecarun.html?cp=SSAW57_8.5.5%2F1-8-2-33-0-2-1


*  Logs:
  * manageprofiles.sh : WAS_HOME/logs/manageprofiles/xxx.log
  * addNode.sh : PROFILE_HOME/logs/addNode.sh
  
  
* When creating a new messaging engine in a WAS server, then it is most likely the 'SIB service' was not previously enabled and thus the server will need to be restarted.
The SIB service is enabled automatically when a new messaging engine is created.
* Installation and updates of enterprise applications happen in deployment manager.  It could happen that deployment manager does verification of the application being installed and expects certain dependencies to be available (e.g. in a shared library) and fails the installation because of it (classdefnotfound errors).  This is noticed in WAS SCA BLA applications. This happen because shared library is not available for deployment manager.  In this case, as a workaround, the libraries have to be copied to <WAS_HOME>/lib/ext folder of deployment managers.

* Changing setting in existing data source or jms resources requires a server restart for the changes to take effect.
http://stackoverflow.com/questions/4562561/websphere-application-server-v7-configurations-without-restart
* Transaction service configuration changes require server restart for changes to take effect except for this set at 'Runtime' tab.
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/tjta_settlog.html?cp=SSAW57_8.5.5%2F1-3-9-6-2-0&lang=en


* WAS thin client does do dependency injection.  WAS Java EE client container does dependency inject at client side.

??Look into Hot deployment and dynamic reloading
http://www.ibm.com/developerworks/websphere/techjournal/0510_apte/0510_apte.html
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/trun_app_upgrade.html


JMS Sibus connection:
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/tjn0033_.html?lang=en
If provider endpoint is not set, then default value is assumed:
* createConnection without username and password: 
* createConnection with username and password: localhost:7286:BootstrapSecureMessaging

The client is going to contact localhost on port 7286 and going to get 'java.net.ConnectException: Connection refused'

??Sibus service: bootstrap server.  configuration reload enabled
??messaging engine inbound transports

* SIBus dynamic reloading of configuration files:
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/cji0004_.html?lang=en

http://www.hermesjms.com


