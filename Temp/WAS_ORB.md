WsnNameService

IDL:com.ibm/WsnBootstrap/WsnNameService:1.0

Notes about Corba:

Interoperability of WebSphere ORB with other ORBs:
==================================================
1- NameService extensions : when using jndi (com.ibm.websphere.naming.WsnInitialContextFactory) is used WsnNameService is used instead of NameService.  The extension allows storing non-corba objects (Java Objects)
2- Authentication Protocols that are specific for WebSphere: BasicAuth, LTPA

JNDI lookup of a resource non-EJB, returns an object representation of the resource will all information in the resource included.  Then JNDI creates objects according to this resource information.


ORB ports:
* bootstrap
* ORB Listener
* CSIv secur
* ...

Stages in JNDI communication from standalone clients to WebSphere ORB:
1- Connect to Bootstrap port send 'locate' request for 'WsnNameService'.  The bootstrap returns the IOR to 'WsnNameService' service which contains the host and ip address
2-.... ?? look at LSD and WLM and the mechanism ulitlized and wether thin client with JNDI takes advantage of this.
http://www.ibm.com/developerworks/websphere/techjournal/0512_col_alcott/0512_col_alcott.html
http://www.ibm.com/developerworks/websphere/techjournal/1109_col_vanrun/1109_col_vanrun.html
http://www.ibm.com/developerworks/websphere/techjournal/0807_pape/0807_pape.html
https://books.google.com.sa/books?id=PJ49xcoRb1QC&pg=PA338&lpg=PA338&dq=websphere+application+server+%22LSD%22+WLM&source=bl&ots=qkyL6aPqH6&sig=cgJvRMyCXSnc1Byy4q-V3yYs5Cg&hl=en&sa=X&ei=g8vRVO7jENDuaOb6gJAK&ved=0CFMQ6AEwCQ#v=onepage&q=websphere%20application%20server%20%22LSD%22%20WLM&f=false

Advantage of JNDI Naming Context API EJB access over Corba API is:
1- fault tolerance: you can specify multiple bootstrap servers for the JNDI initial context.  
2- JNDI naming caching at client side.
3- WLM advantages?? need to check
http://www.ibm.com/developerworks/websphere/techjournal/1109_col_vanrun/1109_col_vanrun.html

http://www.redbooks.ibm.com/redpapers/pdfs/redp4308.pdf


WAS Security:
The token of single sign-on LTPA (in HTTP cookie or IIOP context) has the combination of username and realm(user registry).  When request comes to another server, the user and realm are compared against the existing one to determine if the user exists.  This why for SSO to work the same realm has to be accessed by the different servers.

* if global security is not enabled, the CSIv2 inbound settings are ignored and the TCP/IP orb port 9100 is set in the IOR of remote object even is 'SSL-Required' is configured in inbound CSIv2.

* Using Corba NameService interface and resolve method to lookup non remote objects like UserTransaction resulted in null returned.
While with JNDI API and object is returned.  
The jndi uses resolve_complete_info method of com.ibm.WsnOptimizedNaming.NamingContext.  This method take valuetype parameters 
which are 'out' parameters that hold returned values from method invocation (In corba IDL you can defind parameter as in or out)
?? Check the contents of these parameters for non corba objects
