As usual I will start by enumerating all the open services over the TCP protocoll stack, and Immediately I don't see anything out of order.

```bash
PORT      STATE SERVICE       REASON          VERSION
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  syn-ack ttl 127 Windows 10 Enterprise 10240 microsoft-ds (workgroup: SIDECAR)
3389/tcp  open  ms-wbt-server syn-ack ttl 127 Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: SIDECAR
|   NetBIOS_Domain_Name: SIDECAR
|   NetBIOS_Computer_Name: WS01
|   DNS_Domain_Name: Sidecar.vl
|   DNS_Computer_Name: ws01.Sidecar.vl
|   DNS_Tree_Name: Sidecar.vl
|   Product_Version: 10.0.10240
|_  System_Time: 2026-03-18T09:28:37+00:00
|_ssl-date: 2026-03-18T09:28:46+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=ws01.Sidecar.vl
| Issuer: commonName=ws01.Sidecar.vl
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2026-03-17T03:22:33
| Not valid after:  2026-09-16T03:22:33
| MD5:     6c97 99fb 244c 92a7 f3eb fb5a 7c95 eaf5
| SHA-1:   132d 4ad1 0471 924e b652 cd64 df65 fbed fe76 01cb
| SHA-256: 92b2 1171 04f7 f7b7 0b62 3dd3 3dfb 5d2a 26e9 072e 24f4 49f4 f3b6 ea80 ae9c 09c4
| -----BEGIN CERTIFICATE-----
| MIIC4jCCAcqgAwIBAgIQZOgpCRnikLVG2U0bvy0yzTANBgkqhkiG9w0BAQUFADAa
| MRgwFgYDVQQDEw93czAxLlNpZGVjYXIudmwwHhcNMjYwMzE3MDMyMjMzWhcNMjYw
| OTE2MDMyMjMzWjAaMRgwFgYDVQQDEw93czAxLlNpZGVjYXIudmwwggEiMA0GCSqG
| SIb3DQEBAQUAA4IBDwAwggEKAoIBAQCs67hv7pxcKOesa+SuCbFKIpApdBp4PKdE
| sNKFh73X3UtKMW5LV9w+eKIqueCzfcYT9RQzXZG2htV3esAkvl+xjzbr3AYtgBry
| oktNe//RJdYVYkro6uAA3Ux5gbM9alrIGenq/OCjZ+u9QV4DBLXh7EVgUZZogiha
| uYX14+7nq3veGGK36wq2ugfLsCVQIRyudzCay/UUJcSK2C13KL/AW06MxQHpAeHo
| lp2hDrRK0xO+Y0w/+oh3tFhuBaXcZHAhedgdXSCCrH9W4cIEXojWwRAACTHulBPd
| AB8ZWjSzTORy6b/o7uglnUUNZ+hqHL3hxSY4tkdqpAFbk431/bPPAgMBAAGjJDAi
| MBMGA1UdJQQMMAoGCCsGAQUFBwMBMAsGA1UdDwQEAwIEMDANBgkqhkiG9w0BAQUF
| AAOCAQEAOXgb2bjrG1em76h1k58ITdnv/4dK87MUzOR6Wv2aodV6cQjuysXvXWdu
| srLwQ77p7QIyleRoLdeSN0U7EkTVstyo3HGQrkfxH9qGikAG9PaVMjkBlmfz6/3a
| rbNtuZVLcdZ93bnlBp/jpD+DPKpktpzW0yQKYSHr6PEKKywl1fLPC9Uoam2lGjSe
| HhbipVl35e1HPCIBb1K8fN8LL6cTubIAmtu8rKMlGvDSxv0WAvXZPz5je8NoZkw8
| 5bjVoxn3G/ADlVLbIZp9CE81yXubywY+IuqzZPEZONr2mxvOnkx2mKyoSQ3yF5k0
| 1fWHXljTAjqlUmZs2uXaCn0W/Ro0Pw==
|_-----END CERTIFICATE-----
49408/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49409/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49410/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49411/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49412/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49413/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49414/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49415/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Microsoft Windows Server 2016 (96%), Microsoft Windows 10 1511 (95%), Microsoft Windows 10 1607 (95%), Microsoft Windows 7 SP1 or Windows Server 2008 R2 or Windows 8.1 (94%), Microsoft Windows 7 or Windows Server 2008 R2 or Windows 8.1 (94%), Microsoft Windows Server 2012 R2 (93%), Microsoft Windows 10 (93%), Microsoft Windows 7 SP1 (92%), Microsoft Windows 7 SP1 or Windows Server 2008 SP2 (92%), Microsoft Windows Windows 7 SP1 (92%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.98%E=4%D=3/18%OT=135%CT=%CU=41373%PV=Y%DS=2%DC=T%G=N%TM=69BA704E%P=x86_64-pc-linux-gnu)
SEQ(SP=101%GCD=1%ISR=109%TI=I%CI=I%II=I%SS=S%TS=A)
SEQ(SP=105%GCD=1%ISR=10B%TI=I%CI=I%II=I%SS=S%TS=A)
OPS(O1=M552NW8ST11%O2=M552NW8ST11%O3=M552NW8NNT11%O4=M552NW8ST11%O5=M552NW8ST11%O6=M552ST11)
WIN(W1=2000%W2=2000%W3=2000%W4=2000%W5=2000%W6=2000)
ECN(R=Y%DF=Y%T=80%W=2000%O=M552NW8NNS%CC=N%Q=)
T1(R=Y%DF=Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)
U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)
IE(R=Y%DFI=N%T=80%CD=Z)

```

To be on the safe side I also performed a quick scan over the common UDP ports without showing traces of uncommon services running on the machine.

```bash
└─$ nmap -F -sU 10.13.38.48
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-18 10:29 +0100
Nmap scan report for 10.13.38.48
Host is up (0.025s latency).
Not shown: 93 closed udp ports (port-unreach)
PORT     STATE         SERVICE
123/udp  open|filtered ntp
137/udp  open|filtered netbios-ns
138/udp  open|filtered netbios-dgm
500/udp  open|filtered isakmp
1900/udp open|filtered upnp
4500/udp open|filtered nat-t-ike
5353/udp open|filtered zeroconf

Nmap done: 1 IP address (1 host up) scanned in 172.58 seconds

```