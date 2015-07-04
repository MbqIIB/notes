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
     *  The client sends IIOP request using the IOR to one of the nodeagents.  The nodeagent replies with IIOP forward and returns an IOR of a the best cluster memeber.
     *  The client connect to the cluster member and sends IIOP request and continues to use that member.
  * 
