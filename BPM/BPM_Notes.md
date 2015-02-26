> These notes were taken or tested while working with version 8.5

#### Name shortcuts used in BPM
```
IBM Business Process Manager (BPM)
Lombardi Teamworks (TW)
WebSphere Lombardi Edition (WLE)
WebSphere Process Server (WPS)
```

#### Concepts
* [Troubleshooting IBM Business Process Manager - *Dec 2013*](http://www.ibm.com/developerworks/bpm/bpmjournal/1312_chan/1312_chan.html)
* [Configuring error handling for Advanced Integration Services in IBM Business Process Manager Advanced V8 - *Oct 2012*](http://www.ibm.com/developerworks/bpm/library/techarticles/1210_agrawal/1210_agrawal.html)
* [Not sure if useful or outdated: Integrating WebSphere Process Server V7 and Lombardi Teamworks - *Jun 2010*](http://www.ibm.com/developerworks/websphere/library/techarticles/1006_fasbinder/1006_fasbinder.html)

#### [Business Process Choreographer Components](https://www-01.ibm.com/support/knowledgecenter/SSFTN5_8.5.0/com.ibm.wbpm.bpc.doc/topics/t2demanual.html?cp=SSFTN5_8.5.0%2F1-3-14-3-0&lang=en):
* Business Flow Manager: BPEContainer_*clusterName*
* Human Task Manager: TaskContainer_*clusterName*
* Business Process Choreographer Explorer: BPCExplorer_*clusterName*: 
* HTM predefined task templates: 
  * HTM_PredefinedTasks_*Vnnn_clusterName*
    The task templates provided can be seen by going: `SCA modules -> HTM_PredefinedTasks_Vnnn_clusterName -> Human tasks` .
    Those can also be seen from BPCExplorer under `Task Templates`
    1. Approval_Request
    2. Question
    3. Review_Request
    4. Todo
  * HTM_PredefinedTaskMsg_*Vnnn_clusterName*: 
    Same as above but contains only one template
    1. Message
