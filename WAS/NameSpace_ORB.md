#### EJB Client Types:

* non-WLM clients
  * include all other corba clients that do not use WebSphere client code and logic embedded in WAS implementation of initial context.
  * For load-balancing between cluster server, the nodeagent plays an important role as it directs the EJB method call to the best available cluster server and provides basic load-balancing:
   * `InitialContext context = new InitialContext(env)`
     * The client sends LocateRequest to one of the cluster memebers bootstrap port and send IIOP request with object key `NameService`
     * The boostrap replies with IIOP forward and return an IOR to the context object that contains all nodesagent hostname and port (using an  IOR `TAG_ALTERNATE_IIOP_ADDRESS` tag for every address)
     * The client uses the returned IOR to send LocateRequest to one of the nodeagents and with object key taken from the IOR object key.
     * The nodeagent replies with IIOP forward and return an IOR to the context object has one of cluster members hostname and port
   * `context.lookup`
     * The client using the IOR that points to one of the cluster memebers connects to the cluster memeber and sends IIOP Request with operation `resolve`
     * The cluster memeber replies with the IOR of the object looked up.  The IOR contains all nodeagents addresses (using an IOR `TAG_ALTERNATE_IIOP_ADDRESS` tag for every address)
   * `EJB method call`
     *  The client sends IIOP request using the IOR to one of the nodeagents.  The nodeagent replies with IIOP forward and returns an IOR of the object in the best cluster memeber.
     *  The client connect to the cluster member and sends IIOP request and continues to use that member.
  * The nodeagent in this scenario maintains routing table and directs the client to the best server.  The client is unaware of the toplogy.  The component that handles this routing is called LSD `location service deamon`
  * The only load-balancing done is when the nodeagent selects and cluster server.  For multiple clients, the nodeagent will select differetn servers.  However, the clients will continue using thier servers and do not load-balance among other members.
  *  since the nodeagent is going to send object IORs residing in cluster memeber servers, the cluster members need to registr their object IORs with the nodeagent.  The nodeagent does not contain any binding to EJB objects in its namespace but only context objects bindings.
  * Reference:
```
The LSD is kind of like a route map that maintains information about specific application servers that are exposing EJB containers (and EJB homes). With the LSD approach, clients (Java, C++, or otherwise ) obtain an IOR to the LSD as opposed to the specific EJB that they're after. The LSD then provides an IOR to the best available EJB container clone for the client request. This sort of connection is deemed to be non-server-group-aware and, as such, the request isn't directly aware of load balancing and WebSphere-specific WLM policies active within the containers. Because there is limited ability for non-WLM-aware clients to automatically participate in load balancing, there are limitations in what is available in terms of failover for your application code, although it's still somewhat seamless!

book: Maximizing Performance and Scalability with IBM WebSphere
http://flylib.com/books/en/4.91.1.44/1/
https://books.google.com.sa/books?id=tr4_IoEMlo8C&pg=PA33&lpg=PA33&dq=Maximizing+Performance+and+Scalability+with+IBM+WebSphere&source=bl&ots=oZM57Ffamu&sig=5M6YeNnTgO0Bi7SnP4R7P_GUuEo&hl=en&sa=X&ei=v7uXVd3qMMKsU_TQjAg&ved=0CD8Q6AEwBw#v=onepage&q=LSD&f=false
```
   * Code snippet for calling WAS EJB using Oracle JVM (WAS security has to be disabled for it to work Oracle JVM ORB).  
```
Using Java Initial Context

// the lookup operation of CosNaming service takes NameComponents.  Each NameComponent has value and kind attributes.
// context.lookup takes a string representaion of NameCompoents where they are seperated by forward slash and the  value and king attributes of a NameCompoenent are seperated by dot.  This why if a context name contains a dot it has to be escaped.  Otherwise, that part after dot will be interpreted as a kind attribute.

Hashtable env = new Hashtable(); 
env.put(Context.INITIAL_CONTEXT_FACTORY,"com.sun.jndi.cosnaming.CNCtxFactory");
env.put(Context.PROVIDER_URL, "iiop://wasvr1:9811"); 
InitialContext context = new InitialContext(env);
context.lookup("cell/clusters/mycluster/ejb/MyEJBEAR/MyEJB\\.jar/MyService#com\\.myejb\\.view\\.MyServiceRemote");
```
```
Using java ORB API

Properties props = new Properties();
ORB orb = ORB.init(args, props);
org.omg.CORBA.Object objRef = orb.string_to_object("corbaloc::wasvr1:9812/NameServiceServerRoot");
NamingContext nc = NamingContextHelper.narrow(objRef);
NameComponent[] nameParams = {
	new NameComponent("cell", ""),
	new NameComponent("clusters", ""),
	new NameComponent("mycluster", ""),
 new NameComponent("ejb", ""),
 new NameComponent("MyEJBEAR", ""),
 new NameComponent("MyEJB.jar", ""),
 new NameComponent("MyService#com.myejb.view.MyServiceRemote", ""),
};
		
org.omg.CORBA.Object obj = nc.resolve(nameParams);
```

