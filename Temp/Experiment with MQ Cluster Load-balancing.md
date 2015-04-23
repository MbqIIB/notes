Experiment with MQ cluster load-balancing:

1- One gateway queue manager load balancing to cluster queue hosted in two other queue managers in the same cluster
2- in VMWare workstation limit the bandwidth of the network interface over which channels are communicating.  This the messages put into the cluster transmission queue will take time until they are delivered to their destination and therefore allowing one to inspect the message.
3- put many messages to the cluster queue.
4- make sure you have some depth in the cluster transmission queue, then stop the channels to inspect the messages and their headers.
5- In the transmission header of the message, see if there is any thing indicating which cluster channel should pick the message.
6- Another test to be done is to stop one channel and inspect the load-balancing that is going to happen to the messages destined to the stopped channel.
