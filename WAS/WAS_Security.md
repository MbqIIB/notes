##### LTPA
* In web application, when WAS authenticates a user (form-based authentication), it set two imporant cookies:
  * JSESSIONID: the value of the cookie is a string constituted of: cache ID, session ID, separator `:`, clone ID, and partition ID. JSESSION ID will include a partition ID instead of a clone ID when memory-to-memory replication in peer-to-peer mode is selected. Typically, the partition ID is a long numeric number.  
   * Reference: `WebSphere Application Server V7: Session Management` redbookt, page 15.
   * [How to find out the WAS server handling request from JSESSIONID]http://wpcertification.blogspot.com/2009/09/how-to-find-out-was-server-handling.html)
  * LtpaToken2: base64 encoded string of an encrypted concatenated string of expiration time, user identiy and signature.
    * [Reference1: Understanding LTPA](http://www-01.ibm.com/support/knowledgecenter/SS9H2Y_5.0.0/com.ibm.dp.xs.doc/understandingltpa.htm)
    * [Reference2: LTPA versions and token formats](http://www-01.ibm.com/support/knowledgecenter/SS9H2Y_5.0.0/com.ibm.dp.xs.doc/understandingltpa02.htm%23ltpaversions)
    * [Decode LTPA token](http://wrschneider.blogspot.com/2011/10/quick-and-dirty-sso-with-ltpa.html)
* WAS has an authentication cache in which is stores authentication credentials and other security stuff.  When an LTAP token is sent to WAS, it checks if it has seen this token before in the cache and if it has then it authenticates the request without validating the token. If it does not exist in the cache, then it validates the token (by decrypting and validating signature) and if it authenticates the token it adds it to the cache.
 * [Reference: WAS v8.5.5 Authentication cache settings](http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/usec_sec_domains_cache.html?cp=SSAW57_8.5.5)
* The authentication cache stores the following:
  * User credentials with the LTPA token as key. (the LTPA token as key is my guess based on test where I change on character of the string of an already authenticated LTPA token in the cookie sent from browser and the WAS does not accept the request and directs to login page.  If it was not taking the token as key then it would not have detected this since it will only validate token content for the first time only)
  * Username and password: The password is hashed in one-way hash that cannot be reversed and both the username and hashed password are used as key to the cache.  Whenever a username and password is used, WAS uses the combination of username and hased password as key into the cahce.  If an entry is found, then the user is considered authenticated and his credentials are retrieved from the entry.  This is to avoid accessing user registry every time username and password authentication is used.
  * My understanding: it seems that authentication cache entries can have two keys: one is the LTPA token and the other is the username and password or that these are maintained in two seperate caches.
  * Using username and the hashed password as key to the cache explains why when changing the password in backend, a user can login with both old and new password at the same time.  The username with old password forms a key and this it can be looked up in the cache and therefore the request is validated.  The username with new password as key does not exist in the cache and WAS looks it up in backend user registry and then adds to the cache.
```
Test TODO: use LDAP as registry in WAS and do network sniffing on LDAP traffic.  Login with the username and password multiple times and watch the LDAP packets to see how many times the registry is accessed.  Then disable username and password caching and how many times the registry is accsssed
```

* an LTPA token can be used as long as it is not expired.  LTPA token expiry is a seperate concept from http session timeout.  The same token can be used with multiple different sessions and WAS accepts http requests with the LTPA token.  In general, when the http session is expired, the web application will potentially direct the user to login page and upon a new login an new token will be generated.  However, in principle if the same token is used with the new session, it is still accepted.  I have conducted the below two tests Using `Modify Headers` firefox extension to change the cookie header sent from browser:
  * First test: Login to the admin console and capture the ltaptoken using firefox `Developer tools -> Network`.  Then logout and login again.  In this case a new LTPA token is generated and set in the cookie. Using modify headers change the ltpatoken of the new login with one from previous login preserving all other tokens.  The result is that WAS still accepts all HTTP requests.
  * Second test: login to admin console with one user and from another window login with a different user.  Take and ltpatoken from the first user login and set it as the ltpatoken for the second user.  The result is that WAS still accepts all HTTP requests of the second user.
```
Test TODO: write a servlet application with a login function that print the principal id of the logged in user.  Login with two different users and change the ltpatoken of the second user to the one of th first user and then see what the servet is going to print as principle id for the first user
```
````
Test TODO: write a servlet with basic authentication and then formulate an http request and include LTPA cookie (using firefox ModifyHeaders extention) without the header of the username and password of the basic authentiation and see if the request is accepted.  If it does, then the container looks for this single sign-one cookie first regardless of the web security configured for the web module
````

### Resources
* [WAS v8.5.5 Tuning security configurations](http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/tsec_tune.html?cp=SSAW57_8.5.5%2F1-12-2-9-0&lang=en)


#### TODO: links about WAS security cache
* http://www.coderanch.com/t/71921/Websphere/Clear-refresh-credentials-cache
* ftp://ftp.software.ibm.com/software/iea/content/com.ibm.iea.was_v8/was/8.0/Security/WASv8_VMM.pdf
* (was-85-security-hardening.pdf) http://www.websphereusergroup.org/southernontario/go/document/download/2c88dd749cd5104adb827ab531fa2136
* http://www-01.ibm.com/support/docview.wss?uid=swg21240176
* http://www-01.ibm.com/support/docview.wss?uid=swg21078845
* http://www-01.ibm.com/support/docview.wss?uid=swg21320747
* http://www.ibm.com/developerworks/websphere/techjournal/1003_botzum/1003_botzum.html#sec2c

##### WIM : WebSphere Identity Manager
* File based user registry is stored in fileRegistry.xml under profile/config/cells/cellname.
** The passwords in the file are hashed(or encrypted) and the base64 encoded
* The user registry repository configuration is stored in wimconfig.xml under profile/config/cells/cellname/wim/config
* The default reposirty information and basic configuration is in security.xml under profile/config/cells/cellname
* Resources:
** A Guide to Password Use in WebSphere Application Server: https://www.globalknowledge.com/ca-en/resources/resource-library/white-paper/a-guide-to-password-use-in-websphere-application-server/

##### Authenication aliases:
* stored in security.xml under profile/config/cells/cellname
* encoded with XOR cypher
