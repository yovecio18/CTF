## RUSTSCAN:
`PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey:
|   3072 db1d5c65729bc64330a52ba0f01ad5fc (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDMui8XsKyVnrUBcuXeZU88nULgmdJ08nPvDUTXgwL2A1dtQy2YKhqzg4HVQkI8nceWJbCJ/3wKd5PiVeA8L8uBU3DhRpjIMfK3A08aXPtSXpN/lM5GlZztC1AroPGfB8tDce158l5p8vNYkv6my2qxa8CBhiLjO5F2HBVwWY1jHZBPdkigzYKzscvqpbHBk/T4dG64OCEmm79DH01Hq8SA95xLln1xxwuPhQ68s+exCSTB/f/taLnzHoT2qh5wAoWqF912JUMKn1Ojvv5SpJDFNUNBAFgaLHf20GQpO8UxYqw/ZFZfHhKPf7Rz3bhhoKv/ZL0xYN4MleEFxFpej05oADTpHfrABzbkX2C0w6KmzNZIsZaxx1kO9DIDeQprRErTdXKXZD6Ym9CZ1cAPbilwMS945UvZigCLHhFri0iLhoYpdEFBX4kKraqTvxncUQNHibA1Y3rnavpB9XVd/Pdkd5PevNy2UEK253S+Mx/dr4VWB94xwx7C3QCjAQXE5V8=
|   256 4f7956c5bf20f9f14b9238edcefaac78 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBElwFQd0JPcl/MeO0FRD3rz9Fic4TamcO+q2eUjp2HIDCf6HEE+saKGVUmnue904NvlnyyhJJCAZ3MV3yEJnhds=
|   256 df47554f4ad178a89dcdf8a02fc0fca9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDnd0IxAYF7SPECTCC3VhgzZJa4ZUpSQ/6DYR6fXIXRz
80/tcp   open  http    syn-ack ttl 63 Apache httpd 2.4.54 ((Debian))
|_http-trane-info: Problem with XML parsing of /evox/about
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: C797F0B9A0242854B3C20DEC6614399C
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-server-header: Apache/2.4.54 (Debian)
|_http-title: Home
6379/tcp open  redis   syn-ack ttl 63 Redis key-value store
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete`
* * *
## REDIS:
Seems like auth is needed:
`┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# redis-cli -h pollution
pollution:6379> info
NOAUTH Authentication required.`
* * *
## HTTP:
Possible subdomains:
`forum                   [Status: 200, Size: 14098, Words: 910, Lines: 337, Duration: 98ms]
developers              [Status: 401, Size: 469, Words: 42, Lines: 15, Duration: 94ms]`

Checking website mainpage emerges that the real FQDN is:
![954647d4e3b81e671fa1e18658b67c1e.png](../../_resources/954647d4e3b81e671fa1e18658b67c1e.png)
* * *
## Collect.htb:
So far nothing interesting from source code. We can signup with a dummy user:
![8b557efd1c239e25df5f98f0b7939f97.png](../../_resources/8b557efd1c239e25df5f98f0b7939f97.png)

And then loggin into website we can see something about a API:
![fd6a6492205607ec7ddfbd2b6036cf1d.png](../../_resources/fd6a6492205607ec7ddfbd2b6036cf1d.png)

Nothing interesting came out of main website scanned as a guest:
![28ba4a038acac3ed1e47d1fac57c057e.png](../../_resources/28ba4a038acac3ed1e47d1fac57c057e.png)
* * *
## Forum.collect.htb
Seems like there is a login and our dummy user is not working.. Seems like we can find something about a webdirectory?
![dce4ca88b33113769d5d57f0d99dc6ce.png](../../_resources/dce4ca88b33113769d5d57f0d99dc6ce.png)
 ![aa94c698eabb3c9bb32a61ef7f2b840e.png](../../_resources/aa94c698eabb3c9bb32a61ef7f2b840e.png)
 
 We can download a proxy history about those api.., but first we need to create again a dummy user.
 Forum user list:
 ![7361ae1f7649b8ae823a56ea6105bc75.png](../../_resources/7361ae1f7649b8ae823a56ea6105bc75.png)
 
 Checking the burpsuite report from victor we can find the URL to the API and a base64 request that need to be decoded:
 ![2ad1f53167d26f7fdc03d0f2b78048cd.png](../../_resources/2ad1f53167d26f7fdc03d0f2b78048cd.png)
 
 Decoded request:
 `POST /set/role/admin HTTP/1.1
Host: collect.htb
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:104.0) Gecko/20100101 Firefox/104.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: pt-BR,pt;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=r8qne20hig1k3li6prgk91t33j
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 38

token=ddac62a28254561001277727cb397baf`

