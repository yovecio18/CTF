## RUSTSCAN:
`PORT   STATE SERVICE REASON          VERSION
21/tcp open  ftp     syn-ack ttl 127 Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 425 Cannot open data connection.
23/tcp open  telnet? syn-ack ttl 127
80/tcp open  http    syn-ack ttl 127 Microsoft IIS httpd 7.5
|_http-server-header: Microsoft-IIS/7.5
|_http-title: MegaCorp
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
`
* * *
## FTP:
So far we can surf the ftp as guest:
![4ca70696e11297e7741afcb46287105b.png](../../_resources/4ca70696e11297e7741afcb46287105b.png)
Tring to download anything, it's failing:
![e7cb44a3a978d6f56601535f06d0cae6.png](../../_resources/e7cb44a3a978d6f56601535f06d0cae6.png)
The solutions is to use binary mode: https://stackoverflow.com/questions/37187986/bare-linefeeds-received-in-ascii-mode-warning-when-listing-directory-on-my-ftp

Now we have a zip file and a Access database?
![03d88b4bf300f416bcaa5d76002712ce.png](../../_resources/03d88b4bf300f416bcaa5d76002712ce.png)

The zip file seems having a pst file that is password protected and an Access db that needs to be mounted..
To make my life easier i mounted the DB in my Windows machine with a Access from O365 suite and exported all users from the USERINFO table.
`John: 020481
Mark: 010101
Sunita: 000000
Mary: 666666
Monica: 123321`

Opening the file in linux shows that i've missed a table that have something more interesting:
![95f7a027bc750ae0003783e68589f841.png](../../_resources/95f7a027bc750ae0003783e68589f841.png)
About all tables we should check auth_user:
`┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# mdb-export backup.mdb auth_user            
id,username,password,Status,last_login,RoleID,Remark
25,"admin","admin",1,"08/23/18 21:11:47",26,
27,"engineer","access4u@security",1,"08/23/18 21:13:36",26,
28,"backup_admin","admin",1,"08/23/18 21:14:02",26,
`

Using the pasword from engineer we can unzip the archive and export the pst...
https://linux.die.net/man/1/readpst

Using the tool we can convert it to a mbox and then parse the text easy with cat:
`┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# cat Access\ Control.mbox 
From "john@megacorp.com" Fri Aug 24 01:44:07 2018
Status: RO
From: john@megacorp.com <john@megacorp.com>
Subject: MegaCorp Access Control System "security" account
To: 'security@accesscontrolsystems.com'
Date: Thu, 23 Aug 2018 23:44:07 +0000
MIME-Version: 1.0
Content-Type: multipart/mixed;
	boundary="--boundary-LibPST-iamunique-696909418_-_-"
----boundary-LibPST-iamunique-696909418_-_-
Content-Type: multipart/alternative;
	boundary="alt---boundary-LibPST-iamunique-696909418_-_-"
--alt---boundary-LibPST-iamunique-696909418_-_-
Content-Type: text/plain; charset="utf-8"
Hi there,
The password for the “security” account has been changed to 4Cc3ssC0ntr0ller.  Please ensure this is passed on to your engineers.
Regards,
John
`

What can we do with this password now?

* * *
## TELNET:
So far seems nothing interesting here...

Now we have a username and password from secuirty account from pst we can try to login to system with telnet:
![08e203d8eba9dd78587156d8102b0b30.png](../../_resources/08e203d8eba9dd78587156d8102b0b30.png)

Runa and grab last flag:
![83d7f4c80d3ff7336e83dca7e88b03f2.png](../../_resources/83d7f4c80d3ff7336e83dca7e88b03f2.png)
* * *
## HTTP:
So far nothing interesting came our from a subdomain enumeration:
![c6966cd750b636b34bf02c09d340d4b1.png](../../_resources/c6966cd750b636b34bf02c09d340d4b1.png)

We found some folders but no files?
![3e2894cf4b69664ee67eb1cb82371206.png](../../_resources/3e2894cf4b69664ee67eb1cb82371206.png)

Can we see if IIS is vulnerable to shortnames bug?
![fdfdd2242d58c745092695cac14e7605.png](../../_resources/fdfdd2242d58c745092695cac14e7605.png)

* * *
## PRIVESC:
Now checking manually for permissions with whoami/all shows thta we dont't have any particular permissions nor are part of any special group since there aren't any AD environment.

Surfing for files i found something interesting in the root directory:
![cd7e69bc224211d990f337be738d0d10.png](../../_resources/cd7e69bc224211d990f337be738d0d10.png)

Checking about the software seems like a exploit is available:
https://www.exploit-db.com/exploits/40323

Checking the .exe configuration i find another db:
![cc7d852d3d7935f9b3cc4c99f6227916.png](../../_resources/cc7d852d3d7935f9b3cc4c99f6227916.png)
Which seems to be a SQLlite:
![e8f78d98ed84b78a7b004bce0974b910.png](../../_resources/e8f78d98ed84b78a7b004bce0974b910.png)


Edit: I was totally wrong the solution was to check into Public folder:
![0846854ad9fe99b952a1e75198548c31.png](../../_resources/0846854ad9fe99b952a1e75198548c31.png)

Now that we can see that the link is using some saved password:
![7c934a542fb0504f162e8459ee3f4791.png](../../_resources/7c934a542fb0504f162e8459ee3f4791.png)

We can use this guide om privescalation: https://steflan-security.com/windows-privilege-escalation-runas-stored-credentials/

Checking for saved credentials, and bingo!:
![2a4571a80690f89b2afd316cce599931.png](../../_resources/2a4571a80690f89b2afd316cce599931.png)

Now we should upload a nc.exe copy and we can run this part with saved credentials:
![d8dc22b0706b21ad14faa81cde5473c6.png](../../_resources/d8dc22b0706b21ad14faa81cde5473c6.png)

Now running nc.exe to forward traffic to our listener as Administrator with saved creds:
![b63012be71864453d6d0102df6e583cd.png](../../_resources/b63012be71864453d6d0102df6e583cd.png)

Get's us a shell:
![6e44873b47a1861ccdb8b32c119cb4a6.png](../../_resources/6e44873b47a1861ccdb8b32c119cb4a6.png)

Grab last flag!
* * *