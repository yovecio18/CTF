We are given only one IP as entry point: 10.13.37.13.

Easiest is to start by enumerating the Network ports open from the "outside" with tools like NMAP or similar:

```Bash
PORT   STATE SERVICE REASON         VERSION
80/tcp open  http    syn-ack ttl 62 Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Hackfail.htb
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 4.15 - 5.8 (93%), Linux 5.0 - 5.5 (93%), Linux 5.0 - 5.4 (93%), Linux 5.3 - 5.4 (93%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.1 (93%), Linux 3.16 (93%), Linux 3.2 (93%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (93%), Linux 5.0 (92%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94%E=4%D=8/2%OT=80%CT=%CU=30647%PV=Y%DS=2%DC=T%G=N%TM=64CA249B%P=x86_64-pc-linux-gnu)
SEQ(SP=F8%GCD=1%ISR=105%TI=Z%TS=A)
SEQ(SP=F8%GCD=1%ISR=105%TI=Z%II=I%TS=A)
OPS(O1=M53AST11NW7%O2=M53AST11NW7%O3=M53ANNT11NW7%O4=M53AST11NW7%O5=M53AST11NW7%O6=M53AST11)
WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
ECN(R=Y%DF=Y%T=3F%W=FAF0%O=M53ANNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%T=3F%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%T=3F%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)
IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 27.768 days (since Wed Jul  5 17:14:37 2023)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=248 (Good luck!)
IP ID Sequence Generation: All zeros

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   65.90 ms 10.10.16.1
2   33.80 ms 10.13.37.13
```

Surprisingly we have only one service open running on a standard HTTP port 80/TPC, but we can see the possible domain name in the title so I will add that to my local hosts file and continue with enumeration.