* WLM clients
 * Clients that use WAS context implementation of `com.ibm.websphere.naming.WsnInitialContextFactory` and IBM ORB.
 * The nodeagent takes part in the initial requset but after that does not participate.
 * during bootstrap, the `getProperties` of `WsnNameService` returns a list of IORs to context objects in nodeagent that are links to real cluster member root context objects. Those IORs has the addresses of all nodeagents of the cluster (using an  IOR `TAG_ALTERNATE_IIOP_ADDRESS` tag for every address).  Once this context object in nodeagent is accessed, the nodeagent will forward to the actual context object in the cluster member.  The following is a snippet from namespace of a nodeagent showing the context object links:
```
Node agent of a cluster with two memebers:

   5 (top)/clusters/mycluster
    5    Linked to URL: corbaloc::wasvr1:9812,:wasvr1:9811/NameServiceServerRoot
    5    Bound Java type: javax.naming.Context
    5    Local Java type: com.ibm.ws.naming.jndicos.CNContextImpl
    5    Corba binding type: org.omg.CosNaming.BindingType.ncontext
    5    Context unique ID: wasvr1Cell01/clusters/mycluster
    5    String representation: com.ibm.ws.naming.jndicos.CNContextImpl@466cadeb[wasvr1Cell01/clusters/mycluster]
```
```
Node agent of a standalone server:

   13 (top)/nodes/wasvr1Node03/servers/server1
   13    Linked to URL: corbaloc::wasvr1:9811/NameServiceServerRoot
   13    Bound Java type: javax.naming.Context
   13    Local Java type: com.ibm.ws.naming.jndicos.CNContextImpl
   13    Corba binding type: org.omg.CosNaming.BindingType.ncontext
   13    Context unique ID: wasvr1Cell01/clusters/mycluster
   13    String representation: com.ibm.ws.naming.jndicos.CNContextImpl@e946f7a5[wasvr1Cell01/clusters/mycluster]
```
 * In the nodeagent 
 * WLM information (cluster memebers and their weight) is returned to the client in the initial request
 * changes to WLM is communicated to the client in IIOP reply "service context"
 * References:
   * [Webcast Replay: How WLM routing and HA Manager work together in WebSphere Application Server ND](http://www-01.ibm.com/support/docview.wss?uid=swg27045558)
   * [Webcast replay: Workload Management (WLM) Overview and Problem Determination](http://www-01.ibm.com/support/docview.wss?uid=swg27012101)
   * [Troubleshooting: Workload Management Problems](http://www-01.ibm.com/support/docview.wss?uid=swg21250664)
   * [Workload management and high availability problem determination](ftp://ftp.software.ibm.com/software/iea/content/com.ibm.iea.was_v7/was/7.0/ProblemDetermination/WorkloadManagement.pdf)
   * [WebSphere Application Server V6 Scalability and Performance Handbook](http://www.redbooks.ibm.com/abstracts/sg246392.html?Open)
   * [Understanding how EJB calls operate in WebSphere Application Server V6.1](http://www.ibm.com/developerworks/websphere/techjournal/0807_pape/0807_pape.html)
   * [Comment lines: Tom Alcott: Everything you always wanted to know about WebSphere Application Server but were afraid to ask -- Part 2](http://www.ibm.com/developerworks/websphere/techjournal/0512_col_alcott/0512_col_alcott.html)
   
#### Initial Context Object:
* Using `com.sun.jndi.cosnaming.CNCtxFactory`, the initial context object is the cell context object in the namespace.  To lookup ejb, you need to start at cell `cell/clusters/.../ejb/...`.  You can start at server root context object by specifying it implicitly using corbaloc in the Context.PROVIDER_URL: `corbaloc:iiop:wasvr1:9811/NameServiceServerRoot`
* Using `com.ibm.websphere.naming.WsnInitialContextFactory`, the initial context object is the server root context object.  To lookup ejb, you can start directly at `ejb/....`
* The server root context in a cluster server member is : cell/clusters/mycluster
* The server root context in a standalone server is : cell/nodes/wasvr1Node03/servers/server1
* [Resource: Setting the provider URL property to select a different root context as the initial context](http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/rnam_example_prop5.html?lang=en)

#### WAS NameService and NamingContext extentions
* WAS Naming Service implementation is in two types of Corba Objects: 
  1. bootstrap name service object: `com.ibm.WsnBootstrap.WsnNameService` with one operation `getProperties`.  It is accessed through the bootstrap port with object key `WsnNameService` as opposed to Corba `NameService`.
    * `getProperties` returns a list of root context paths and their IORs. Example:
```
com.ibm.ws.naming.implementation : WsnIpCos
com.ibm.ws.naming.boot.serverrootctxids : <WsnNameSpaceRootNamingContextID>;wasvr1Cell01;wasvr1Cell01/clusters
com.ibm.ws.naming.boot.domainrootior : IOR:00bdbdbd0000003149444c3a636f6d2e69626d2f57736e4f7074696d697a65644e616d696e672f4e616d696e67436f6e746578743a312e3000bdbdbd0000000100000000000001a8000102bd0000000777617376723100bd2390bdbd000000634a4d4249000000124773e3aa37643062633737336533616166633334000000240000003f49454a5002013a46d549096d79636c75737465721a57736e44697374436f734f626a65637441646170746572574c4d0000000c77617376723143656c6c3031bd0000000a000000010000001400bdbdbd0501000100000000000101000000000049424d0a0000000800bd00011626000200000026000000020002bdbd49424d0b0000007a00000004000c77617376723143656c6c303100096d79636c75737465720000000223f323f423f523f60006776173767231239000067761737672312391000000350000000002000b434c55535445524e414d4500096d79636c7573746572000843454c4c4e414d45000c77617376723143656c6c303100000000bdbd000000030000001200bdbdbd0000000777617376723100bd2390bdbd000000030000001200bdbdbd0000000777617376723100bd2391bdbd49424d04000000050005020102bdbdbd0000001f0000000400bd0003000000200000000400bd0001000000250000000400bd0003
com.ibm.ws.naming.boot.serverrootctxid : wasvr1Cell01/clusters/mycluster
com.ibm.ws.naming.boot.serverrootname : wasvr1Cell01/clusters/mycluster
com.ibm.ws.naming.boot.noderootctxid : wasvr1Cell01/nodes/wasvr1Node03
com.ibm.ws.naming.boot.serverrootior : IOR:00bdbdbd0000003149444c3a636f6d2e69626d2f57736e4f7074696d697a65644e616d696e672f4e616d696e67436f6e746578743a312e3000bdbdbd0000000100000000000001bc000102bd0000000777617376723100bd2390bdbd000000764a4d4249000000124773e3aa37643062633737336533616166633334000000240000005249454a50020178641dd7096d79636c75737465721a57736e44697374436f734f626a65637441646170746572574c4d0000001f77617376723143656c6c30312f636c7573746572732f6d79636c7573746572bdbd0000000a000000010000001400bdbdbd0501000100000000000101000000000049424d0a0000000800bd00011626000200000026000000020002bdbd49424d0b0000007a00000004000c77617376723143656c6c303100096d79636c75737465720000000223f323f423f523f60006776173767231239000067761737672312391000000350000000002000b434c55535445524e414d4500096d79636c7573746572000843454c4c4e414d45000c77617376723143656c6c303100000000bdbd000000030000001200bdbdbd0000000777617376723100bd2390bdbd000000030000001200bdbdbd0000000777617376723100bd2391bdbd49424d04000000050005020102bdbdbd0000001f0000000400bd0003000000200000000400bd0001000000250000000400bd0003
com.ibm.ws.naming.boot.legacyrootctxid : wasvr1Cell01/persistent
com.ibm.ws.naming.boot.treerootctxids : 
com.ibm.ws.naming.boot.noderootname : wasvr1Cell01/nodes/wasvr1Node03
com.ibm.ws.naming.boot.domainrootctxid : wasvr1Cell01
com.ibm.ws.naming.boot.appsrootctxid : wasvr1Cell01/applications
com.ibm.ws.naming.boot.appsrootname : wasvr1Cell01/applications
com.ibm.ws.naming.boot.noderootior : IOR:00bdbdbd0000003149444c3a636f6d2e69626d2f57736e4f7074696d697a65644e616d696e672f4e616d696e67436f6e746578743a312e3000bdbdbd000000010000000000000100000102bd0000000777617376723100bd2390bdbd000000774a4d4249000000124773e3aa37643062633737336533616166633334000000240000005349454a5002002862529b07736572766572311d57736e44697374436f734f626a656374416461707465724e6f6e574c4d0000001f77617376723143656c6c30312f6e6f6465732f7761737672314e6f64653033bd00000007000000010000001400bdbdbd0501000100000000000101000000000049424d0a0000000800bd00011626000200000026000000020002bdbd49424d04000000050005020102bdbdbd0000001f0000000400bd0003000000200000000400bd0001000000250000000400bd0003
com.ibm.ws.naming.boot.noderootctxids : <WsnNameSpaceRootNamingContextID>;wasvr1Cell01;wasvr1Cell01/nodes
com.ibm.ws.naming.boot.legacyrootctxids : <WsnNameSpaceRootNamingContextID>;wasvr1Cell01
com.ibm.ws.naming.boot.domainrootctxids : <WsnNameSpaceRootNamingContextID>
com.ibm.ws.naming.boot.appsrootctxids : <WsnNameSpaceRootNamingContextID>;wasvr1Cell01
com.ibm.ws.naming.boot.treerootname : 
com.ibm.ws.naming.boot.treerootior : IOR:00bdbdbd0000003149444c3a636f6d2e69626d2f57736e4f7074696d697a65644e616d696e672f4e616d696e67436f6e746578743a312e3000bdbdbd0000000100000000000001bc000102bd0000000777617376723100bd2390bdbd000000784a4d4249000000124773e3aa37643062633737336533616166633334000000240000005449454a50020162cd1354096d79636c75737465721a57736e44697374436f734f626a65637441646170746572574c4d000000213c57736e4e616d655370616365526f6f744e616d696e67436f6e7465787449443e0000000a000000010000001400bdbdbd0501000100000000000101000000000049424d0a0000000800bd00011626000200000026000000020002bdbd49424d0b0000007a00000004000c77617376723143656c6c303100096d79636c75737465720000000223f323f423f523f60006776173767231239000067761737672312391000000350000000002000b434c55535445524e414d4500096d79636c7573746572000843454c4c4e414d45000c77617376723143656c6c303100000000bdbd000000030000001200bdbdbd0000000777617376723100bd2390bdbd000000030000001200bdbdbd0000000777617376723100bd2391bdbd49424d04000000050005020102bdbdbd0000001f0000000400bd0003000000200000000400bd0001000000250000000400bd0003
com.ibm.ws.naming.boot.legacyrootior : IOR:00bdbdbd0000003149444c3a636f6d2e69626d2f57736e4f7074696d697a65644e616d696e672f4e616d696e67436f6e746578743a312e3000bdbdbd0000000100000000000001b4000102bd0000000777617376723100bd2390bdbd0000006e4a4d4249000000124773e3aa37643062633737336533616166633334000000240000004a49454a50020182c85b9e096d79636c75737465721a57736e44697374436f734f626a65637441646170746572574c4d0000001777617376723143656c6c30312f70657273697374656e74bdbd0000000a000000010000001400bdbdbd0501000100000000000101000000000049424d0a0000000800bd00011626000200000026000000020002bdbd49424d0b0000007a00000004000c77617376723143656c6c303100096d79636c75737465720000000223f323f423f523f60006776173767231239000067761737672312391000000350000000002000b434c55535445524e414d4500096d79636c7573746572000843454c4c4e414d45000c77617376723143656c6c303100000000bdbd000000030000001200bdbdbd0000000777617376723100bd2390bdbd000000030000001200bdbdbd0000000777617376723100bd2391bdbd49424d04000000050005020102bdbdbd0000001f0000000400bd0003000000200000000400bd0001000000250000000400bd0003
com.ibm.ws.naming.boot.appsrootior : IOR:00bdbdbd0000003149444c3a636f6d2e69626d2f57736e4f7074696d697a65644e616d696e672f4e616d696e67436f6e746578743a312e3000bdbdbd0000000100000000000001b4000102bd0000000777617376723100bd2390bdbd000000704a4d4249000000124773e3aa37643062633737336533616166633334000000240000004c49454a5002015fd64e0d096d79636c75737465721a57736e44697374436f734f626a65637441646170746572574c4d0000001977617376723143656c6c30312f6170706c69636174696f6e730000000a000000010000001400bdbdbd0501000100000000000101000000000049424d0a0000000800bd00011626000200000026000000020002bdbd49424d0b0000007a00000004000c77617376723143656c6c303100096d79636c75737465720000000223f323f423f523f60006776173767231239000067761737672312391000000350000000002000b434c55535445524e414d4500096d79636c7573746572000843454c4c4e414d45000c77617376723143656c6c303100000000bdbd000000030000001200bdbdbd0000000777617376723100bd2390bdbd000000030000001200bdbdbd0000000777617376723100bd2391bdbd49424d04000000050005020102bdbdbd0000001f0000000400bd0003000000200000000400bd0001000000250000000400bd0003
com.ibm.ws.naming.boot.treerootctxid : <WsnNameSpaceRootNamingContextID>
com.ibm.ws.naming.boot.domainrootname : wasvr1Cell01
com.ibm.ws.naming.boot.legacyrootname : wasvr1Cell01/persistent
com.ibm.ws.naming.wsnidl.level : 3
```
```
// Code snippet for accessing WAS WsnNameService object
// If using Oracle JDK, then WAS security has to be disabled for this code to work

Properties props = new Properties();
ORB orb = ORB.init(args, props);
org.omg.CORBA.Object objRef = orb.string_to_object("corbaloc::wasvr1:9811/WsnNameService");
WsnNameService ns = WsnNameServiceHelper.narrow(objRef);
Prop[] nsProps = ns.getProperties();
```

  2. Naming Context object: `com.ibm.WsnOptimizedNaming.NamingContext` extends Corba `org.omg.CosNaming.NamingContextExt`.  It contains the same operations as Corba CosNaming in addition to extra operation `resolve_complete_info`.  This method looks up Corba Objects (EJB) as well as java values (JDBC definitions, JMS definitions, ...etc) that are bound in the namespace.  Corba implementation only looks up Corba Objects.  If a java value is being looked up, then it returns null (no Corba Object) and the java value is in one of the input parameter holder objects.
```
//Code snippet for acessing com.ibm.WsnOptimizedNaming.NamingContext object and calling its resolve_complete_info operation
// If using Oracle JDK, then WAS security has to be disabled for this code to work

Properties props = new Properties();
ORB orb = ORB.init(args, props);
org.omg.CORBA.Object objRef = orb.string_to_object("corbaloc::wasvr1:9812/NameServiceServerRoot");
NamingContext ns = NamingContextHelper.narrow(objRef);
NameComponent[] nameParams = {
  new NameComponent("ejb", ""),
  new NameComponent("MyEJBEAR", ""),
  new NameComponent("MyEJB.jar", ""),
  new NameComponent("MyService#com.myejb.view.MyServiceRemote", ""),
};
		
ContextIDStringsHolder ctxHolder = new ContextIDStringsHolder();
StringHolder strHolder1 = new StringHolder();
AnyHolder anyHolder = new AnyHolder();
StringHolder strHolder2 = new StringHolder();
NameHolder nameHolder = new NameHolder();
BindingTypeHolder bindHolder1 = new BindingTypeHolder();
BindingTypeHolder bindHolder2 = new BindingTypeHolder();
org.omg.CORBA.Object ejbObj = ns.resolve_complete_info(nameParams, ctxHolder, strHolder1, 
	anyHolder, strHolder2, nameHolder, 
	bindHolder1, bindHolder2);
```
* These classes exist in `WAS_INSTALL_HOME/runtimes/com.ibm.ws.ejb.thinclient_8.5.0.jar.  Add this jar to the client application to use these objects.

#### dump Namespace
* dump starting from cell context object: `dumpNameSpace -port 9809 -root cell -report long > /home/wasadmin/jndi.txt`
* dump starting from server root context object: `dumpNameSpace -port 2811 -root server -report long > /home/wasadmin/nodeagent1.txt`

#### Resources
* [How to lookup an EJB and other Resources in WebSphere Application Server using a Oracle JDK client](http://www-01.ibm.com/support/docview.wss?uid=swg21382740)
