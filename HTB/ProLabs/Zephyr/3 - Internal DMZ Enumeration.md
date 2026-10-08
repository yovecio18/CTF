We got a entire /24 subnet as entry point and our main goal is to start by finding all the alive hosts:

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# fping -asgq 10.10.110.100/24
10.10.110.2

     254 targets
       1 alive
     253 unreachable
       0 unknown addresses                                                                                                                                                                   
                                                                                                                                                                                             
    1012 timeouts (waiting for response)
    1013 ICMP Echos sent
       1 ICMP Echo Replies received
      11 other ICMP received

 65.4 ms (min round trip time)
 65.4 ms (avg round trip time)
 65.4 ms (max round trip time)
        9.772 sec (elapsed real time)
```

Only one device is alive, and I guess it's the Router:

![98878f44a01d711d1ccfbb929b62a720.png](../../../_resources/98878f44a01d711d1ccfbb929b62a720.png)

I will run NMAP and check that results matches so we don´t lose important stuff from the beginning! Here Instead we got some discrepancies, we have another device that we missed on first scan:

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# nmap -sn 10.10.110.0/24
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-08 10:28 CEST
Stats: 0:00:01 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 2.54% done; ETC: 10:30 (0:01:17 remaining)
Stats: 0:00:03 elapsed; 0 hosts completed (0 up), 256 undergoing Ping Scan
Ping Scan Timing: About 17.53% done; ETC: 10:29 (0:00:14 remaining)
Nmap scan report for 10.10.110.2
Host is up (0.059s latency).
Nmap scan report for 10.10.110.35
Host is up (0.041s latency).
Nmap done: 256 IP addresses (2 hosts up) scanned in 11.66 seconds

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# nmap -sP 10.10.110.0/24
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-08 10:29 CEST
Nmap scan report for 10.10.110.2
Host is up (0.063s latency).
Nmap scan report for 10.10.110.35
Host is up (0.039s latency).
Nmap done: 256 IP addresses (2 hosts up) scanned in 11.40 seconds
```

As you see it´s important to use different tools for archive same result as sometimes it might give different results even if the purpose is the same.

&nbsp;

&nbsp;