The customer gave us a /24 subnet as our initial entry point without telling us exactly which devices are in scope or detailts about devices and so on which categorize this as a "BlackBox" type of penetrations test.

I will start by enumerating the local network and hunt for alive hosts:

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/RASTALABS]
└─# fping -asgq 10.10.110.0/24    
10.10.110.2

     254 targets
       1 alive
     253 unreachable
       0 unknown addresses

    1012 timeouts (waiting for response)
    1013 ICMP Echos sent
       1 ICMP Echo Replies received
      13 other ICMP received

 38.6 ms (min round trip time)
 38.6 ms (avg round trip time)
 38.6 ms (max round trip time)
        9.784 sec (elapsed real time)
```

fping shows only one host alive running on .2 I will run NMAP and check that results are the same so we don't lose any potential hosts in the meantime.

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/RASTALABS]
└─# nmap -sn 10.10.110.*   
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-20 14:09 CEST
Nmap scan report for 10.10.110.2
Host is up (0.033s latency).
Nmap scan report for 10.10.110.254
Host is up (0.032s latency).
Nmap done: 256 IP addresses (2 hosts up) scanned in 5.56 seconds
```

NMAP shows that also .254 is alive. Lastly I will run a old fashion ping scan to check for all the alive hosts on the /24 subnet answering to a PING.

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/RASTALABS]
└─# for i in {1..254} ;do (ping 10.10.110.$i -c 1 -w 5  >/dev/null && echo "10.10.110.$i" &) ;done
10.10.110.2
```

I guess then we have all what we need so far!

&nbsp;