### Transport Channels and Transport Chains
* Transport Channel: a communication layer in a communication stak
* Transport Chain: A communication stack composed of layered communication channels
* Transport chains are configured for ports (inbound connections).
* Multiple transport channels can be configured for the same port which can share the lower transport channels but have their own upper transport channels. These upper transport channels have `discrimination weight` that determines the priotiy by which chains get to access incoming data. The channel in the chain with the lowest `discrimination weight` is the first one given the opportunity to look at incoming data and determine whether or not it owns that data
* Transport chains can be configured for the following types of ports.  These can be configured in admin console gui:
	* WebContainer HTTP ports: by default WAS defines two transport chains per HTTP port
	* SIBus JFAP ports (for communication between SIBus messaging engines in the same bus or across buses.  Also, for communication between JMS clients and messaing engines)
	* SIBus MQFAP ports (for communication between messaging engine and Websphere MQ)
	* HAManager/DCS core group group commuinication ports
* Transport are also configured for certain types of outbound connections.  These can only be configured through wsadmin. [Reference: Outbound transport options](http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.multiplatform.doc/ae/cjk2000_.html?lang=en):
	* SIBus JFAP chains: this chain is configured in:
		* SIBus JMS Connection Factory `bootstrap provider endpoint`: The bootstrap transport chain specified here is used by JMS client to connect to SIBus messaging engine.
		* SIBus Link `boostrap provider endpoint`.  Used by SIBus to connect to remote SIBus messaging engine.
		* `Inter-engine transport chain` configuration of SIBus:  messaging engines in belonging to the same SIBus used this transport chain to connect to each other.
	* SIBus MQFAP chains: this chain is configured in:
		* `Transport Chain` in WebSphere MQ link sender channel configuration panel
* The above types of chains can be configured with different sets of tranport channels
* WAS by default comes with preconfigured transport chains
* More transport chains can be configured with different transport channels.
* The definitions and configurations of transport channels and chains are stored in `server.xml`