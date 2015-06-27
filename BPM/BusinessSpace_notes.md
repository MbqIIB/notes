#### Business Space URLs:
* The URLs returned in business space are of three types as seen by firefox `developer tools -> Network`:
 * href URLs related to business space itself as an application:
  * Relative href URLs: These URLs do not have any host or port information and therefore are accessible regardless of the host name and port used in the browser 
  * Absolute href URLs: These URLs are complete with `https://hostname:port/xxx` where the hostname and port is taken from http host header.  Therefore, whatever hostname and port used in the browser is going to be rewritten in these URLs. ` Note: I have tested this with the ModifyHeaders plugin in firefox by forcing a host header to be sent that is different from the one types in the URL field and noticed that the URLs returned have followed the host header`
   * If business space is accessed through a proxy and the proxy does not rewrite the host header (for security reasons as some proxies do), then business space works fine without any special configuration as the hostname of the proxy will be returne in such URLs.
   * If the proxy server rewtries the host header
 * URLs for calling external REST service by widgets:
 * 


#### The Ajax Proxy
* For security purposes, the Ajax proxy is restricted and can forward requests only to configured endpoint URLs.  To configure more URLs, the following is the procedure:
  * [Technote: Registering additional URLs as endpoints](http://www-01.ibm.com/support/docview.wss?uid=swg21570464)

#### BPM Process Portal:
* IBM BPM Process Portal is based in business space.
* If Process Portal is deployed on a cluster different from the cluster where BPM REST services are hosted then a proxy needs to be configured to access both Process Portal and The REST services to maintain same origin restriction of the browser.
