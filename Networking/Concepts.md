#### Firewall
* Close Age out:
  * The firewall will not allow any packets to flow for a `Close Age Out` connection. A connection is identified by its source ip and port and destination ip and port.
  * There are two scenarios for Close Age out in Juniper firewall:
    1. Stale connection: the age out means that an active connection has timed out due to no traffic flowing that would keep that connection alive. For such connections, both communicating peers will be unaware that their connection is timed out by the firewall. Their packets will not reach its destination but be dropped by the firewall. The firewall does not send any RST or FIN packets to either communicating ends when it times out the connection.
    2. Syn traffic black hole: syn packet is sent but no response from peer (usually if the service is down and port is not open, RST packet is sent by server as a response).  The firewall will see a Syn request.. build a partial/embryonic session with a time out of 30 seconds (3 ticks).
  * [Reference](http://forums.juniper.net/t5/ScreenOS-Firewalls-NOT-SRX/many-drops-with-msg-AGE-OUT-quot-while-running-TCP-and-UDP/td-p/20555) 


#### How a closed TCP connection is detected
* Connection is closed by process:
  * Either of the communicating peers can send FIN or RST packet to the other end to request connection gracefully or abruptly.
* Connection is no longer valid because a process is dead:
  * Soft failure: the process died but the host server is still running.  In this case, the OS will send RST packet to othe peer.
  * Hard failure: Server host crached or network disconnected: In this case, no RST packet will be sent to the other side. There is no way for the other end to know that the connection is no longer valid if the connection remains idle with no packets flowing. If packets are flowing, then the other end will detect that the connection is not valid when it times out waiting for response or acknowledgement for packets sent.  If a connection is idle, then there are two ways of detecting an invalid connectino:
    1. Process level heartbeat: the process sends heartbeat messages to the other end through an agreed heartbeat protocol.
    2. Socket level heartbeat: the OS handles sending KEEP_ALIVE packets to the other end of the socket.  If the other end does not response with ack, the sending OS peer will timeout waiting and close the connection as invalid.

#### TCP KEEP_ALIVE
* KEEP_ALIVE is a packet that has zero length data payload and sequence number that is the current sequence number - 1
* Example: two communication peers A and B.  A sends a packet with `seq 5` and data payload length of 3.  B responds with `ack 9`.  9 becomes the current sequences number that A has to use to send new packets that contain data as 9 will be the sequences number of the first byte in the new packet data payload.  Then, if A wants to send KEEP_ALIVE, it sends a packet with `seq 8` and zero data length. B response withs `ack 9`.
* [Resource: TCP keepalive overview](http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/overview.html)
* [Resource video: Tips in Packet Analysis - What is a TCP Keep Alive?](https://www.youtube.com/watch?v=j8lgFaIajko)

