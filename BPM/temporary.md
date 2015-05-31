1) make sure that the hostname in the config file is added to hosts file or DNS and test it
2) BPMConfig.sh -create -de aaa.properties
3) run the DB scripts
4) start deployment manager
5) Fix JDBC and datasource issues and point to the Oracle thin client jar
6) in deployment manager profile bin directory: bootstrapProcessServerData.sh -clusterName cluster_name
7) in deployment environment page, click 'configuration done' in deffered configuration
8) start node of all nodes
9) start deployment environment and make sure there are no errors in logs
10) in deployment environment, click on health center and make that all status are ok.


=================

Installation Manager:
1) ./userinst -dataLocation /opt/IBM/IMData
2) directory: /opt/IBM/InstallationManager/eclipse
3) Preferences -> Files for Rollback : uncheck "save files for rollback"

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
1) DE Admin: psdeadmin/psdeadmin
2) Cell Admin
4) deployment manager and node hostnames and port are correct.
3) database hostname and name
5) set bpm.de.psPurpose if it is development, test, staging or production 

