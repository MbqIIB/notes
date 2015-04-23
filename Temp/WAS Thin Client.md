WAS Thin Client:

* com.ibm.ws.ejb.thinclient_8.5.0.jar: contains jndi.properties orb.properties

* WAS tools in %WAS_PROFILE%/bin (e.g. dumpNameSpace.bat) that communicate over IIOP uses %WAS_PROFILE%/properties/sas.client.props
The shell script for all of these tools calls 'setupCmdLine' which sets these JVM properties where %USER_INSTALL_ROOT% points to the profile directory:
SET CLIENTSAS=-Dcom.ibm.CORBA.ConfigURL=file:%USER_INSTALL_ROOT%/properties/sas.client.props
SET CLIENTSOAP=-Dcom.ibm.SOAP.ConfigURL=file:%USER_INSTALL_ROOT%/properties/soap.client.props
SET CLIENTIPC=-Dcom.ibm.IPC.ConfigURL=file:%USER_INSTALL_ROOT%/properties/ipc.client.props
SET CLIENTSSL=-Dcom.ibm.SSL.ConfigURL=file:%USER_INSTALL_ROOT%/properties/ssl.client.props
SET JAASSOAP=-Djava.security.auth.login.config=%USER_INSTALL_ROOT%/properties/wsjaas_client.conf

Note about in file sas.client.props: section 'Authentication Configuration' lists the properties and their values in the comments.  The property 'source' is wrong and the correct property name is 'loginSource'.

The thin client does load-balancing (using JNDI access) in a cluster environment.  the information of the servers in the cluster is returned to the client in Service Context in IIOP reply message if the cluster changes (servers added or removed).

*
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/tcli_developthin.html?lang=en
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/rnam_example_prop1.html?lang=en

https://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/cnam_mapping_considerations.html?lang=en


* createEJBStubs.bat c:/temp/MyEAR.ear
