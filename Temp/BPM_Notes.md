Process Center Enterprise App:
C:\IBM\BPMAdv\v8.5\BPM\Lombardi\process-center\applications
Process Server (Lombardi) enterprise Apps:
C:\IBM\BPMAdv\v8.5\BPM\Lombardi\process-server\applications
Datawarehouse enteprise apps:
C:\IBM\BPMAdv\v8.5\BPM\Lombardi\performance-data-warehouse
Shared libraries:
C:\IBM\BPMAdv\v8.5\BPM\Lombardi\lib
* Note: to know which application name maps to which module name, click on the enterprise appliation -> Display module build Ids. 

bootstrapProcessServerData (system TWX files: toolkits, coaches, ...etc):
C:\IBM\BPMAdv\v8.5\BPM\Lombardi\imports

* bootstrapProcessServerData uses one of BPM security roles mappings to import changed system twx into database.  Those security roles are stored in cell-bpm.xml
When you run the bootstrapProcessServerData command, configuration data for the IBM® Business Process Manager applications is loaded into the Process database. This data is required for these applications to run correctly.
** Important: If you created an Advanced-only Process Server deployment environment (an environment without the capabilities included in Standard deployment environments), you can skip this task. There is no need to run the bootstrapProcessServerData command.
** If you created the database tables when you created the deployment environment, either by setting the parameter bpm.de.deferSchemaCreation to false for the BPMConfig command, or by enabling Create Tables in the Deployment Environment wizard, there is no need to run the bootstrapProcessServerData command.
** In a Standard deployment environment or an Advanced deployment environment, you must run this command after a server or cluster of servers is created. For a cluster, you need to specify the cluster name. Run this command before the first server is started. You do not need to rerun the command if you add another cluster member.
** If a single WebSphere cell contains multiple application target clusters, you must run this command on each of the clusters.


* <PROFILE_HOME>/config/cells/cell_name/cell-bpm.xml : contains deployment environments information.

* IBM BPM v8.5 Fixpack 1 (v8.5.0.1) has a problem that needs to be fixed after the fix is applied:
http://www-01.ibm.com/support/docview.wss?uid=swg21662071

* Prcess Designer:
process designer uses Windows IE proxy settings to connect to process center.  This has to have bypass proxy for the process center hostname
https://www.ibm.com/developerworks/community/blogs/aimsupport/entry/ibm_bpm_help_the_proxy_ate_my_process_designer?lang=en

* In BPM v8.5 more of Lombardi configuration files (99Database.xml 99Local.xml 100Custom.xml) are being made WAS configuration files (e.g. cell-bpm.xml)
* The BPM security role mappings are used by BPM process server (Lombardi) components and tools (bootstrapProcessServerData) to call services, access Sibus messaging engines, ...etc.  They are not used by BPC components.

?? Summarize DB configuration and deployment env property file notes.  Look at knowledge center BPM Collection

* Process Designer connects to 'jms/TWClientConnectionFactory' in process center.  This why this connection factory has to have endpoint provider defined
