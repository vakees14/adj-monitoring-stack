# adj-monitoring-stack
To Monitoring the servers which act as a load balancer and terminate TLS

# Architectural Diagram for problem statement
- SSL Offloading
- SSL offloading with a reverse proxy

### SSL Offloading
![with out proxy](/images/Architecture_Solution_Diagram_without_proxy.png)

### SSL offloading with a reverse proxy
![with proxy](/images/Architecture_Solution_Diagram.png)

# Which Metrics To Monitor ?

* Server Stats
* Proxy Server Stats
* Network Dump
* Process Monitoring Metrics

### Server Stats

 Type | Description
------------ | -------------
CPU Stats    | To monitor CPU metrics (user,system,io)
Disk Stats	 | Get the Metrics from /proc/diskstats
FileSystem Stats | Storage system used
loadavg 	 | Get the Metrics from /proc/loadavg to identify the workload for the last 15min
netstat	 | Active Internet connections (only servers)
meminfo 	 | Memory related metrics of server to identify memory issues
filefd	 | Exposes file descriptor statistics from /proc/sys/fs/file-nr 

### Proxy Server Stats
Type | Description
------------ | -------------
Inbound Traffic | The number of bytes sent from client/endpoints to backend web server through the SSL-Offloading Server
Outbound Traffic	 | Get the Metrics from /proc/diskstats
Open Connections | The number of connections open at the given sample moment.
Closed Connections per second	 | To track TCP Closed Connections
Round trip time	 | RTT measured for each connection between client and SSL-Offloading Server

### Network Dump

> tcpdump
```
23:36:26.013903 IP adj-ubuntu.ssh > _gateway.54987: Flags [P.], seq 1434032:1436816, ack 10401, win 65535, length 2784
23:36:26.014039 IP adj-ubuntu.ssh > _gateway.54987: Flags [P.], seq 1436816:1437072, ack 10401, win 65535, length 256
23:36:26.014157 IP _gateway.54987 > adj-ubuntu.ssh: Flags [.], ack 1435492, win 65535, length 0
23:36:26.014165 IP _gateway.54987 > adj-ubuntu.ssh: Flags [.], ack 1436816, win 65535, length 0
23:36:26.014226 IP adj-ubuntu.ssh > _gateway.54987: Flags [P.], seq 1437072:1437248, ack 10401, win 65535, length 176
23:36:26.014315 IP _gateway.54987 > adj-ubuntu.ssh: Flags [.], ack 1437072, win 65535, length 0
23:36:26.014377 IP adj-ubuntu.ssh > _gateway.54987: Flags [P.], seq 1437248:1437520, ack 10401, win 65535, length 272
23:36:26.014492 IP _gateway.54987 > adj-ubuntu.ssh: Flags [.], ack 1437248, win 65535, length 0
23:36:26.014561 IP adj-ubuntu.ssh > _gateway.54987: Flags [P.], seq 1437520:1437792, ack 10401, win 65535, length 272
23:36:26.014690 IP _gateway.54987 > adj-ubuntu.ssh: Flags [.], ack 1437520, win 65535, length 0
23:36:26.014803 IP _gateway.54987 > adj-ubuntu.ssh: Flags [.], ack 1437792, win 65535, length 0
```
> tcpdump -D
```
root@adj-ubuntu:/proc/sys/fs# tcpdump -D
1.enp0s3 [Up, Running]
2.any (Pseudo-device that captures on all interfaces) [Up, Running]
3.lo [Up, Running, Loopback]
4.docker0 [Up]
5.nflog (Linux netfilter log (NFLOG) interface)
6.nfqueue (Linux netfilter queue (NFQUEUE) interface)
7.usbmon1 (USB bus number 1)
```
### Linux Kernel Dump

### Process Monitoring Metrics
* Prometheus custom exporter (Python Code which could check the aliveness of the process and report it to the promethues server)
* Nagios for the process monitoring.
