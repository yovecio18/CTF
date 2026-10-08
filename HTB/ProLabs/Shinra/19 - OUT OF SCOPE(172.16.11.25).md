The initial UDP scan shows the following:

```bash
└─$ nmap -F -sU 172.16.11.25
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-20 14:45 +0100
Nmap scan report for 172.16.11.25
Host is up (0.00022s latency).
All 100 scanned ports on 172.16.11.25 are in ignored states.
Not shown: 100 open|filtered udp ports (no-response)

Nmap done: 1 IP address (1 host up) scanned in 2.42 seconds

```

Where instead the TCP scan shows much more information:

```bash
PORT     STATE SERVICE       REASON         VERSION
22/tcp   open  ssh           syn-ack ttl 64 OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a8:93:f2:10:03:d3:19:42:71:c2:44:3f:41:76:99:ba (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNnG7Khva7qv0IRbEUqqAnsWPQsph53reinVWcWcnwvRRCsq9VuYkxz7+c1igeDdhWqDkD/NKrBbULtSe2eSe/c=
|   256 4e:1d:98:bd:66:ff:8e:59:e8:d1:1e:fc:c5:d0:d7:84 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICETM6OFLzCweVOaDNv3kCdEvmcBExuIxA56QJZ+YfWP
80/tcp   open  http          syn-ack ttl 64 nginx 1.18.0 (Ubuntu)
|_http-title: Welcome to nginx!
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-server-header: nginx/1.18.0 (Ubuntu)
9200/tcp open  http          syn-ack ttl 64 Elasticsearch REST API 7.0 or later (Shield plugin; realm: security)
|_http-title: Site doesn't have a title (application/json).
| http-methods: 
|   Supported Methods: GET DELETE HEAD OPTIONS
|_  Potentially risky methods: DELETE
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|   Basic charset=UTF-8 realm=security
|_  ApiKey
9300/tcp open  elasticsearch syn-ack ttl 64 Elasticsearch binary API
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): IBM z/OS 1.12.X (85%)
OS CPE: cpe:/o:ibm:zos:1.12
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: IBM z/OS 1.12 (85%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.98%E=4%D=3/20%OT=22%CT=%CU=%PV=Y%G=N%TM=69BD511B%P=x86_64-pc-linux-gnu)
SEQ(SP=103%GCD=1%ISR=10F%TI=I%CI=RD%II=RI%TS=A)
SEQ(SP=103%GCD=2%ISR=10B%TI=I%CI=I%II=RI%TS=A)
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

Uptime guess: 2.300 days (since Wed Mar 18 07:40:04 2026)
TCP Sequence Prediction: Difficulty=259 (Good luck!)
IP ID Sequence Generation: Incrementing by 2
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT      ADDRESS
1   14.50 ms 172.16.11.25

```