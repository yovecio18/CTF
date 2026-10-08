We can start by sweeping the whole \*\*10.10.110.0/24 \*\*subnet to find exactly which hosts are alive in the DMZ network, we must remember to remove the .2 which is the Firewall and it's out of scope.

Let's start by enumerating the whole /24 subnet and find which devices are alive:

```
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# fping -asgq 10.10.110.0/24
10.10.110.2
10.10.110.100

     254 targets
       2 alive
     252 unreachable
       0 unknown addresses

    1008 timeouts (waiting for response)
    1010 ICMP Echos sent
       2 ICMP Echo Replies received
      11 other ICMP received

 39.9 ms (min round trip time)
 40.2 ms (avg round trip time)
 40.6 ms (max round trip time)
        9.226 sec (elapsed real time)
```

Ok only 2 devices seems oline where one is the firewall and shouldn't be counted in. I will use NMAP as well to check for discrepancies:

```
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# nmap -sP 10.10.110.0/24              
Starting Nmap 7.94 ( https://nmap.org ) at 2023-08-31 18:30 CEST
Nmap scan report for 10.10.110.2
Host is up (0.054s latency).
Nmap scan report for 10.10.110.100
Host is up (0.041s latency).
Nmap done: 256 IP addresses (2 hosts up) scanned in 5.89 seconds
```

We get the same results which means we can move forward with the enumeration.

* * *

## Identified hosts

| IP  | Name | Informations |
| --- | --- | --- |
| 10.10.110.2 | Firewall | Firewall used by ISP, out of scope! |
| 10.10.110.100 | DANTE-WEB-NIX01 | DMZ Wordpress |