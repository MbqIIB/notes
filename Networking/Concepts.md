#### Firewall
* Close Age out:
  * The firewall will not allow any packets to flow for a `Close Age Out` connection. A connection is identified by its source ip and port and destination ip and port.
  1. Stale connection: the age out means that an active connection has timed out due to no traffic flowing that would keep that connection alive.  For such connections, both communicating peers will be unaware that their connection is timed out by the firewall.  Their packets will not reach its destination but be dropped by the firewall. The firewall does not send any RST or FIN packets to either communicating ends when it times out the connection.
  2. Syn traffic black hole: syn packet is sent but no response from peer (usually if the service is down and port is closed, RST packet is sent by server as a response).  The firewall will see a Syn request.. build a partial/embryonic session with a time out of 30 seconds (3 ticks).
