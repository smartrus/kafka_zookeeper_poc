# Zookeeper installation and configuration

## Zookeeper standalone server installation

```
# tar -zxf zookeeper-3.4.12.tar.gz
# mv zookeeper-3.4.12 /usr/local/zookeeper
# mkdir -p /var/lib/zookeeper
# cat > /usr/local/zookeeper/conf/zoo.cfg << EOF
  tickTime=2000
  dataDir=/var/lib/zookeeper
  clientPort=2181
  EOF
# /usr/local/zookeeper/bin/zkServer.sh start
JMX enabled by default
Using config: /usr/local/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
#
```

Test it by telneting and running 4 letter word command like srvr:

```
# telnet localhost 2181
Trying ::1...
Connected to localhost.
Escape character is '^]'.
srvr
Zookeeper version: 3.4.12-e5259e437540f349646870ea94dc2658c4e44b3b, built on 03/27/2018 03:55 GMT
Latency min/avg/max: 0/0/0
Received: 1
Sent: 0
Connections: 1
Outstanding: 0
Zxid: 0x0
Mode: standalone
Node count: 4
Connection closed by foreign host.
```

Configuring a Zookeeper ensemble:

```
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
initLimit=20
syncLimit=5
server.1=zoo1.example.com:2888:3888
server.2=zoo2.example.com:2888:3888
server.3=zoo3.example.com:2888:3888
```

* initLimit - the amount of time to allow followers to connect
with a leader
* syncLimit - limits how out-of-sync followers can be with
the leader
* tickTime units make the initLimit 20 x 2000 ms, or 40 seconds
* server.X=hostname:peerPort:leaderPort
** X - The ID number of the server
** hostname - the hostname or IP address of the server
** peerPort - the TCP port over which servers in the ensemble communicate with each other
** leaderPort - the TCP port over which leader election is performed

The last configuration option is that each server must have a file in the dataDir directory with the name myid. This file must contain the ID number of the server,
which must match the configuration file.
