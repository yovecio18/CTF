# Intro

This Fortress, created by Faraday, was designed not only as a puzzle, but mainly as a tool to learn: a server’s alert system has been hacked, your task is to use your skills to find out exactly how they did it, and to take advantage of this knowledge in order to hack the system yourself. The idea behind the Fortress is that security is not only about knowing, it’s about being able to learn what you need, when you need to. To be a hacker means not only a set of skills, but also an attitude towards learning. To conquer the Fortress, participants will need to exercise the following abilities: Web Exploitation Lateral thinking Networking You’ll also be able to get an introduction to reverse engineering and binary exploitation, crucial skills to face complex problems. “Hack the box has been a gateway for learning in new, unconventional ways, in line with the principles of the hacker community. Our fortress was designed to do exactly that: practice learning from another hacker’s activity in a challenging environment”. Says Javier Aguinaga, Security Research Lead at Faraday. “Faraday conceives cyber security as an integrated ecosystem, and it’s main goal is to improve the security environment for all the community. Collaborating with the formation of new hackers is part of our mission, and Hack the Box is the perfect ally”. Confronting this fortress will be a great opportunity to become a better hacker. Conquering it will allow you to stand out.

# Entry point

As usual i get only one machine as entry point so far:

![a1c05e2d1b406a58f3b574872d3360d1.png](../../../_resources/a1c05e2d1b406a58f3b574872d3360d1.png)

Now I will perform a initial scan of the available UDP services:

```bash
└─$ nmap -F -sU 10.13.37.14
Starting Nmap 7.98 ( https://nmap.org ) at 2026-01-20 13:11 +0100
Stats: 0:00:48 elapsed; 0 hosts completed (1 up), 1 undergoing UDP Scan
UDP Scan Timing: About 57.40% done; ETC: 13:12 (0:00:35 remaining)
Nmap scan report for 10.13.37.14
Host is up (0.11s latency).
All 100 scanned ports on 10.13.37.14 are in ignored states.
Not shown: 100 closed udp ports (port-unreach)

Nmap done: 1 IP address (1 host up) scanned in 101.84 seconds
                                                                
```

So far nothing interesting, but what about the TCP services instead?

