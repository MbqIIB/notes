## The below information is validated for IBM BPM v8.5.5

#### BPM URLs.  Reference: [Configuring IBM BPM endpoints to match your topology](http://www-01.ibm.com/support/knowledgecenter/SSFPJS_8.5.5/com.ibm.wbpm.imuc.stbpm.doc/topics/tsec_thirdpartyauthentication_endpointservice.html?lang=en)
* The URLs computed and returned from all BPM applications (Lombardi) are organized into three generic scenarios and for each scenario and set of URL computation strategies are defined.  The following are the scenarios
  * EXTERNAL_CLIENT: intended for non-relative URLs to be used by clients outside the data center, such as web browsers or process designer
  * INTERNAL_CLIENT: this is for communication between BPM components
  * Relative:  intended for relative URLs to facilitate access to browser-based web applications through various entry points.  So this facilitates accessing the server directly or through a proxy
* There are also specific scenarios for the different aspects of BPM applications.  Configuring these specific senarios is optional and if a scenario is not configured it defaults to one of the generic scenarios and follows its URL computation strategy.  The default of each specific scenario is documented in the reference.
* The BPM URL scenarios and their associated strategies are stored in `PROFILE_ROOT/config/cells/PROD-PServerCell/cell-bpm.xml`.  Inside the file, there is a section named `bpmurls` which has the configuration of the three generic scenarios as well as any specific scenario to override the its generic default.
* For each scenario, a BPMURL object (this is the xml entry for a scenario in cell-bpm.xml) defines a list of strategies. The strategies are attempted in the order that is specified until one returns the required information. Each strategy uses a different approach to determine the transport protocol, host, and port that are used to generate URLs, for example, by extracting them from a particular header in the request. The BPMURL object can also reference a `BPMVirtualHostInfo` object that contains fixed values for the transport protocol, host name, port number of the virtual host, and any URL prefix
* You can define a default virtual host for the deployment environment.  All scenarios that do not return a specific protocol, host, port and context-root combination will default to to this virtual host.

#### Process Portal
* Process portal is based on busienss space
* by default process portal uses relative URLs.
* The configuration for process portal URLs is part ob BPM URL configuration
* There are scenarios specific for process portal.  As indicated in [Configuring IBM BPM endpoints to match your topology](http://www-01.ibm.com/support/knowledgecenter/SSFPJS_8.5.5/com.ibm.wbpm.imuc.stbpm.doc/topics/tsec_thirdpartyauthentication_endpointservice.html?lang=en), the default scenario is 'Relative' which follows the `RelativeUrlStrategy`.
* Process Portal accesses BPM REST APIs.  The URL for these services returned from server are relative (using inspector tool in firefox to look at the returned html):
```
var bpm_endpoint_urls = {
	"webviewer.war": "/WebViewer",
	"process-portal.war": "/ProcessPortal",
	"process-portal-support.war": "/portal",
	"bpmrest.war": "/rest/bpm/wle",	
	"webviewer.war.js": "/WebViewer",
	"process-portal.war.js": "/ProcessPortal",
	"process-portal-support.war.js": "/portal",
	"bpmrest.war.js": "/rest/bpm/wle"
};
```
* The specific BPM URL scenarios related to accessing BPM REST API are `PROCESS_PORTAL_TO_BPM_REST` and `PROCESS_PORTAL_TO_BPM_REST_JS`
* I have tested configuring these specific scenarios and selecting `HttpProtocolHostStrategy` strategy, so that the BPM REST URL will follow the http header in request.  The url used to access process portal was `https://bpmhost:9444/ProcessPortal` and the following was return from server:
```
var bpm_endpoint_urls = {
	"webviewer.war": "/WebViewer",
	"process-portal.war": "/ProcessPortal",
	"process-portal-support.war": "/portal",
	"bpmrest.war": "https://bpmhost:9444/rest/bpm/wle",	
	"webviewer.war.js": "/WebViewer",
	"process-portal.war.js": "/ProcessPortal",
	"process-portal-support.war.js": "/portal",
	"bpmrest.war.js": "https://bpmhost:9444/rest/bpm/wle"
};
```
* One thing was noticed is that the URLs are computed once when they are first accessed and after that they are cached. I have tried accessing process portal using a different host but I was always getting `bpmhost:9444` for `bpmrest.war` and `bpmrest.war.js` and process portal will stop loading in the middle because the browser does not allow accessing URLs not from the same origin.  Obviously, process portal does not have ajax proxy.

#### Business Space
* Business Space uses Federated BPM REST Services for Processes and Tasks which has endpoint `/bpm/federated/bfm` and `/bpm/federated/htm` and is provided by `REST Services Gateway` application rather than directly accessing the REST service in `bfmrestapi.war` module in `BPEContainer_AppCluster` application and `taskrestapi.war` module in `TaskContainer_AppCluster` application which has endpoints `/rest/bpm/bfm` and `/rest/bpm/htm`

#### REST Services and config-rest.xml
* config-rest.xml contains the definitions of all REST service provider applications and their associated endpoints.
* the file is located in `PROFILE_ROOT/config/cells/PROD-PServerCell/config-rest.xml`
* The purpose of `config-rest.xml` is to store all REST providers and their endpoints for the Admin Console to read and present.  Specifically, `WebSphere application server clusters > AppCluster > REST service endpoint registration` reads from this file and present several endpoint target for the user to select from for business space.  From there, The admin console updates `Resource environment providers > Mashups_Endpoints`.  Also, `WebSphere application server clusters > AppCluster > REST services` reads from this file and allows user to update hostname and port but only constrained to `REST Services Gateway_AppCluster` REST services.
* All values that are changed in `REST services` and subsequently in `REST service endpoint registration` are updated in `Resource environment providers > Mashups_Endpoints` automatically.
* `config-rest.xml` is not used by any application to get REST service endpoints.  It is just a convenience.
* Note: if you want to update hostname and port of dmgr REST servces, 1)shutdown dmgr. 2)update hostname and port of `REST Services Gateway Dmgr`. 3)start dmgr.  4) go to `REST service endpoint registration` and `Resource environment providers > Mashups_Endpoints` and see that values have changed.
 is not used by any application to get the endpoint of REST services. The sole purpose of this file is to document the rest endpoints for display in WAS admin console.
