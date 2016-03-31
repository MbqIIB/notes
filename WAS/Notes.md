##### Things to know before deleting temporary, cache and log files in WebSphere Application Server
https://www.ibm.com/developerworks/community/blogs/aimsupport/entry/deleting_temporary_files_in_websphere_application_server?lang=en

##### The WebSphere Contrarian: Run time management high availability options, redux
http://www.ibm.com/developerworks/websphere/techjournal/1001_webcon/1001_webcon.html
https://www.ibm.com/developerworks/community/forums/html/topic?id=20fe2cf6-c781-4067-b3a6-3fb5ea218f46

##### Changing WAS hostname and moving profiles to a new host:
  *  [The WebSphere Contrarian: Changing host names and migrating profiles in WebSphere Application Server](http://www.ibm.com/developerworks/websphere/techjournal/0905_webcon/0905_webcon.html)

##### Forcing JVM/WAS to resolve hostnames to IPv4 instead of IPv6 if the DNS or hosts file does not have IPv6
* The symptom of this is a timeout error the following exception stacktrace:
 java.net.Inet6AddressImpl.getHostByAddr(Native Method)
 java.net.InetAddress$2.getHostByAddr(InetAddress.java:985)

* Setting JVM property `-Djava.net.preferIPv4Stack=true` makes it resolve to IPv4 addresses
http://www-01.ibm.com/support/docview.wss?uid=swg21170467

##### Understanding, Tuning, and Testing the InetAddress Class and Cache
* http://www-01.ibm.com/support/docview.wss?uid=swg21207534

##### Using packet trace tools iptrace, snoop, tcpdump, wireshark, and nettl
* http://www-01.ibm.com/support/docview.wss?uid=swg21175744

##### PropFilePasswordEncoder to encode passwords in soap.client.props 
* https://websphereissues.wordpress.com/category/websphere-application-server/

##### Externalize from the profile:
* transaction log
* WAS applicaiton logs

This way we can take profile backup while the server is running because these are usally changed while the server is 	running

