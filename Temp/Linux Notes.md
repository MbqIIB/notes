Linux Notes:
=============

* ulimit.  hard limit. soft limit
http://www.linuxhowtos.org/Tips%20and%20Tricks/ulimit.htm
http://ithubinfo.blogspot.com/2013/07/how-to-increase-ulimit-open-file-and.html
http://stackoverflow.com/questions/344203/maximum-number-of-threads-per-process-in-linux

In Java/WAS, the following exception can be thrown because of limits and not memory:java.lang.OutOfMemoryError: Failed to create a thread.  
http://www-01.ibm.com/support/docview.wss?uid=swg21633466

WAS:
http://www-01.ibm.com/support/docview.wss?uid=swg21407889

* the limits set in Oracle 11g server:
# oracle-rdbms-server-11gR2-preinstall setting for nofile soft limit is 1024
oracle   soft   nofile    1024

# oracle-rdbms-server-11gR2-preinstall setting for nofile hard limit is 65536
oracle   hard   nofile    65536

