# Intro

Ready for the next level? This challenging fortress from Synacktiv showcases some very interesting and unique vectors. Learn infrastructure hacking, web and appsec exploitation techniques, prove your skills, and add some l337 techniques to your armoury.

Entry point:

![efb645ac04e844008df442bd525cf5aa.png](../../../_resources/efb645ac04e844008df442bd525cf5aa.png)

# Initial Enumeration

Now in this case I have no idea of how many  machines there will be or what OSses I will have to pwn rather than the machine I have so far.

Now as usual I will start by checking presence of interesting UDP services via a NMAP scan:

```bash
└─$ nmap -sU -F 10.13.37.13
Starting Nmap 7.98 ( https://nmap.org ) at 2026-01-17 11:32 +0100
Stats: 0:00:32 elapsed; 0 hosts completed (1 up), 1 undergoing UDP Scan
UDP Scan Timing: About 92.17% done; ETC: 11:33 (0:00:03 remaining)
Nmap scan report for 10.13.37.13
Host is up (0.039s latency).
All 100 scanned ports on 10.13.37.13 are in ignored states.
Not shown: 60 closed udp ports (port-unreach), 40 open|filtered udp ports (no-response)

Nmap done: 1 IP address (1 host up) scanned in 57.09 seconds

```

Ok, nothing so far... So now I will do same but for the TCP services as well:

```bash
└─$ nmap -p 80 -A -T4 10.13.37.13
Starting Nmap 7.98 ( https://nmap.org ) at 2026-01-17 11:39 +0100
Nmap scan report for 10.13.37.13
Host is up (0.033s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: Hackfail.htb
|_http-server-header: Apache/2.4.38 (Debian)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|router
Running: Linux 4.X|5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 4.15 - 5.19, Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
Network Distance: 2 hops

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   30.73 ms 10.10.14.1
2   36.63 ms 10.13.37.13

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.42 seconds

```

Now since I already attempted one time in the past I remember seeing only one service open and running so surfing to the site I can see that it points to the specific FQDN "hackfail.htb".

![6c9d6e4f528b3c2efacf00a3e5ec69ab.png](../../../_resources/6c9d6e4f528b3c2efacf00a3e5ec69ab.png)

And here I will move on to the following chapter.