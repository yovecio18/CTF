We are given only one Internal IPv4 address as out initial entry point: 10.13.37.10.

But let's start by finding more about the Machine and running services accessible from the outside:

```Bash
PORT     STATE SERVICE  REASON         VERSION
22/tcp   open  ssh      syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 62:f6:49:80:81:cf:f0:07:0e:5a:ad:e9:8e:1f:2b:7c (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCzfo72P9KbpK9EZIE/AdtfSawGO5rjNM4GOu1Te5M2Z576c7aEVWv+284kw4OQ6JxQtFL56QsVaxRwxY9jGdpluJw5AWQpASy/Rx8q2JT7yGv0yGI8+tAIjLOMNmq5Qt6IjbDiSbL+gp6a+AsA0Mvm9OUYxBDM+LRsKFjwLDJCzFVKMFGc+gNrYJwpRa9RADsXN/19ogVYG8v9GvqFAJygMyTqVM0fbX3dDcAlMWgcHu81wMmQQGznjLe2gTY/sFAhASAfieVnSYIF11amofP8eUd+6jWL1wSlhRcW+j15tsPcotcfdpCrUJMFXq2tumXfNLJUWhv75Rf8pUeVobsx
|   256 54:e2:7e:5a:1c:aa:9a:ab:65:ca:fa:39:28:bc:0a:43 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLz7MT1sqbOHsYtNwWDfYJW8uCAPUL+zMJrW2DvIM7a9jG2RI40LNKjtiYv+M6DXjTWr3DK21kIWWK4TKMEl5Wo=
|   256 93:bc:37:b7:e0:08:ce:2d:03:99:01:0a:a9:df:da:cd (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEEtJp2PzxXAhVXs6nNbgJXgRbLCiB/hcGJY5IgBKfS4
53/tcp   open  domain   syn-ack ttl 63 ISC BIND 9.10.3-P4 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.10.3-P4-Ubuntu
80/tcp   open  http     syn-ack ttl 63 nginx 1.10.3 (Ubuntu)
|_http-server-header: nginx/1.10.3 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-title: Welcome to nginx on Debian!
5555/tcp open  freeciv? syn-ack ttl 63
| fingerprint-strings: 
|   DNSVersionBindReqTCP, GenericLines, GetRequest, adbConnect: 
|     enter your name:
|     [31mMember manager!
|     edit
|     change name
|     gift
|     exit
|   NULL: 
|     enter your name:
|   SMBProgNeg: 
|     enter your name:
|     [31mMember manager!
|     edit
|     change name
|     gift
|     exit
|     invalid option!
|     [31mMember manager!
|     edit
|     change name
|     gift
|     exit
|     invalid option!
|     [31mMember manager!
|     edit
|     change name
|     gift
|     exit
|     invalid option!
|     [31mMember manager!
|     edit
|     change name
|     gift
|     exit
|     invalid option!
|     [31mMember manager!
|     edit
|     change name
|     gift
|     exit
|     invalid option!
|     [31mMember manager!
|     edit
|     change name
|     gift
|     exit
|     invalid option!
|     [31mMember manager!
|     edit
|     change name
|     gift
|     exit
|     invalid option!
|     [31mMember manager!
|     edit
|     change name
|     gift
|     exit
|     invalid option!
|     [31mMember manager!
|     edit
|     change name
|     gift
|     exit
|_    invalid option!
7777/tcp open  cbt?     syn-ack ttl 63
| fingerprint-strings: 
|   Arucer, DNSStatusRequestTCP, DNSVersionBindReqTCP, GenericLines, GetRequest, HTTPOptions, RPCCheck, RTSPRequest, Socks5, X11Probe: 
|     --==[[ Spiritual Memo ]]==--
|     Create a memo
|     Show memo
|     Delete memo
|     Can't you read mate?
|   NULL: 
|     --==[[ Spiritual Memo ]]==--
|     Create a memo
|     Show memo
|_    Delete memo
9201/tcp open  http     syn-ack ttl 63 BaseHTTPServer 0.3 (Python 2.7.12)
|_http-title: Site doesn't have a title (application/json).
|_http-server-header: BaseHTTP/0.3 Python/2.7.12
| http-methods: 
|_  Supported Methods: GET
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port5555-TCP:V=7.94%I=7%D=7/27%Time=64C224C3%P=x86_64-pc-linux-gnu%r(NU
SF:LL,11,"enter\x20your\x20name:\n")%r(GenericLines,63,"enter\x20your\x20n
SF:ame:\n\x1b\[31mMember\x20manager!\x1b\[0m\n1\.\x20add\n2\.\x20edit\n3\.
SF:\x20ban\n4\.\x20change\x20name\n5\.\x20get\x20gift\n6\.\x20exit\n")%r(D
SF:NSVersionBindReqTCP,63,"enter\x20your\x20name:\n\x1b\[31mMember\x20mana
SF:ger!\x1b\[0m\n1\.\x20add\n2\.\x20edit\n3\.\x20ban\n4\.\x20change\x20nam
SF:e\n5\.\x20get\x20gift\n6\.\x20exit\n")%r(SMBProgNeg,9D1,"enter\x20your\
SF:x20name:\n\x1b\[31mMember\x20manager!\x1b\[0m\n1\.\x20add\n2\.\x20edit\
SF:n3\.\x20ban\n4\.\x20change\x20name\n5\.\x20get\x20gift\n6\.\x20exit\nin
SF:valid\x20option!\n\x1b\[31mMember\x20manager!\x1b\[0m\n1\.\x20add\n2\.\
SF:x20edit\n3\.\x20ban\n4\.\x20change\x20name\n5\.\x20get\x20gift\n6\.\x20
SF:exit\ninvalid\x20option!\n\x1b\[31mMember\x20manager!\x1b\[0m\n1\.\x20a
SF:dd\n2\.\x20edit\n3\.\x20ban\n4\.\x20change\x20name\n5\.\x20get\x20gift\
SF:n6\.\x20exit\ninvalid\x20option!\n\x1b\[31mMember\x20manager!\x1b\[0m\n
SF:1\.\x20add\n2\.\x20edit\n3\.\x20ban\n4\.\x20change\x20name\n5\.\x20get\
SF:x20gift\n6\.\x20exit\ninvalid\x20option!\n\x1b\[31mMember\x20manager!\x
SF:1b\[0m\n1\.\x20add\n2\.\x20edit\n3\.\x20ban\n4\.\x20change\x20name\n5\.
SF:\x20get\x20gift\n6\.\x20exit\ninvalid\x20option!\n\x1b\[31mMember\x20ma
SF:nager!\x1b\[0m\n1\.\x20add\n2\.\x20edit\n3\.\x20ban\n4\.\x20change\x20n
SF:ame\n5\.\x20get\x20gift\n6\.\x20exit\ninvalid\x20option!\n\x1b\[31mMemb
SF:er\x20manager!\x1b\[0m\n1\.\x20add\n2\.\x20edit\n3\.\x20ban\n4\.\x20cha
SF:nge\x20name\n5\.\x20get\x20gift\n6\.\x20exit\ninvalid\x20option!\n\x1b\
SF:[31mMember\x20manager!\x1b\[0m\n1\.\x20add\n2\.\x20edit\n3\.\x20ban\n4\
SF:.\x20change\x20name\n5\.\x20get\x20gift\n6\.\x20exit\ninvalid\x20option
SF:!\n\x1b\[31mMember\x20manager!\x1b\[0m\n1\.\x20add\n2\.\x20edit\n3\.\x2
SF:0ban\n4\.\x20change\x20name\n5\.\x20get\x20gift\n6\.\x20exit\ninvalid\x
SF:20option!\n\x1b")%r(adbConnect,63,"enter\x20your\x20name:\n\x1b\[31mMem
SF:ber\x20manager!\x1b\[0m\n1\.\x20add\n2\.\x20edit\n3\.\x20ban\n4\.\x20ch
SF:ange\x20name\n5\.\x20get\x20gift\n6\.\x20exit\n")%r(GetRequest,63,"ente
SF:r\x20your\x20name:\n\x1b\[31mMember\x20manager!\x1b\[0m\n1\.\x20add\n2\
SF:.\x20edit\n3\.\x20ban\n4\.\x20change\x20name\n5\.\x20get\x20gift\n6\.\x
SF:20exit\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port7777-TCP:V=7.94%I=7%D=7/27%Time=64C224C3%P=x86_64-pc-linux-gnu%r(NU
SF:LL,5D,"\n--==\[\[\x20Spiritual\x20Memo\x20\]\]==--\n\n\[1\]\x20Create\x
SF:20a\x20memo\n\[2\]\x20Show\x20memo\n\[3\]\x20Delete\x20memo\n\[4\]\x20T
SF:ap\x20out\n>\x20")%r(X11Probe,71,"\n--==\[\[\x20Spiritual\x20Memo\x20\]
SF:\]==--\n\n\[1\]\x20Create\x20a\x20memo\n\[2\]\x20Show\x20memo\n\[3\]\x2
SF:0Delete\x20memo\n\[4\]\x20Tap\x20out\n>\x20Can't\x20you\x20read\x20mate
SF:\?")%r(Socks5,71,"\n--==\[\[\x20Spiritual\x20Memo\x20\]\]==--\n\n\[1\]\
SF:x20Create\x20a\x20memo\n\[2\]\x20Show\x20memo\n\[3\]\x20Delete\x20memo\
SF:n\[4\]\x20Tap\x20out\n>\x20Can't\x20you\x20read\x20mate\?")%r(Arucer,71
SF:,"\n--==\[\[\x20Spiritual\x20Memo\x20\]\]==--\n\n\[1\]\x20Create\x20a\x
SF:20memo\n\[2\]\x20Show\x20memo\n\[3\]\x20Delete\x20memo\n\[4\]\x20Tap\x2
SF:0out\n>\x20Can't\x20you\x20read\x20mate\?")%r(GenericLines,71,"\n--==\[
SF:\[\x20Spiritual\x20Memo\x20\]\]==--\n\n\[1\]\x20Create\x20a\x20memo\n\[
SF:2\]\x20Show\x20memo\n\[3\]\x20Delete\x20memo\n\[4\]\x20Tap\x20out\n>\x2
SF:0Can't\x20you\x20read\x20mate\?")%r(GetRequest,71,"\n--==\[\[\x20Spirit
SF:ual\x20Memo\x20\]\]==--\n\n\[1\]\x20Create\x20a\x20memo\n\[2\]\x20Show\
SF:x20memo\n\[3\]\x20Delete\x20memo\n\[4\]\x20Tap\x20out\n>\x20Can't\x20yo
SF:u\x20read\x20mate\?")%r(HTTPOptions,71,"\n--==\[\[\x20Spiritual\x20Memo
SF:\x20\]\]==--\n\n\[1\]\x20Create\x20a\x20memo\n\[2\]\x20Show\x20memo\n\[
SF:3\]\x20Delete\x20memo\n\[4\]\x20Tap\x20out\n>\x20Can't\x20you\x20read\x
SF:20mate\?")%r(RTSPRequest,71,"\n--==\[\[\x20Spiritual\x20Memo\x20\]\]==-
SF:-\n\n\[1\]\x20Create\x20a\x20memo\n\[2\]\x20Show\x20memo\n\[3\]\x20Dele
SF:te\x20memo\n\[4\]\x20Tap\x20out\n>\x20Can't\x20you\x20read\x20mate\?")%
SF:r(RPCCheck,71,"\n--==\[\[\x20Spiritual\x20Memo\x20\]\]==--\n\n\[1\]\x20
SF:Create\x20a\x20memo\n\[2\]\x20Show\x20memo\n\[3\]\x20Delete\x20memo\n\[
SF:4\]\x20Tap\x20out\n>\x20Can't\x20you\x20read\x20mate\?")%r(DNSVersionBi
SF:ndReqTCP,71,"\n--==\[\[\x20Spiritual\x20Memo\x20\]\]==--\n\n\[1\]\x20Cr
SF:eate\x20a\x20memo\n\[2\]\x20Show\x20memo\n\[3\]\x20Delete\x20memo\n\[4\
SF:]\x20Tap\x20out\n>\x20Can't\x20you\x20read\x20mate\?")%r(DNSStatusReque
SF:stTCP,71,"\n--==\[\[\x20Spiritual\x20Memo\x20\]\]==--\n\n\[1\]\x20Creat
SF:e\x20a\x20memo\n\[2\]\x20Show\x20memo\n\[3\]\x20Delete\x20memo\n\[4\]\x
SF:20Tap\x20out\n>\x20Can't\x20you\x20read\x20mate\?");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 3.12 (96%), Linux 3.13 (96%), Linux 3.16 (96%), Linux 3.2 - 4.9 (96%), Linux 3.8 - 3.11 (96%), Linux 4.8 (96%), Linux 4.4 (95%), Linux 4.9 (95%), Linux 3.18 (95%), Linux 4.2 (95%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94%E=4%D=7/27%OT=22%CT=%CU=41448%PV=Y%DS=2%DC=T%G=N%TM=64C2256D%P=x86_64-pc-linux-gnu)
SEQ(SP=106%GCD=1%ISR=109%TI=Z%CI=I%II=I%TS=8)
OPS(O1=M53CST11NW7%O2=M53CST11NW7%O3=M53CNNT11NW7%O4=M53CST11NW7%O5=M53CST11NW7%O6=M53CST11)
WIN(W1=7120%W2=7120%W3=7120%W4=7120%W5=7120%W6=7120)
ECN(R=Y%DF=Y%T=40%W=7210%O=M53CNNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)
IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 1.763 days (since Tue Jul 25 15:46:41 2023)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=262 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   33.92 ms 10.10.14.1
2   34.26 ms 10.13.37.10
```

As we can see we have several standard services as SSH(denotes that most likely it´s a Linux type machine), and DNS, HTTP services. Plus there are some other custom services on 5555 and 7777 and another HTTP based service on port 9201.

As usual Bruteforce is not a contempt way is that's why we won't do anything on SSH service without having a valid set of credentials.