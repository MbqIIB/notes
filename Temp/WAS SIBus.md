?? Look into 'Updating enterprise application files'.  What requires server or application restart and what does not
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/trun_app_upgrade.html

SIB Service: can be enabled for a server to act as SIB endpoint provider bootstrap server or to access messaging engine locally hosted.  When a messaging is created on WAS server for the first time, SIB service is automatically enabled.  When SIB service is enabled or disabled, server has to be restarted for the change to take effect.

SIB service uses two ports and each port has its own transport chain:
1- SIB_ENDPOINT_ADDRESS: unsecured
	* default port 7276
	* transport chain: InboundBasicMessaging
2- SIB_ENDPOINT_SECURE_ADDRESS: secured
	* default port 7286
	* transport chain: InboundSecureMessaging 
The SIB transport chains can be accessed from two locations in the server page:
	1- Server Messaging -> Messaging engine inbound transports
	2- Ports -> 'View associated transports' in transport details next to the port type
	

When a server is a member of SIBus, clicking on 'Server Messaging -> Messaging engines' on the server page, shows the messaging engine.
However, if the server is a member of a cluster and the cluster is a member of the SIBus, then no messaging engine will be show on the server page if the server is hosting a messaging engine for that bus.  Instead the messaging engine show in the cluster page under 'Cluster messaging -> Messaging engines'

The ports of SIB service will not opened until either:
	a) the server host a messaging engine
	b) the server is in the 'Bootstrap members' of a bus.  Can be added under 'Bus -> Topology -> Bootstrap members'.  By default, the selection is 'All members of the cell with the SIB Service enabled' but can changed to different criteria.
	
The SIB service in a WAS server with its transport channels can serve multiple messaging engines of different buses.  The bus service can answer bootstrap request or messaging engine access requests.  When a new JMS connection comes to the SIB service for a bus and the it can be served by a local messaging engine(depends on criteria), then the service will bootrap the client to the local messaging engine in the same request.  Otherwise, it will bootstrap the client to a different server by redirect the client to a messaging engine in that server.  In this case, the client will be making two connections: one to the bootstrap SIB service and another one to messaging SIB service.

put the knowledgecenter information here:

??endpoint provider: unsecure the username and password can be seen.  ??the relation between 'endpoint provider' and 'Target inbound transport chain' 

* why bootstrap server is needed for SIBus clients? For two reasons:
	a- Location transperancy: The messaging engine could be not in a fixed location (in case of fail-over).  Or there could be multiple messaging engine and the decision of the best messaging engine is left as a server decision rather than a client decision.
	b- load-balancing: the bootstrap server will load-balance connections to multiple messaging engines (in cluster).

Scenarios:
==========
A standalone client is trying to connect to a bus in a was cell.  The bootstrap server configured in JMS connection factory is also a member of bus and is hosting a messaging engine.  Let us assume that the bootstrap server is always going to connect the client to the local messaging engine hosted.  The cases where the connection factory has the following configuration:
1- 
	* provider endpoint: BootstrapSecureMessaging
	* Target inbound transport chain : InboundBasicMessaging
	Then, the client will make one connection to SIB service through the unsecured 'SIB_ENDPOINT_ADDRESS' and this same connection will be connected to the messaging engine. (In wireshark only one connection is seen)
	
2-
	* provider endpoint: BootstrapSecureMessaging
	* Target inbound transport chain : InboundSecureMessaging
	Then, the client will make one connection to SIB service through the unsecured 'SIB_ENDPOINT_ADDRESS' and will be redirected to create another secure connection through port 'SIB_ENDPOINT_SECURE_ADDRESS' to the same SIB service to connect to the messaging engine.  (In wireshark, two connections can be seen)

??check the relation between bus security ' transport level security' configuration when creating a new bus and the impact of this on the selection of Target inbound transport chain : InboundBasicMessaging or InboundSecureMessaging.  Will connection succeed if JMS Connection factory Target inbound transport chain is 'InboundBasicMessaging' and the bus has secure transport configured?
	
