This is done by changing authentication aliases under deployment environment and watching the exceptions in the logs:

ProcessServerUser 
* is used by BPM component TeamWorks application to access BPM.ProcessServer.Bus
* is used by MDB in 'social-bus-ejb.jar' module in BPM component 'IBM_BPM_Process_Portal_Notification' as authentication in jms/PortalWebMessagingActivationSpec
activation specification for topic jms/PortalWebMessagingTopic
PerformanceDWUser is used to access BPM.ProcessServer.Bus

* EmbeddedECMTechnicalUser is used by BPM component IBM_BPM_DocumentStore application to access ConfigRepository MBean to get information from 'cells/PSCell1/resources.xml'

* SCAUser is used by SCA runtime to authenticate to in accessing bus BPM.ProcessServer.Bus in SCA activation specifications.  
Those activation specification can be found under 'Resource Adapters -> J2C activation specifications'.  The authentication alias
is not set in the activation specification but provided programatically by the runtime.







The SCA runtime does not use 'SIB JMS Resource Adapter' but 'Platform Messaging Component SPI Resource Adapter' to connect to SIBus.  During the deployment of SCA Modules, 
no Connection Factories are created explicitly on the SIB SPI Resource Adapter, but instead the SCA runtime creates Connection Factories programmatically using 
the low level SIB Core API. Therefore the properties of these Connection Factories, such as the Connection Pool settings, are not visible and can't be changed 
by an administrator for tuning purposes.  Activation specifications are created under 'Resource Adapters -> J2C activation specifications'
http://www.ibm.com/developerworks/websphere/library/techarticles/0809_faulhaber/0809_faulhaber.html
* by default this resource adapter uses 'Default' thread pool.  To change it go to 'Resource Adapters -> Platform Messaging Component SPI Resource Adapter'
The resource adapter rar file : sib.ra.rar


All buses in previous versions (BPC Bus, CEI  Bus, SCA.SYSTEM bus, SCA.APPLICATION bus) are now converged
in one bus  BPM.ProcessServer.Bus


IBM_BPM_DocumentStore contain embedded in resource adapter in the ear (module name: FileNet P8 Connector, rar file: engine.rar)
The connection factory and pool settings are done from this module and not from under 'Resource-> Resource Adapters'


Note: In WAS admin console: 'WebSphere enterprise applications -> enterprise app name -> manage modules' shows the modules by their
name rather than by their 'war/jar/rar' module file name.  To know the module file name:
	* click 'Display module build Ids'
	* by clicking any of the modules it show the actual module file name in the bread crumb bar at the top


