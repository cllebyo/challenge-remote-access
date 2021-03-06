# Challenge Solution

## My environment:


I developed a small virtualbox architecture where it was created three virtual machines in two differents network:



**1) Client-A** 

This machine is running with a CentOS 7.6 inside the network with only one Nic Card. 


IP: _192.168.1.2_

Netmaks: _255.255.255.0_

**2) Server-A**

This machine is also running with a CentOS 7.6 with two Nic Cards, enp0s3 and enp0s8, spread in two distinct networks. 
The only opens port in this server are 22 (ssh) and 8000 (nginx).

enp0s3

-**IP:** _192.168.1.240_

-**Netmask:** _255.255.255.0_


enp0s8

-**IP:** _172.16.1.1_

-**Netmask:** _255.255.0.0_

**3) Server-B**

This machine is also running with a CentOs 7.6, but the communication occurs only the Server-A. The only opens port in 
this server are 22 (ssh) and 8000 (apache).

**IP:** _172.16.1.2_

**Netmaks:** _255.255.0.0_


## Solutions:

I tried to solve the problem in a simple, easy and without major complications. Then, for this effort, 
the following steps were performed:

- Creating a static route on the Client-A to the net 172.16.0.0:
  > route add -net 172.16.0.0 netmask 255.255.0.0 gw 192.168.1.240
  
- Enabling packet forwarding on Server-A:
  > echo 1 > /proc/sys/net/ipv4/ip_forward
  > vim /etc/sysctl.conf (line net.ipv4.ip_forward = 1 added)
  
- Iptables rule on Server-A
  > iptables -I INPUT -p tcp -d 172.16.1.2 --dport 8000 -j ACCEPT
  
With theses stepes above, the client-A could access the applications on Server-A (curl http://192.168.1.240:8000) and 
Server-B (http://172.16.1.2:8000)
