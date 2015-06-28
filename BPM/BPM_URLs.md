## The below information is validated for IBM BPM v8.5.5

#### BPM URLs.  Reference: [Configuring IBM BPM endpoints to match your topology](http://www-01.ibm.com/support/knowledgecenter/SSFPJS_8.5.5/com.ibm.wbpm.imuc.stbpm.doc/topics/tsec_thirdpartyauthentication_endpointservice.html?lang=en)
* The URLs computed and returned from all BPM applications (Lombardi) are organized into three generic scenarios and for each scenario and set of URL computation strategies are defined.  The following are the scenarios
  * EXTERNAL_CLIENT: intended for non-relative URLs to be used by clients outside the data center, such as web browsers or process designer
  * INTERNAL_CLIENT: this is for communication between BPM components
  * Relative:  intended for relative URLs to facilitate access to browser-based web applications through various entry points.  So this facilitates accessing the server directly or through a proxy
* There are also specific scenarios for the different aspects of BPM applications.  Configuring these specific senarios is optional and if a scenario is not configured it defaults to one of the generic scenarios and follows its URL computation strategy.  The default of each specific scenario is documented in the reference.
* The BPM URL scenarios and their associated strategies are stored in `PROFILE_ROOT/config/cells/PROD-PServerCell/cell-bpm.xml`.  Inside the file, there is a section named `bpmurls` which has the configuration of the three generic scenarios as well as any specific scenario to override the its generic default.
* For each scenario, a BPMURL object (this is the xml entry for a scenario in cell-bpm.xml) defines a list of strategies. The strategies are attempted in the order that is specified until one returns the required information. Each strategy uses a different approach to determine the transport protocol, host, and port that are used to generate URLs, for example, by extracting them from a particular header in the request. The BPMURL object can also reference a `BPMVirtualHostInfo` object that contains fixed values for the transport protocol, host name, port number of the virtual host, and any URL prefix
* You can define a default virtual host for the deployment environment.  All scenarios that need to reference a `BPMVirtualHostInfo` and does not have one define will default to this virtual host.

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

#### BPC Explorer
* The BPC REST APIs are not part of the REST Services Gateway and the endpoints used are configured in `WebSphere application server clusters > SupCluster > Business Process Choreographer Explorer > BPCExplorer_SupCluster`

#### Proxy Server load-balancing
* Update virutalhost mapping of BPM Apps: `BPMConfig -update -profile PROFILE_NAME -de DE_name -virtualHost virtualHostName` the web modules of the IBM BPM applications in the specified deployment environment are mapped to the specified virtual host. If you have more than one deployment environment in your cell, you can either use context root prefixes to differentiate between the multiple deployment environments or you can use the BPMConfig -update -virtualHost command to configure another virtual host.
  * [Reference: BPMConfig command-line utility](http://www-01.ibm.com/support/knowledgecenter/SSFPJS_8.5.5/com.ibm.wbpm.ref.doc/topics/rbpmconfig.html?lang=en) 
  * Note: this addition to BPMConfig is new v8.5.5.  In previous versions, there was a standalone command `updateVirtualHost `.
