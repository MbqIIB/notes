##### Using MQSC command line
* To backspace: use Ctrl-Backspace in the shell. [Reference](http://www-01.ibm.com/support/docview.wss?uid=swg21240694)
* For more flexibility and to combine the result with other shell tools using pipes or to be included in a shell script:
	* `echo "MQSC command" | mqsc QMGR_NAME` 


##### MQGET
* Behaviour of MQGET depends on MQ Message object passed as parameter:
	* If MQMD has non-NULL (not zeros) MSGID, then MQGET will fetch from queue the message having a matching MSGID
	* if MQMD has non-NULL (not zeros) CorrelID, then MQGET will fetch from queue the message having a matching CorrelID
	* If MQMD has a non-NULL MSGID and CorrelID, then MQGET will fetch the message that matches both.
	* If both MQMD and CorrelID are NULL, then the next message in the queue is fetched.
	* In MQGET MessageOptions v2, a new field added 'matchingOptions' which can specify if matching should happen on MSGID, CorrelID or None.  This is a convenience in programming so that you do not have to reset MsgID or CorrelID to NULL in the message object in case you do not need to match against MSGID or CorrelID.

##### [How the Message ID and Correlation ID are handled in a Transmission Queue Techdoc](http://www-01.ibm.com/support/docview.wss?uid=swg27021177)

##### [How to match correlation id's when request is made via JMS application and reply generated from base Java API](http://www-01.ibm.com/support/docview.wss?uid=swg21569646)