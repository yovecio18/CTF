Now that the socks proxy is setup, the routing table is fixed and we have a stable meterpreter shell in our C2 machine we can start by enumerating the whole Internal network and identify the alive host.

I will strat by using metasploit ping sweep module:

```
msf6 post(multi/gather/ping_sweep) > show options 

Module options (post/multi/gather/ping_sweep):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   RHOSTS   172.16.1.0/24    yes       IP Range to perform ping sweep against.
   SESSION  2                yes       The session to run this module on


View the full module info with the info, or info -d command.

msf6 post(multi/gather/ping_sweep) > run

[*] Performing ping sweep for IP range 172.16.1.0/24
[+] 	172.16.1.5 host found
[+] 	172.16.1.17 host found
[+] 	172.16.1.13 host found
[+] 	172.16.1.10 host found
[+] 	172.16.1.12 host found
[+] 	172.16.1.19 host found
[+] 	172.16.1.20 host found
[+] 	172.16.1.100 host found
[+] 	172.16.1.102 host found
[+] 	172.16.1.101 host found
[*] Post module execution completed
```

I will upload a nmap binary on NIX01 as well and double check that values are same so we don't miss any host in between:

```
balthazar@DANTE-WEB-NIX01:/tmp$ ./nmap -sP 172.16.1.0/24

Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2023-08-31 23:53 PDT
Cannot find nmap-payloads. UDP payloads are disabled.
Nmap scan report for 172.16.1.1
Host is up (0.00033s latency).
Nmap scan report for 172.16.1.5
Host is up (0.00048s latency).
Nmap scan report for 172.16.1.10
Host is up (0.00045s latency).
Nmap scan report for 172.16.1.12
Host is up (0.00071s latency).
Nmap scan report for 172.16.1.13
Host is up (0.00034s latency).
Nmap scan report for 172.16.1.17
Host is up (0.00027s latency).
Nmap scan report for 172.16.1.19
Host is up (0.00043s latency).
Nmap scan report for 172.16.1.20
Host is up (0.00032s latency).
Nmap scan report for 172.16.1.100
Host is up (0.00017s latency).
Nmap scan report for 172.16.1.101
Host is up (0.00033s latency).
Nmap scan report for 172.16.1.102
Host is up (0.00076s latency).
Nmap done: 256 IP addresses (11 hosts up) scanned in 28.31 seconds
```

Indeed it matches, now we can use nmap on our machine to target them and identify everything.

For some stupid reason I can' t use the full functionalities of metepreter and pivot via Socks proxy so I have to use other alternatives like chisel or sshuttle.

