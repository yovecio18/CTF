## RUSTSCAN

`PORT STATE SERVICE REASON VERSION 22/tcp open ssh syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0) | ssh-hostkey: | 3072 3d12971d86bc161683608f4f06e6d54e (RSA) | ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC/Jyuj3D7FuZQdudxWlH081Q6WkdTVz6G05mFSFpBpycfOrwuJpQ6oJV1I4J6UeXg+o5xHSm+ANLhYEI6T/JMnYSyEmVq/QVactDs9ixhi+j0R0rUrYYgteX7XuOT2g4ivyp1zKQP1uKYF2lGVnrcvX4a6ds4FS8mkM2o74qeZj6XfUiCYdPSVJmFjX/TgTzXYHt7kHj0vLtMG63sxXQDVLC5NwLs3VE61qD4KmhCfu+9viOBvA1ZID4Bmw8vgi0b5FfQASbtkylpRxdOEyUxGZ1dbcJzT+wGEhalvlQl9CirZLPMBn4YMC86okK/Kc0Wv+X/lC+4UehL//U3MkD9XF3yTmq+UVF/qJTrs9Y15lUOu3bJ9kpP9VDbA6NNGi1HdLyO4CbtifsWblmmoRWIr+U8B2wP/D9whWGwRJPBBwTJWZvxvZz3llRQhq/8Np0374iHWIEG+k9U9Am6rFKBgGlPUcf6Mg7w4AFLiFEQaQFRpEbf+xtS1YMLLqpg3qB0= | 256 7c4d1a7868ce1200df491037f9ad174f (ECDSA) | ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNgPXCNqX65/kNxcEEVPqpV7du+KsPJokAydK/wx1GqHpuUm3lLjMuLOnGFInSYGKlCK1MLtoCX6DjVwx6nWZ5w= | 256 dd978050a5bacd7d55e827ed28fdaa3b (ED25519) |_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIDyp1s8jG+rEbfeqAQbCqJw5+Y+T17PRzOcYd+W32hF 80/tcp open http syn-ack ttl 63 nginx 1.18.0 (Ubuntu) | http-methods: |_ Supported Methods: GET HEAD POST OPTIONS |_http-server-header: nginx/1.18.0 (Ubuntu) |_http-title: Did not follow redirect to http://stocker.htb Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port`

* * *

## HTTP

An extra subdomain have been found:
![aa06a920ff8e0a4af2895e1f44deba0f.png](../../_resources/aa06a920ff8e0a4af2895e1f44deba0f.png)

Checking on main website souce code we found nothing in particular except we may know a possible username:
![51ca7c6df58ec418b406077bbd1682a1.png](../../_resources/51ca7c6df58ec418b406077bbd1682a1.png)

So far nothing interesting about web directories on main website:
![a396fbbf5d7c34e059967396e04bef15.png](../../_resources/a396fbbf5d7c34e059967396e04bef15.png)

Again same on dev subdomain:
![a07d632977902305e4c876d4c6c551cb.png](../../_resources/a07d632977902305e4c876d4c6c551cb.png)

Now i had to check for tips on forum and apparently dev subdomain is injectable but is NOSQL(aka Mongodb) so checking here: https://book.hacktricks.xyz/pentesting-web/nosql-injection#basic-authentication-bypass
And using the bypass as application/json we can bypass the login form:
![d3ef1d64bfc5f858e088e0dd7a2246cb.png](../../_resources/d3ef1d64bfc5f858e088e0dd7a2246cb.png)

Now playing withb the website we can add one or several items to the basket and then sending the order we send a request to /api/poo/XXXXX where XXXX stands for the order number:
![5e8f2b453c193e870a5959903d99ab1e.png](../../_resources/5e8f2b453c193e870a5959903d99ab1e.png)
And knowing from the PDF that title field is the only field that we can use as exploit.
Googling aorund this may fit our case: https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting/server-side-xss-dynamic-pdf#path-disclosure

Using this to get our path:
https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting/server-side-xss-dynamic-pdf#path-disclosure

Now using wappalizer we can see that is a NodeJS application:
![e2cd73718d91a27eded9a00cbb3e5640.png](../../_resources/e2cd73718d91a27eded9a00cbb3e5640.png)

And knowing that main path should be /var/www/dev/xxx where xxx can be main.js or app.js or even main.js.

And now we can read local files with XSS: https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting/server-side-xss-dynamic-pdf#read-local-file
![27042089a095025f545451ccaba1a5a6.png](../../_resources/27042089a095025f545451ccaba1a5a6.png)

But the result frame is too smalll so let's see if we can resize it:
![ccb498832bce2c7bfad76f643c8503d7.png](../../_resources/ccb498832bce2c7bfad76f643c8503d7.png)

And we can read /etc/passwd:
![840b0e9e2f9a10a79a82648aa1ff3416.png](../../_resources/840b0e9e2f9a10a79a82648aa1ff3416.png)

Now we can try to read /var/www/dev/main.js
Edit: not showing anything, so checking app.js nothing but index.js shows us a user:
![752b94b6e8c3d941763f1194d2345f45.png](../../_resources/752b94b6e8c3d941763f1194d2345f45.png)

const dbURI = "mongodb://dev:IHeardPassphrasesArePrettySecure@localhost/dev?authSource=admin&w=1";

* * *
## USER.txt
Now going back to /etc/passwd we know that the only user that have bin/bash is angoose so trying to login as  him via ssh get's us the first flag:
![0a8ebac675f496719428522ad5261cc1.png](../../_resources/0a8ebac675f496719428522ad5261cc1.png)

* * *
## ROOT.txt
Now checking with some manual enumerations seems like we can run as sudo .js files with node:
`angoose@stocker:~$ sudo -l
[sudo] password for angoose: 
Matching Defaults entries for angoose on stocker:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User angoose may run the following commands on stocker:
    (ALL) /usr/bin/node /usr/local/scripts/*.js
`

But checking under /usr/local/scripts/ we don't  have write permissions, so we need to check what can we do extra. Going back on forum i got that wildcard can be exploited by doint directory trasversal and file actually don't need to be in the exactly folder as long the exension is same.

So we can generate a revshell in NODEJS from Payloadallthethings:
![c9248a6550b8cba2c947b49255aa0f1b.png](../../_resources/c9248a6550b8cba2c947b49255aa0f1b.png)
And place in under /tmp

Then knowing from sudo -l we can do:
/usr/bin/node /usr/local/scripts/*.js

The path can be escaped like this:
/usr/bin/node /usr/local/scripts/../../../tmp/shell.js

![6b9cd7481f85da4bc3403ffef64bb58c.png](../../_resources/6b9cd7481f85da4bc3403ffef64bb58c.png)

And then we can grab the last flag in the NC listener:
![9452dc0e4f11d43a7e05fcef62b85bd7.png](../../_resources/9452dc0e4f11d43a7e05fcef62b85bd7.png)