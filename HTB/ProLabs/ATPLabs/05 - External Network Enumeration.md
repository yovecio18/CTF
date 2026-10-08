Without further do I will start nby running FPING to check all the hosts that are alive on the given entry point network: **10.10.110.0/24**.

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/APTLabs]
└─# fping -asgq 10.10.110.0/24 
10.10.110.2
10.10.110.13
10.10.110.21
10.10.110.50
10.10.110.62
10.10.110.74
10.10.110.88
10.10.110.231
10.10.110.242

     254 targets
       9 alive
     245 unreachable
       0 unknown addresses

     980 timeouts (waiting for response)
     989 ICMP Echos sent
       9 ICMP Echo Replies received
      11 other ICMP received

 27.5 ms (min round trip time)
 28.1 ms (avg round trip time)
 28.8 ms (max round trip time)
        9.596 sec (elapsed real time)
```

9 targets alive so far, nice! I will execute the same command but this time with NMAP just to be sure that I am not missing anything.. Might sound silly but some times different tools that serves same purpose might give different results.

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/APTLabs]
└─# nmap 10.10.110.0/24 -n -sP
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-29 16:29 CEST
Nmap scan report for 10.10.110.2
Host is up (0.047s latency).
Nmap scan report for 10.10.110.13
Host is up (0.028s latency).
Nmap scan report for 10.10.110.21
Host is up (0.034s latency).
Nmap scan report for 10.10.110.50
Host is up (0.031s latency).
Nmap scan report for 10.10.110.62
Host is up (0.030s latency).
Nmap scan report for 10.10.110.74
Host is up (0.030s latency).
Nmap scan report for 10.10.110.88
Host is up (0.031s latency).
Nmap scan report for 10.10.110.231
Host is up (0.028s latency).
Nmap scan report for 10.10.110.242
Host is up (0.030s latency).
Nmap done: 256 IP addresses (9 hosts up) scanned in 3.64 seconds
```

Same same, I will move on with manual enumeration of the singular hosts.