```bash
PORT     STATE SERVICE         REASON         VERSION
22/tcp   open  ssh             syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 a8:05:53:ae:b1:8d:7e:90:f1:ea:81:6b:18:f6:5a:68 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCn+OBVZ8aW1WpHnk2y+RBJfAAjWWc8wtiMWv4EF/TnNALqXgClmpRGHQYyJy7q43WCzhAfsYJorggsyTRW6HNnXnB4U+PZhmn90wl5DX4GJUWQH3S1PH0x0hMQ+8bDt/qe9Anw2ZB6pSEJX0istdOnihiVoZIlwpfHESxm8oXC05hgvL1BIFHTauntV+YnnqgkJ0rJU5r9qoPMdUyUSf+x7Ao+GVW0KOqhWjJnfV4gDgBJrQyMYmida7O8iOam9tdgMee4JqPyPH/RTiCBr+vVmfAXxK3GRxMBrOxoqyouWTEXQsmQlkvubmBFSzdwkIg7bhVkDmRLBB1VIprSkuWDanQ/6TRg3EDJ1PN+9uiZLsDVYP0399c7gNZEBheeMSaXL1NsH/VIGJL2eQsOVtWYN5yHw15KzCQ2TaXYw07A5ThbXAukGO+xwCfpLHzJPSwtzxCaW4RpQ7yYUk3tCgBcGQmO1i9gDT8KFogiWAEjDRQI9lFKl8SYkQ90NwYXvK8=
|   256 2e:7f:96:ec:c9:35:df:0a:cb:63:73:26:7c:15:9d:f5 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFj0dK19uhVUDpnZEaNhITtRBIBZU46rEw4cRP6yp6A8xBYRGKVa1HSf9C96sXPqw86J/nphdkt1ZTxrrhvOuZ0=
|   256 2f:ab:d4:f5:48:45:10:d2:3c:4e:55:ce:82:9e:22:3a (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGgy3Ea/mMDIcLku2KNKVYbbvYJVIhvhoL3rRXoxdii6
80/tcp   open  http            syn-ack ttl 62 nginx 1.13.12
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
| http-title: Notifications
|_Requested resource was http://10.13.37.14/login?next=%2F
|_http-server-header: nginx/1.13.12
| http-git: 
|   10.13.37.14:80/.git/
|     Git repository found!
|     .git/config matched patterns 'user'
|     Repository description: Unnamed repository; edit this file 'description' to name the...
|_    Last commit message: Add app logic & requirements.txt 
8888/tcp open  sun-answerbook? syn-ack ttl 63
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, JavaRMI, Kerberos, LDAPBindReq, LDAPSearchReq, LPDString, LSCP, RPCCheck, RTSPRequest, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServerCookie, X11Probe: 
|     Welcome to FaradaySEC stats!!!
|     Username: Bad chars detected!
|   NULL: 
|     Welcome to FaradaySEC stats!!!
|_    Username:
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8888-TCP:V=7.98%I=7%D=1/20%Time=696F7139%P=x86_64-pc-linux-gnu%r(NU
SF:LL,29,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20")%r(GetRe
SF:quest,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20
SF:chars\x20detected!")%r(HTTPOptions,3C,"Welcome\x20to\x20FaradaySEC\x20s
SF:tats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(FourOhFourRequest,3
SF:C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x
SF:20detected!")%r(JavaRMI,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUs
SF:ername:\x20Bad\x20chars\x20detected!")%r(LSCP,3C,"Welcome\x20to\x20Fara
SF:daySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(GenericL
SF:ines,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20c
SF:hars\x20detected!")%r(RTSPRequest,3C,"Welcome\x20to\x20FaradaySEC\x20st
SF:ats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(RPCCheck,3C,"Welcome
SF:\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20detected
SF:!")%r(DNSVersionBindReqTCP,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\
SF:nUsername:\x20Bad\x20chars\x20detected!")%r(DNSStatusRequestTCP,3C,"Wel
SF:come\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20dete
SF:cted!")%r(Help,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x
SF:20Bad\x20chars\x20detected!")%r(SSLSessionReq,3C,"Welcome\x20to\x20Fara
SF:daySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(Terminal
SF:ServerCookie,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20
SF:Bad\x20chars\x20detected!")%r(TLSSessionReq,3C,"Welcome\x20to\x20Farada
SF:ySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(Kerberos,3
SF:C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x
SF:20detected!")%r(SMBProgNeg,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\
SF:nUsername:\x20Bad\x20chars\x20detected!")%r(X11Probe,3C,"Welcome\x20to\
SF:x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(L
SF:PDString,3C,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\
SF:x20chars\x20detected!")%r(LDAPSearchReq,3C,"Welcome\x20to\x20FaradaySEC
SF:\x20stats!!!\nUsername:\x20Bad\x20chars\x20detected!")%r(LDAPBindReq,3C
SF:,"Welcome\x20to\x20FaradaySEC\x20stats!!!\nUsername:\x20Bad\x20chars\x2
SF:0detected!");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
TCP/IP fingerprint:
OS:SCAN(V=7.98%E=4%D=1/20%OT=22%CT=%CU=36219%PV=Y%DS=2%DC=T%G=N%TM=696F7148
OS:%P=x86_64-pc-linux-gnu)SEQ(SP=FE%GCD=1%ISR=10E%TI=Z%CI=Z%II=I%TS=A)OPS(O
OS:1=M552ST11NW7%O2=M552ST11NW7%O3=M552NNT11NW7%O4=M552ST11NW7%O5=M552ST11N
OS:W7%O6=M552ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)ECN(R
OS:=Y%DF=Y%T=40%W=FAF0%O=M552NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%
OS:RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y
OS:%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R
OS:%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=
OS:40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S
OS:)

Uptime guess: 23.657 days (since Sat Dec 27 21:26:42 2025)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=254 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   104.87 ms 10.10.14.1
2   105.18 ms 10.13.37.14

```

I will move on to the next page.