I will start by engaging FPING and check all the host alive on the network, and I see only 2:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/APTLabs]
└─# fping -asgq 10.10.110.0/24 
10.10.110.2
10.10.110.123

     254 targets
       2 alive
     252 unreachable
       0 unknown addresses

    1008 timeouts (waiting for response)
    1010 ICMP Echos sent
       2 ICMP Echo Replies received
      11 other ICMP received

 26.5 ms (min round trip time)
 31.4 ms (avg round trip time)
 36.2 ms (max round trip time)
        9.839 sec (elapsed real time)
```

To be sure I am not missing anything I will use NMAP to compare the results:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/APTLabs]
└─# nmap -sn 10.10.110.0/24
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-30 13:00 CEST
Nmap scan report for 10.10.110.2
Host is up (0.043s latency).
Nmap scan report for 10.10.110.3
Host is up (0.026s latency).
Nmap scan report for 10.10.110.123
Host is up (0.027s latency).
Nmap done: 256 IP addresses (3 hosts up) scanned in 5.29 seconds
```

As you see NMAP shows that 3 are the hosts alive, sometimes is good to not rely on only one tool, because different tools that poses same purpose can sometimes give different results.

Before wrapping up I will do same task but directly from BASH:

![ca05e89aee2c1187192ea9a1b02af366.png](../../../_resources/ca05e89aee2c1187192ea9a1b02af366.png)

Bash seems very slow, so I will trust what I have found out so far and start by enumerating the singular IPs in the specific pages following next.

&nbsp;

&nbsp;