* There is and admin task to update `config-rest.xml` : `AdminTask.updateRESTServiceProvider`

#### BPC Explorer
* The BPC REST APIs are not part of the REST Services Gateway and the endpoints used are configured in `WebSphere application server clusters > SupCluster > Business Process Choreographer Explorer > BPCExplorer_SupCluster`

#### Proxy Server load-balancing
* Browsers have same origin URL access security restriction that mandates using a proxy (routing) server when URLs accessed are distributed across multiple nodes
* Update virutalhost mapping of BPM Apps: `BPMConfig -update -profile PROFILE_NAME -de DE_name -virtualHost virtualHostName` the web modules of the IBM BPM applications in the specified deployment environment are mapped to the specified virtual host. If you have more than one deployment environment in your cell, you can either use context root prefixes to differentiate between the multiple deployment environments or you can use the BPMConfig -update -virtualHost command to configure another virtual host.
  * [Reference: BPMConfig command-line utility](http://www-01.ibm.com/support/knowledgecenter/SSFPJS_8.5.5/com.ibm.wbpm.ref.doc/topics/rbpmconfig.html?lang=en) 
  * Note: this addition to BPMConfig is new v8.5.5.  In previous versions, there was a standalone command `updateVirtualHost `.
* a proxy server can load balance to backend servers based on:
  * front end port: give each web app or service and different fron-end port that identifies it.  This does not mandate that the backend apps are listeneing on the same front-end port
  * virutal hostname: sharing the same front end port, a proxy server can identify different web apps or service based on the incoming virtual hostname (http HOST header).  In fact, the port is part of the virtual hostname.  The first case can be considered a generic hostname with port `*:port`.  This also requires that the web app are configured with WAS virtualhost matching HOST header.
  * context root: if different apps or services share the same port and virtual hostname, then by giving each one a different front-end context root prefix, the proxy can identify each app and service and load balance to their respective backend servers.  This does not mandate that the backedn web app or service to have the same context root.  In fact, different apps in different backend server can have the same context root but different front-end context root.
* In all of the above strategies of identifying apps at the proxy, it is important that the backend web application is deployed to the correct WAS virtualhost.  Here are the different scenarios that the proxy could do regarding virtualhost:
  * A full proxy server will open a new connection to backend server.  The first scenario is that the http HOST header received by backend server is the its DNS hostname and port used by proxy to access it.
  * the second scenario is that the proxy server could pass the original HOST header from client to backend server.
  * the third scenario is that the proxy server could pass the originl HOST header from client in `X-Forwarded-Host` header.
* Depending on the configuration and behaviour of proxy server, a BPM environment needs to be configured to for URLs to function properly.
* [Reference: Real life usage of the X-Forwarded-Host header?](http://stackoverflow.com/questions/19084340/real-life-usage-of-the-x-forwarded-host-header)

```
Summarization of the above:
A full proxy server open a new connection to the backend server:
	1- forward the same host header received from the client.
	2- just open a connection to the backend and in this the host header will be the hostname(DNS name) of
		backedn server
	3- inlcude x-forwardedhost header

In all of the above cases, you have to make sure that the WAS virtual host of the backend application will
match whatever the proxy server is going to send in host header


In case of multiple deployment environments:
The proxy server will route requests based on 'virtualhost' or 'root context prefix'


Strategies for load-balancing to backend:
1- load-balance based on fron-end port.  Have a different port for every load-balancing group
2- sharing the same port, load balance based on virtualhost
3- sharing the same port, load balance based on URL (application context root)
```
