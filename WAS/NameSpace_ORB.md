### EJB Client Types:

* non-WLM clients
  * include all other corba clients that do not use WebSphere client code and logic embedded in WAS implementation of initial context.
  * For load-balancing between cluster server, the nodeagent plays an important role as it directs the EJB method call to the best available cluster server and provides basic load-balancing:
   * `InitialContext context = new InitialContext(env)`
     * The client connects to one of the cluster memebers bootstrap port
     * The boostrap replies with IIOP forward and return an IOR that contains all nodesagent hostname and port (using an  IOR `TAG_ALTERNATE_IIOP_ADDRESS` tag for every address)
     * The client uses the returned IOR to connect to one of the nodeagents.
     * The nodeagent replies with IIOP forward and return an IOR has one of cluster members hostname and port
   * `context.lookup`
     * The client using the IOR that points to one of the cluster memebers connects to the cluster memeber and sends IIOP request with operation `resolve`
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
// the lookup operation of CosNaming service takes NameComponents.  Each NameComponent has value and kind attributes.
// context.lookup takes a string representaion of NameCompoents where they are seperated by forward slash and the  value and king attributes of a NameCompoenent are seperated by dot.  This why if a context name contains a dot it has to be escaped.  Otherwise, that part after dot will be interpreted as a kind attribute.

Hashtable env = new Hashtable(); 
env.put(Context.INITIAL_CONTEXT_FACTORY,"com.sun.jndi.cosnaming.CNCtxFactory");
env.put(Context.PROVIDER_URL, "iiop://wasvr1:9811"); 
InitialContext context = new InitialContext(env);
context.lookup("cell/clusters/mycluster/ejb/MyEJBEAR/MyEJB\\.jar/MyService#com\\.myejb\\.view\\.MyServiceRemote");
```

* WLM clients
 * Clients that use WAS context implementation of `com.ibm.websphere.naming.WsnInitialContextFactory` and IBM ORB.
 * The nodeagent takes part in the initial requset but after that does not participate.
 * WLM information (cluster memebers and their weight) is returned to the client in the initial request
 * changes to WLM is communicated to the client in IIOP reply "service context"
 * References:
   * [Webcast Replay: How WLM routing and HA Manager work together in WebSphere Application Server ND](http://www-01.ibm.com/support/docview.wss?uid=swg27045558)
   * [Webcast replay: Workload Management (WLM) Overview and Problem Determination](http://www-01.ibm.com/support/docview.wss?uid=swg27012101)
   * [Troubleshooting: Workload Management Problems](http://www-01.ibm.com/support/docview.wss?uid=swg21250664)
   * [Workload management and high availability problem determination](ftp://ftp.software.ibm.com/software/iea/content/com.ibm.iea.was_v7/was/7.0/ProblemDetermination/WorkloadManagement.pdf)
