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
