> These notes were taken or tested while working with version 8.5

#### Name shortcuts used in BPM
```
IBM Business Process Manager (BPM)
Lombardi Teamworks (TW)
WebSphere Lombardi Edition (WLE)
WebSphere Process Server (WPS)
BPM Database tables are prefixed with (LSW)
```

#### Concepts
* [Troubleshooting IBM Business Process Manager - *Dec 2013*](http://www.ibm.com/developerworks/bpm/bpmjournal/1312_chan/1312_chan.html)
* [Configuring error handling for Advanced Integration Services in IBM Business Process Manager Advanced V8 - *Oct 2012*](http://www.ibm.com/developerworks/bpm/library/techarticles/1210_agrawal/1210_agrawal.html)
* [Not sure if useful or outdated: Integrating WebSphere Process Server V7 and Lombardi Teamworks - *Jun 2010*](http://www.ibm.com/developerworks/websphere/library/techarticles/1006_fasbinder/1006_fasbinder.html)

#### [BPM Security Roles and authentication aliases](http://www-01.ibm.com/support/knowledgecenter/SSFTN5_8.5.5/com.ibm.wbpm.ref.doc/topics/managing_users_E.html):
> I did an experiment to change the authentication alias of all the security roles and then see the security exceptions thrown in System.out to figure out where the security role authentication aliases are used

* ProcessServerUser
  * is used by BPM component TeamWorks application to access BPM.ProcessServer.Bus `JMS createConnection`
  * is used by MDB in `social-bus-ejb.jar` module in BPM component `IBM_BPM_Process_Portal_Notification` as authentication in `jms/PortalWebMessagingActivationSpec` for accessing topic `jms/PortalWebMessagingTopic`
* PerformanceDWUser
  * used to access BPM.ProcessServer.Bus
* EmbeddedECMTechnicalUser
  * used by BPM component `IBM_BPM_DocumentStore` application to access `ConfigRepository` MBean to get information from `cells/PSCell1/resources.xml`
* SCAUser
  * is used by SCA runtime to authenticate to bus `BPM.ProcessServer.Bus` in SCA activation specifications.
  * Those activation specifications can be found under `Resource Adapters -> J2C activation specifications`. The authentication alias is not set in the activation specification but provided programatically by the runtime. The activation specifications are not JMS based but using `SIB JMS Resource Adapter`

#### BPM Configuration files:
* `config\cells\PSCell1\nodes\Node1\servers\server1\process-server\config`
* `config\cells\PSCell1\nodes\Node1\servers\server1\performance-data-warehouse\config`

#### BPMConfig tool
* create a deployment environment: `BPMConfig -create –de StandalonePS.properties`

#### Create Standalone Server
1. Edit file `$BPM_INSTALL/profileTemplates/BPM/BpmServer/actionRegistry.xml` and comment out `<validator path="../BpmDmgr/validators/productTypeValidator.ijc"/>`
2. Use BPMConfig file `8501StandalonePC.properties` to create standalone process center or `8501-StandalonePS.properties` to create standalone process server

#### Log files
* BPMConfig: `$INSTALL_ROOT/logs/config`

#### Default SCA modules installed
* BFMIF_*clusterName*
* HTMIF_*clusterName*
* HTM_PredefinedTaskMsg_*Vnnn_clusterName*
* HTM_PredefinedTasks_*Vnnn_clusterName*

#### Default Activation Specifications:
> The SCA runtime does not use `SIB JMS Resource Adapter` but `Platform Messaging Component SPI Resource Adapter` to connect to SIBus.  During the deployment of SCA Modules, no Connection Factories are created explicitly on the SIB SPI Resource Adapter, but instead the SCA runtime creates Connection Factories programmatically using the low level SIB Core API. Therefore the properties of these Connection Factories, such as the Connection Pool settings, are not visible and can't be changed by an administrator for tuning purposes.  Activation specifications are created under `Resource Adapters -> J2C activation specifications` . 
[Reference Link](http://www.ibm.com/developerworks/websphere/library/techarticles/0809_faulhaber/0809_faulhaber.html)
>
>By default this resource adapter uses `Default` thread pool.  To change it go to `Resource Adapters -> Platform Messaging Component SPI Resource Adapter` .  The resource adapter rar file : `sib.ra.rar`

> Note: in previous versions there used to be multiple buses created (BPC Bus, CEI  Bus, SCA.SYSTEM bus, SCA.APPLICATION bus).  In v8.5, I can see only one bus `BPM.ProcessServer.Bus` where all of the those buses are converged in.

* System SCA queues activation specifications for the four default SCA modules
  * sca/BFMIF_*clusterName*/ActivationSpec
  * sca/HTMIF_*clusterName*/ActivationSpec   
  * sca/HTM_PredefinedTaskMsg_*Vnnn_clusterName*/ActivationSpec
  * sca/HTM_PredefinedTasks_*Vnnn_clusterName*ActivationSpec
* ?? -Verify- used for integration between SCA and Lombardi:
  * sca/WLE_*clustername*/ActivationSpec

### IBM_BPM_DocumentStore application
* contain embedded resource adapter (module name: `FileNet P8 Connector`, rar file: `engine.rar`)
* The connection factory and pool settings are done from this module and not from under `Resource-> Resource Adapters`

> Side note: In WAS admin console: `WebSphere enterprise applications -> enterprise app name -> manage modules` shows the modules by their name rather than by their `war/jar/rar` module file name.  To know the module file name either:
* click `Display module build Ids`
* Or by clicking on any of the modules, it show the actual module file name in the bread crumb bar at the top

### [Business Process Choreographer
####[EAR file Components](https://www-01.ibm.com/support/knowledgecenter/SSFTN5_8.5.0/com.ibm.wbpm.bpc.doc/topics/t2demanual.html?cp=SSFTN5_8.5.0%2F1-3-14-3-0&lang=en):
* Business Flow Manager: BPEContainer_*clusterName*
* Human Task Manager: TaskContainer_*clusterName*
* Business Process Choreographer Explorer: BPCExplorer_*clusterName*: 
* HTM predefined task templates: 
  * HTM_PredefinedTasks_*Vnnn_clusterName*
    The task templates provided can be seen by going: `SCA modules -> HTM_PredefinedTasks_Vnnn_clusterName -> Human tasks` .
    Those can also be seen from BPCExplorer under `Task Templates`
    1. Approval_Request
    2. Question
    3. Review_Request
    4. Todo
  * HTM_PredefinedTaskMsg_*Vnnn_clusterName*: 
    Same as above but contains only one template
    1. Message

####??-Complete- Navigation:
* JMS based
  * eis/BPEInternalActivationSpec
* Workmanager based
