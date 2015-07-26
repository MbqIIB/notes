#### Symptoms
1-


#### Investigation
1- Check log file:  last entry is:
```
<entry num='19' time='1437897990966' elapsed='10:31.44' level='INFO' thread='ModalContext'>
 <logger>com.ibm.cic.agent.internal.core.Director</logger>
 <method>prepare</method>
 <message>
  <key>Preparing {0}</key>
  <arg>com.ibm.bpm.ADV.v85 8.5.5000.20140604_1130</arg>
 </message>
</entry>
```
At this entry IM is hanging.  The thread name from the log entry is `ModalContext`
2- Increate logging to `DEBUG`: last entry is:
```
<entry num='16882' time='1437899753046' elapsed='00:55.49' level='DEBUG' thread='ModalContext'>
 <logger>com.ibm.cic.common.downloads.FileCacheManager</logger>
 <method>getFileCacheInfo</method>
 <message>FCM:   fetching {/lppsoft/S2-IBMSW/BPM/v8.5.5/repository/repository.config}</message>
</entry>
<entry num='16883' time='1437899753046' elapsed='00:55.49' level='DEBUG' thread='ModalContext'>
 <logger>com.ibm.cic.common.downloads.FileCacheManager</logger>
 <method>getFileCacheInfo</method>
 <message>FCM:   added {/lppsoft/S2-IBMSW/BPM/v8.5.5/repository/repository.config}, {13} remaining</message>
</entry>
```
The thread is trying to acess a file but still not clear why is hanging.
3. Do Java Dump `kill -3 ProcessID`.  A java core file is producted.  By Analysing the threads and looking at thread `ModalContext`, I found:
```
3XMTHREADINFO      "ModalContext" J9VMThread:0x32C12200, j9thread_t:0x31F4BD28, java/lang/Thread:0xCF77CD30, state:R, prio=6
3XMJAVALTHREAD            (java/lang/Thread getId:0x2B, isDaemon:false)
3XMTHREADINFO1            (native thread ID:0x107005B, native priority:0x6, native policy:UNKNOWN, vmstate:R, vm thread flags:0x00000001)
3XMCPUTIME               CPU usage total: 0.036034000 secs, user: 0.033045000 secs, system: 0.002989000 secs
3XMHEAPALLOC             Heap bytes allocated since last GC cycle=3554728 (0x363DA8)
3XMTHREADINFO3           Java callstack:
4XESTACKTRACE                at sun/nio/ch/FileDispatcherImpl.lock0(Native Method)
4XESTACKTRACE                at sun/nio/ch/FileDispatcherImpl.lock(FileDispatcherImpl.java:103)
4XESTACKTRACE                at sun/nio/ch/FileChannelImpl.lock(FileChannelImpl.java:1044)
```
It is clear that the thread is waiting on file locking.


#### Resolution:
* disable file locking for IM