Since we have a Linux machine we can try to use [SSHUTTLE](https://github.com/sshuttle/sshuttle)  and try to pivot thru the whole 172.16.1.0/24 network and it should create a vpn tunnetl wich means we don't need to use proxychains or similar:

```
┌──(root㉿kali-linux)-[/home/millycash/Downloads/Dante]
└─# sshuttle -r balthazar@10.10.110.100 172.16.1.0/24 -x 10.10.110.100
balthazar@10.10.110.100's password: 
c : Connected to server.
```

In this case we will create a ssh tunnel to the NIX01 by using Balthazar's credentials and will ssh the whole internal 172.16.1.0/24 network. The -x options is used to exclude the address of the server itself and avoid potential error by routing itself via VPN. And if everithyng worked we should be able to just talk to the internal network without using proxy or socks servers... But again for some reason I can't rooute the traffic thru, nevermind I will use other ways in.

Keeping the LOL metality(Living off  the Land) and use the SSH dynaming to reverse forward all the traffic to port 9050 and then use proxychains.

First we setup the dynaminc forward over port 9050:

```
┌──(root㉿kali-linux)-[/home/millycash/Downloads/Dante]
└─# ssh -D 9050 balthazar@10.10.110.100
balthazar@10.10.110.100's password: 
Welcome to Ubuntu 20.04 LTS (GNU/Linux 5.4.0-29-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

636 updates can be installed immediately.
398 of these updates are security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Your Hardware Enablement Stack (HWE) is supported until April 2025.
Last login: Fri Sep  1 00:29:44 2023 from 10.10.14.10
balthazar@DANTE-WEB-NIX01:~$
```

### 172.16.1.5:

```
balthazar@DANTE-WEB-NIX01:/tmp$ ./nmap 172.16.1.5
Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2023-09-01 02:13 PDT
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Stats: 0:00:02 elapsed; 0 hosts completed (0 up), 1 undergoing Ping Scan
Parallel DNS resolution of 1 host. Timing: About 0.00% done
Nmap scan report for 172.16.1.5
Host is up (0.00010s latency).
Not shown: 1175 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp
111/tcp  open  sunrpc
135/tcp  open  epmap
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
1433/tcp open  ms-sql-s
2049/tcp open  nfs
Nmap done: 1 IP address (1 host up) scanned in 14.96 seconds
```

### 172.16.1.10:

```
balthazar@DANTE-WEB-NIX01:/tmp$ ./nmap 172.16.1.10
Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2023-09-01 02:15 PDT
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Nmap scan report for 172.16.1.10
Host is up (0.00021s latency).
Not shown: 1178 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
Nmap done: 1 IP address (1 host up) scanned in 13.06 seconds
```

### 172.16.1.12:

```
balthazar@DANTE-WEB-NIX01:/tmp$ ./nmap 172.16.1.12
Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2023-09-01 02:17 PDT
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Nmap scan report for 172.16.1.12
Host is up (0.00013s latency).
Not shown: 1177 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
80/tcp   open  http
443/tcp  open  https
3306/tcp open  mysql
Nmap done: 1 IP address (1 host up) scanned in 13.05 seconds
```

### 172.16.1.13:

```
balthazar@DANTE-WEB-NIX01:/tmp$ ./nmap 172.16.1.13
Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2023-09-01 02:18 PDT
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Nmap scan report for 172.16.1.13
Host is up (0.00050s latency).
Not shown: 1179 filtered ports
PORT    STATE SERVICE
80/tcp  open  http
443/tcp open  https
445/tcp open  microsoft-ds
Nmap done: 1 IP address (1 host up) scanned in 18.34 seconds
```

### 172.16.1.17:

```
balthazar@DANTE-WEB-NIX01:/tmp$ ./nmap 172.16.1.17
Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2023-09-01 02:19 PDT
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Nmap scan report for 172.16.1.17
Host is up (0.00011s latency).
Not shown: 1178 closed ports
PORT      STATE SERVICE
80/tcp    open  http
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
10000/tcp open  webmin
Nmap done: 1 IP address (1 host up) scanned in 13.05 seconds
```

### 172.16.1.19:

```
balthazar@DANTE-WEB-NIX01:/tmp$ ./nmap 172.16.1.19
Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2023-09-01 02:21 PDT
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Stats: 0:00:12 elapsed; 0 hosts completed (0 up), 1 undergoing Ping Scan
Parallel DNS resolution of 1 host. Timing: About 0.00% done
Nmap scan report for 172.16.1.19
Host is up (0.00010s latency).
Not shown: 1180 closed ports
PORT     STATE SERVICE
80/tcp   open  http
8080/tcp open  http-alt
Nmap done: 1 IP address (1 host up) scanned in 13.06 seconds
```

### 172.16.1.20:

```
balthazar@DANTE-WEB-NIX01:/tmp$ ./nmap 172.16.1.20
Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2023-09-01 02:22 PDT
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Stats: 0:00:09 elapsed; 0 hosts completed (0 up), 1 undergoing Ping Scan
Parallel DNS resolution of 1 host. Timing: About 0.00% done
Stats: 0:00:15 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 30.82% done; ETC: 02:22 (0:00:04 remaining)
Nmap scan report for 172.16.1.20
Host is up (0.00030s latency).
Not shown: 1169 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
53/tcp   open  domain
80/tcp   open  http
88/tcp   open  kerberos
135/tcp  open  epmap
139/tcp  open  netbios-ssn
389/tcp  open  ldap
443/tcp  open  https
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd
593/tcp  open  unknown
636/tcp  open  ldaps
3389/tcp open  ms-wbt-server
Nmap done: 1 IP address (1 host up) scanned in 19.27 seconds
```

### 172.16.1.100:

```
balthazar@DANTE-WEB-NIX01:/tmp$ ./nmap 172.16.1.100
Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2023-09-01 02:23 PDT
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Stats: 0:00:12 elapsed; 0 hosts completed (0 up), 1 undergoing Ping Scan
Parallel DNS resolution of 1 host. Timing: About 0.00% done
Nmap scan report for 172.16.1.100
Host is up (0.000056s latency).
Not shown: 1179 closed ports
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
Nmap done: 1 IP address (1 host up) scanned in 13.05 seconds
balthazar@DANTE-WEB-NIX01:/tmp$
```

### 172.16.1.101:

```
balthazar@DANTE-WEB-NIX01:/tmp$ ./nmap 172.16.1.101
Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2023-09-01 02:24 PDT
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Nmap scan report for 172.16.1.101
Host is up (0.00016s latency).
Not shown: 1178 closed ports
PORT    STATE SERVICE
21/tcp  open  ftp
135/tcp open  epmap
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
Nmap done: 1 IP address (1 host up) scanned in 15.08 seconds
```

### 172.16.1.102:

```
balthazar@DANTE-WEB-NIX01:/tmp$ ./nmap 172.16.1.102
Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2023-09-01 02:24 PDT
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Stats: 0:00:04 elapsed; 0 hosts completed (0 up), 1 undergoing Ping Scan
Parallel DNS resolution of 1 host. Timing: About 0.00% done
Stats: 0:00:06 elapsed; 0 hosts completed (0 up), 1 undergoing Ping Scan
Parallel DNS resolution of 1 host. Timing: About 0.00% done
Stats: 0:00:11 elapsed; 0 hosts completed (0 up), 1 undergoing Ping Scan
Parallel DNS resolution of 1 host. Timing: About 0.00% done
Nmap scan report for 172.16.1.102
Host is up (0.00023s latency).
Not shown: 1175 closed ports
PORT     STATE SERVICE
80/tcp   open  http
135/tcp  open  epmap
139/tcp  open  netbios-ssn
443/tcp  open  https
445/tcp  open  microsoft-ds
3306/tcp open  mysql
3389/tcp open  ms-wbt-server
Nmap done: 1 IP address (1 host up) scanned in 15.07 seconds
```

Now that >

* * *

## Identified Hosts:

| IP  | Name | Informations |
| --- | --- | --- |
| 172.16.1.5 | DANTE-SQL01 | (Windows) TODO |
| 172.16.1.10 | DANTE-NIX02 | (Linux) |
| 172.16.1.12 | DANTE-NIX04 | (Linux) |
| 172.16.1.13 | DANTE-WS01 | (Windows) |
| 172.16.1.17 | DANTE-NIX03 | (Linux) |
| 172.16.1.19 | DANTE-NIX07 | (Linux) |
| 172.16.1.20 | DC01-DANTE | DC (Windows) |
| 172.16.1.100 | DANTE-WEB-NIX01 | Interal NIC on our Pivot machine in DMZ (Linux) |
| 172.16.1.101 | DANTE-WS02 | (Windows) |
| 172.16.1.102 | DANTE-WS03 | (Windows) |
| 172.16.2.5 | DANTE-ADMIN-DC02 | (Windows) |
| 172.16.2.101 | DANTE-ADMIN-NIX05 | (Linux) |

&nbsp;

## Missing Hosts(TO-DO):

|     |     |     |
| --- | --- | --- |
| **IP** | **Name** | **Informations** |
|     | DANTE-FW01 |     |
| ~~172.16.1.19~~ | ~~DANTE-NIX07~~ | ~~(Linux)~~ |
| ~~172.16.2.5~~ | ~~DANTE-ADMIN-DC02~~ | ~~(Windows)~~ |
|     | DANTE-ADMIN-NIX06 |     |
| ~~172.16.2.101~~ | ~~DANTE-ADMIN-NIX05~~ | ~~(Linux)~~ |
| ~~172.16.2.5~~ | ~~DANTE-ADMIN-DC02~~ | ~~(Windows)~~ |