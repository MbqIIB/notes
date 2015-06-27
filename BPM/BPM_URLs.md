#### BPM URLs.  Reference: [Configuring IBM BPM endpoints to match your topology](http://www-01.ibm.com/support/knowledgecenter/SSFPJS_8.5.5/com.ibm.wbpm.imuc.stbpm.doc/topics/tsec_thirdpartyauthentication_endpointservice.html?lang=en)
* The URLs computed and returned from all BPM applications (Lombardi) are organized into three generic scenarios and for each scenario and set of URL computation strategies are defined.  The following are the scenarios
  * EXTERNAL_CLIENT: intended for non-relative URLs to be used by clients outside the data center, such as web browsers or process designer
  * INTERNAL_CLIENT: this is for communication between BPM components
  * Relative:  intended for relative URLs to facilitate access to browser-based web applications through various entry points.  So this facilitates accessing the server directly or through a proxy
* There are also specific scenarios for the different aspects of BPM applications.  Configuring these specific senarios is optional and if a scenario is not configured it defaults to one of the generic scenarios and follows its URL computation strategy.  The default of each specific scenario is documented in the reference.
* The BPM URL scenarios and their associated strategies are stored in `PROFILE_ROOT/config/cells/PROD-PServerCell/cell-bpm.xml`.  Inside the file, there is a section named `bpmurls` which has the configuration of the three generic scenarios as well as any specific scenario to override the its generic default.

#### Process Portal
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

#### REST Services and config-rest.xml

#### BPC Explorer
* The BPC REST APIs are not part of the REST Services Gateway and the endpoints used are configured in `WebSphere application server clusters > SupCluster > Business Process Choreographer Explorer > BPCExplorer_SupCluster`
