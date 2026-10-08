## Rustscan:

```Bash
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ+m7rYl1vRtnm789pH3IRhxI4CNCANVj+N5kovboNzcw9vHsBwvPX3KYA3cxGbKiA0VqbKRpOHnpsMuHEXEVJc=
|   256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOtuEdoYxTohG80Bo6YCqSzUY9+qbnAFnhsk4yAZNqhM
80/tcp open  http    syn-ack ttl 63 nginx
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://2million.htb/
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 4.15 - 5.8 (96%), Linux 5.3 - 5.4 (95%), Linux 5.0 - 5.5 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 (93%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94%E=4%D=6/7%OT=22%CT=%CU=39451%PV=Y%DS=2%DC=T%G=N%TM=648088BA%P=x86_64-pc-linux-gnu)
SEQ(SP=105%GCD=1%ISR=108%TI=Z%CI=Z%II=I%TS=A)
SEQ(SP=105%GCD=1%ISR=109%TI=Z%CI=Z%II=I%TS=A)
OPS(O1=M550ST11NW7%O2=M550ST11NW7%O3=M550NNT11NW7%O4=M550ST11NW7%O5=M550ST11NW7%O6=M550ST11)
WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
ECN(R=Y%DF=Y%T=40%W=FAF0%O=M550NNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)
IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 19.517 days (since Fri May 19 03:15:32 2023)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 443/tcp)
HOP RTT      ADDRESS
1   33.44 ms 10.10.14.1
2   33.55 ms 10.10.11.221
```

* * *

## SSH:

As usual ssh is pretty new and no known entry points are available which means no party for now! We won't use bruteforce as attackvector do not create any disruptions in daily operationality.

We can come back as soon we find proper credentials!

* * *

## HTTP:

From our first Nmap scan we could see that website is pointing to FQDN **http://2million.htb/**

I will add that to our domain file and then I will move forward with a subdomain and web directory enumeration:

```Bash
┌──(root㉿kali)-[/home/aleksandar/Downloads]
└─# ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u http://2million.htb -H "Host:FUZZ.2million.htb" -fl 8

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.0.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://2million.htb
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.2million.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500
 :: Filter           : Response lines: 8
________________________________________________

:: Progress: [19966/19966] :: Job [1/1] :: 729 req/sec :: Duration: [0:00:27] :: Errors: 0 ::
```

And here all web directories:

```Bash
┌──(root㉿kali)-[/home/aleksandar/Downloads]
└─# dirsearch -u http://2million.htb 

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11712

Output: /home/aleksandar/Downloads/reports/http_2million.htb/_23-06-07_15-47-18.txt

Target: http://2million.htb/

[15:47:18] Starting: 
[15:47:19] 301 -  162B  - /js  ->  http://2million.htb/js/
[15:47:30] 200 -   87B  - /.env
[15:48:18] 200 -    2KB - /404
[15:50:32] 401 -    0B  - /api
[15:50:33] 401 -    0B  - /api/v1
[15:51:12] 301 -  162B  - /assets  ->  http://2million.htb/assets/
[15:51:12] 403 -  548B  - /assets/
[15:52:00] 403 -  548B  - /controllers/
[15:52:06] 301 -  162B  - /css  ->  http://2million.htb/css/
[15:52:49] 301 -  162B  - /fonts  ->  http://2million.htb/fonts/
[15:53:27] 302 -    0B  - /home  ->  /
[15:53:33] 301 -  162B  - /images  ->  http://2million.htb/images/
[15:53:33] 403 -  548B  - /images/
[15:53:46] 403 -  548B  - /js/
[15:53:55] 200 -    4KB - /login
[15:53:58] 302 -    0B  - /logout  ->  /
[15:56:10] 200 -    4KB - /register
[15:58:10] 301 -  162B  - /views  ->  http://2million.htb/views/

Task Completed
```

Here can we see some juiy stuff, we can see a login and register page, a api and lastly some kind of configuration.

```Bash
┌──(root㉿kali)-[/home/aleksandar/Downloads]
└─# curl http://2million.htb/.env
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123
```