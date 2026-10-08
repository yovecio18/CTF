From the challenge we know that there should be a whole /24 subnet to enumerate so i will start by using fpning:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Cybernetics]
└─# fping -asgq 10.10.110.0/24

10.10.110.2
10.10.110.250

     254 targets
       2 alive
     252 unreachable
       0 unknown addresses

    1008 timeouts (waiting for response)
    1010 ICMP Echos sent
       2 ICMP Echo Replies received
      11 other ICMP received

 27.3 ms (min round trip time)
 29.7 ms (avg round trip time)
 32.1 ms (max round trip time)
        9.665 sec (elapsed real time)
```

Seems like only 2 IPs? But if we run NMAP we will get much more hosts?

```
──(root㉿kali-bello)-[/home/millycash/Downloads/Cybernetics]
└─# nmap -sn 10.10.110.0/24
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-10 18:59 CEST
Nmap scan report for 10.10.110.2
Host is up (0.042s latency).
Nmap scan report for 10.10.110.10
Host is up (0.026s latency).
Nmap scan report for 10.10.110.11
Host is up (0.028s latency).
Nmap scan report for 10.10.110.12
Host is up (0.031s latency).
Nmap scan report for 10.10.110.15
Host is up (0.031s latency).
Nmap scan report for 10.10.110.250
Host is up (0.028s latency).
Nmap done: 256 IP addresses (6 hosts up) scanned in 4.60 seconds
```

I feel confident that this is a pretty legit picture about the possible machines on this that might be called as the "DMZ"?

Anyway I will move to the singular hosts and check all the juicy stuff that I can find along.

&nbsp;