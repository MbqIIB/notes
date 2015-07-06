#### SIBus link
* [Resource: Using the service integration bus link in WebSphere Application Server to route messages from a local queue to a remote queue](http://www.ibm.com/developerworks/websphere/techjournal/1201_manickam/1201_manickam.html)

#### WAS v8.5 SIBus enhancements:
* [Technote: New Service Integration Technology Enhancements in WebSphere Application Server 8.5](http://www-01.ibm.com/support/docview.wss?uid=swg21621677)
* [Webcast Replay: New Enhancements to the SIBus in WebSphere Application Server V8.5](http://www-01.ibm.com/support/docview.wss?uid=swg27039490)

#### Configuration
* [The most commonly used Service Integration Bus custom properties in WebSphere Application Server](https://www.ibm.com/developerworks/community/blogs/aimsupport/entry/commonly_used_service_integration_bus_custom_properties_in_websphere_application_server?lang=en)
* [Configuring a messaging engine data store to use a data source](https://www-01.ibm.com/support/knowledgecenter/SSEQTP_8.5.5/com.ibm.websphere.base.doc/ae/tjm0045_.html?cp=SSEQTP_8.5.5%2F1-8-2-32-1-0-8-4-0)

#### Troubleshooting
* [Webcast replay: WebSphere Application Server - Service Integration Bus Messaging Engine Data Store Connectivity Problems and Solutions](http://www-01.ibm.com/support/docview.wss?uid=swg27020333)
* [Webcast replay: Tracing WebSphere's Service Integration Bus For Your Own Use](http://www-01.ibm.com/support/docview.wss?uid=swg27044129)
* [Tracing the Service Integration Bus](http://www-01.ibm.com/support/docview.wss?uid=swg21266767)
* [Webcast replay: Top 5 Service Intergration Bus Problems and Solutions](http://www-01.ibm.com/support/docview.wss?uid=swg27043741)

#### Resources
* [Knowledge Collection](http://www-01.ibm.com/support/docview.wss?uid=swg27038103)
* [Complex SIBus topologies and WAS7 Improvements](http://www.slideshare.net/kelapure/sibus-tuning-for-production-websphere-application-server)
* [Impact 2013 Configuring SIBus for Complex Toplogies Based on Customer Scenarios](https://www.ibm.com/developerworks/community/files/basic/anonymous/api/library/cfa136f0-30c1-4177-9901-62c05d900c5f/document/b4139b38-e1c2-4253-8e72-835d5409bde3/media)
* [What's new in Messaging for WAS v8.5 and Complex SIBus topologies](http://www.websphereusergroup.co.uk/wug/files/presentations/37/1152_Configuring_SIBus_for_Complex_Toplogies_Based_on_Customer_Scenarios.ppt.pdf)
* [Ask the Experts Replay: Service Integration Bus Scalability Best Practices](http://www-01.ibm.com/support/docview.wss?uid=swg27021025)
* [IBM Education Assistant WAS v8.5](https://www-01.ibm.com/support/knowledgecenter/websphere_iea/com.ibm.iea.was_v8/was/8.5/Architecture.html)
* [IBM Service Integration Bus Destination Handler, Version 1.1](http://www-01.ibm.com/support/docview.wss?uid=swg24021439)


#### Messaging Engine Normal Startup Sequence
``` 
Messaging Engine Normal Startup Sequence:
Here is a normal Messaging Engine startup sequence as it would appear in the SystemOut.log:
SibMessage I [:] CWSID0021I: Configuration reload is enabled for bus Bus1.
SibMessage I [Bus1:wasNode01.server1-Bus1] CWSID0016I: Messaging engine wasNode01.server1-Bus1 is in state Joined.
SibMessage I [Bus1:wasNode01.server1-Bus1] CWSID0016I: Messaging engine wasNode01.server1-Bus1 is in state Starting.
SibMessage I [Bus1:wasNode01.server1-Bus1] CWSIS1538I:The messaging engine, ME_UUID=FCC87D95F99CDE6E,
INC_UUID=B44E467DDE9EA5D9, is attempting to obtain an exclusive lock on the data store.
SibMessage I [Bus1:wasNode01.server1-Bus1] CWSIS1543I: No previous owner was found in the messaging engines data
store.
SibMessage I [Bus1:wasNode01.server1-Bus1] CWSIS1537I: The messaging engine, ME_UUID=FCC87D95F99CDE6E,
INC_UUID=B44E467DDE9EA5D9, has acquired an exclusive lock on the data store.
SibMessage I [Bus1:wasNode01.server1-Bus1] CWSIP0212I: messaging engine wasNode01.server1-Bus1 on bus
Bus1 is starting to reconcile the WCCM destination and link configuration.
SibMessage I [Bus1:wasNode01.server1-Bus1] CWSID0016I: Messaging engine wasNode01.server1-Bus1 is in state Started
```

#### SIBus Topologies:
* Design guidelines:
  * Message consumer should be connected directly to the ME which hosts the queue point that contain the message.  This it to avoid expensive `remote get`
  * The message producers and consumers patterns influence the possible topologies to produce correct behaviour:
      * One-way messages (Events) or Request/Reply message exchange
      * Should the reply message arrive at the same ME to which message producer is connected to or it can arrive at any ME where other instances of the producer application is connected to.
* Patterns:
    * Message concentrator ME (message gathering):
        * For producer: if the producer writes to multiple destinations that exist in two or more Buses, then rather than creating a connection factory for each Bus and committing to multiple connections, create a local bus with these buses defined as foreign busses and have remote destinations defined locally on that bus.  In this case, you need only one connection factory and commit to one connection.  Also, you isolate the application from having to know the underlying bus toplogy by having to deal with different connection factories and having to know which queue exist in which bus.
