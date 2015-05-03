#### Connect Process Server to Process Center
* [Recovering from an offline server for IBM Business Process Manager Advanced 8.5.5](https://www.ibm.com/developerworks/community/blogs/aimsupport/entry/offline_bpm_855?lang=en)
* This configuration is stored in:
  * single instance : `config\cells\PSCell1\nodes\Node1\servers\server1\server-bpm`
  * cluster: `config\cells\PSCell\clusters\AppCluster\cluster-bpm`
  * http://www-01.ibm.com/support/knowledgecenter/SSFTBX_8.5.5/com.ibm.wbpm.admin.doc/topics/connect_runtime_to_processcenter.html?lang=en
  * BPMClusterConfigExtension or BPMServerConfigExtension is the name of the XML element in cluster-bpm or server-bpm
```
Note: If a server is currently running, do not run the AdminConfig tool in local mode. Configuration changes that are made in local mode are not be reflected in the running server configuration. If you save a conflicting configuration, you could corrupt the configuration. 
```
#### BPM Database cleanup
* [Technote1](http://www-01.ibm.com/support/docview.wss?uid=swg21439859)
* [Technote2](http://www-01.ibm.com/support/docview.wss?uid=swg21661709)
* [Technote3](http://www-01.ibm.com/support/docview.wss?uid=swg21612755)