Can we use that token to set our user(with our cookie) to use victors token?
* * *
## Developers.collect.htb
Seems like we need a user, so nothhing here...
* * *
## Collect.htb(API):
Now having the lgos from victors account we can recycle the code but use insted of his cookie the one fro our user(mine is yovecio, Cookie: PHPSESSID=v5erv4tlftbt8g49b59qkd38p5) and sending the request will convert our user to Admin?
![aa2351b144dc90ea0ce45458aa52d52d.png](../../_resources/aa2351b144dc90ea0ce45458aa52d52d.png)
We got a 302 found so maybe it worked?

Now we know we found a webdirectory from first Dirscan on mainwebsite called /admin:
![3e5744cd2e2b2a94e0fe3f7b1c9c1560.png](../../_resources/3e5744cd2e2b2a94e0fe3f7b1c9c1560.png)

When i tried first time as yovecio, wasn't working, but does it work now?
![821f87f8b84226d7a8332aae72adb64b.png](../../_resources/821f87f8b84226d7a8332aae72adb64b.png)

After our token impersonation we can see that we are admin on the portal..
Shall we try to register a user in the API?
![e97edc7fccfe92a73679d5d4aa6ee886.png](../../_resources/e97edc7fccfe92a73679d5d4aa6ee886.png)

As we see:
![9889f3c7973979c4c2e9076908f830cc.png](../../_resources/9889f3c7973979c4c2e9076908f830cc.png)
From burp this is a XML request:
![bf829fd5e1023bdd09e6c2e12642a590.png](../../_resources/bf829fd5e1023bdd09e6c2e12642a590.png)
Analyzing what we got, we gained access to /admin portal by forging token from Victors account and by passing it to api that make a user admin we elevated our user to Administrator. Then we gained access to /admin portal in order to be able to add users to the API.
Fetching the request we can see that request are sent to manage_api in XML format, anmd we can see that api answers with something:
![2e732569b731f3f66a9027e660903db6.png](../../_resources/2e732569b731f3f66a9027e660903db6.png)

Now we need to see if is XXE exploitable?
Here i had to check for tips since this is a bit out of my scope and apparently XXE is explotable by doing this:
https://book.hacktricks.xyz/pentesting-web/xxe-xee-xml-external-entity#blind-ssrf-exfiltrate-data-out-of-band

Which means we need to create a malicious.dtd(BTW the key to make it work was to use base64 encoding to bypass any filter):
![8e989000abac63e83adf506429b23a68.png](../../_resources/8e989000abac63e83adf506429b23a68.png)

Setup a python http server to serve the dtd file.
And then send the payload in the Burpsuite
![56f83a71e9b2003cb22873e2e5c2ba25.png](../../_resources/56f83a71e9b2003cb22873e2e5c2ba25.png)
![ddfa1531d663c8d211e5c7a1bc0f4e19.png](../../_resources/ddfa1531d663c8d211e5c7a1bc0f4e19.png)

This will give you back a BASE64 encoded result in the python server:
![01674a7681c7ba0d6cda7515dbe70ba7.png](../../_resources/01674a7681c7ba0d6cda7515dbe70ba7.png)

Now checking the file bootstrap.php gives us redis password:
![d59235aad37aaec3c75d7f416c5cfe67.png](../../_resources/d59235aad37aaec3c75d7f416c5cfe67.png)
![98de6ea32d37ed65ecbfc049a4fe13da.png](../../_resources/98de6ea32d37ed65ecbfc049a4fe13da.png)

`<?php
ini_set('session.save_handler','redis');
ini_set('session.save_path','tcp://127.0.0.1:6379/?auth=');
session_start();
require '../vendor/autoload.php';
=<?php
require '../bootstrap.php';
use app\classes\Routes;
use app\classes\Uri;
$routes = [
    "/" => "controllers/index.php",
    "/login" => "controllers/login.php",
    "/register" => "controllers/register.php",
    "/home" => "controllers/home.php",
    "/admin" => "controllers/admin.php",
    "/api" => "controllers/api.php",
    "/set/role/admin" => "controllers/set_role_admin.php",
    "/logout" => "controllers/logout.php"
];
$uri = Uri::load();
require Routes::load($uri, $routes);
`

* * *