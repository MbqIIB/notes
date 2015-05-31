* make sure that the hostname in the config file is added to hosts file or DNS and test it
* BPMConfig.sh -create -de aaa.properties
* run the DB scripts
* start deployment manager
* Fix JDBC and datasource issues and point to the Oracle thin client jar
* in deployment manager profile bin directory: bootstrapProcessServerData.sh -clusterName cluster_name
* in deployment environment page, click 'configuration done' in deffered configuration
* start node of all nodes
* start deployment environment and make sure there are no errors in logs
* in deployment environment, click on health center and make that all status are ok.


=================

Installation Manager:
* ./userinst -dataLocation /opt/IBM/IMData
* directory: /opt/IBM/InstallationManager/eclipse
* Preferences -> Files for Rollback : uncheck "save files for rollback"

BPM Installatin:
in validate prerequisite check that you are not getting any validation warnings like:
"
Current system has detected a lower level of ulimit open files setting than the recommended value of 8192. Increase the ulimit open files value to minimum value of 8192 and re-start the installation."

In BPM make sure to select all ifixes

In features to install: select process server production license

cell name: FT_PServerCell
bpm.de.name = PServer_DE
bpm.de.psServerName= FT-ProcessServer

Prerequisistes:
===============
* The Test environment should be one server.  Both DMGR and Custome profiles will be created in that server.  Otherwise, you need to run the command twice in each
* make sure the hostname is added in hosts file or DNS before starting


* Edit configuration file and set:
** DE Admin: psdeadmin/psdeadmin
** Cell Admin
** deployment manager and node hostnames and port are correct.
** database hostname and name
** set bpm.de.psPurpose if it is development, test, staging or production 

