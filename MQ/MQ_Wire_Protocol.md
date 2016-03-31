MQ:
===
Sender Receiver channel batch:
* sync point(ommit and rollback) is relevant only to persistent messages.  You can see the non-persisten message immediately in destination queeu but persistence messages are only visible (although queeu depth will show that
there is a message but you will not be able to get it or browse it) until the channel batch commits.
* After every commit, the sender channel increments the LUW identifier (logical unit of work).  The channel status "Current LUWID" field shows the LUWID that is being used for the current batch
* obsevation: if the batch interval is set to zero in the sender channel, when a non persistent message is sent, the batch commit is not send immediately but after one second.  It seems to be an optimization to include as
many message in the same LUW as possible.  Since non-persistent messages are not affected by sync points (unit of work) and appear immediately in destination queue, they are not affected by this one second delay.  However,
if a persistent message is sent, the commit is send immediately.


http://www.mqtechconference.com/sessions_v2013/MQ_Internals_DeepDive.pdf


* MQExplorer workspaces

* sender / receiver channels parameters agreement messages
* sender / receiver batch size
* sender batch internval
* batch commit message from sender or receiver