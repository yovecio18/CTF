## RUSTSCAN:
`PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 97af61441089b953f0803fd719b1e29c (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDBjDFc+UtqNVYIrxJx+2Z9ZGi7LtoV6vkWkbALvRXmFzqStfJ3UM7TuOcZcPd82vk0gFVN2/wjA3LUlbUlr7oSlD15DdJkr/XjYrZLJnG4NCxcAnbB5CIRaWmrrdGy5pJ/KgKr4UEVGDK+oAgE7wbv++el2WeD1DF8gw+GIHhtjrK1s0nfyNGcmGOwx8crtHB4xLpopAxWDr2jzMFMdGcIzZMRVLbe+TsG/8O/GFgNXU1WqFYGe4xl+MCmomjh9mUspf1WP2SRZ7V0kndJJxtRBTw6V+NQ/7EJYJPMeugOtbputyZMH+jALhzxBs07JLbw8Bh9JX+ZJl/j6VcIDfFRXxB7ceSe/cp4UYWcLqN+AsoE7k+uMCV6vmXYPNC3g5xfMMrDfVmGmrPbop0oPZUB3kr8iz5CI/qM61WI07/MME1uyM352WZHAJmeBLPAOy05ZBY+DgpVElkr0vVa+3UyKsF1dC3Qm2jisx/qh3sGauv1R8oXGHvy0+oeMOlJN+k=
|   256 95ed658dcd082b55dd1751311e3e1812 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOL9rRkuTBwrdKEa+8VrwUjloHdmUdDR87hBOczK1zpwrsV/lXE1L/bYvDMUDVD0jE/aqMhekqNfBimt8aX53O0=
|   256 337bc171d3330f924e835a1f5202935e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINM1K8Yufj5FJnBjvDzcr+32BQ9R/2lS/Mu33ExJwsci
80/tcp   open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
|_http-title: DUMB Docs
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.18.0 (Ubuntu)
3000/tcp open  http    syn-ack ttl 63 Node.js (Express middleware)
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: DUMB Docs
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port`
* * *
## SSH:
Nothing can be done on the ssh so far, the version is pretty much new and no known vulnerabibilties are known in the wild.
We can sure try do to a brutefoce attack on the ssh to guess users and passwords but it will make noise and take probably forever, and it's not part of the P.E. scope.
We may come back later when we find valid credentials.
* * *
## PORT 80:
So far trying to open the website we are presented on a website poiting to an possible application called "DUMBDocs" and we can immediately download source code that is hosted locally on the site:
![7a3b4b0e6092a58ed06f66727c1fac6f.png](../../_resources/7a3b4b0e6092a58ed06f66727c1fac6f.png)
We can unzip the folder and check the tree structure:
![bc21fa0d304d62545ff9ee3a38289caa.png](../../_resources/bc21fa0d304d62545ff9ee3a38289caa.png)

In the meantime will try to check for possible subdomains but nothing really came out:
![40d2489737936a1920fda044f244f502.png](../../_resources/40d2489737936a1920fda044f244f502.png)

Same for possible Webdirectories fuzzing and we can see some Api folders:
![91cf0a7041511f96bbeea386d265cf1a.png](../../_resources/91cf0a7041511f96bbeea386d265cf1a.png)

Reading the index.js seems like the api is answeing on the port 3000 and by judging that all /api folders in URL path are not answering I guess need to move forward to port 3000.

* * *
## PORT 3000:
So far seems like port 3000 gives back same results as port 80(HTTP)  both in Webdirectories fuzzing and Subdomain enumeration that's why I won't post again any screen here.
To make it easier I will run a Burpsuite session and will try to map any possible traffic and map a sitetree.
![7fea12b97c96f64bd8dc043b3fad4e9b.png](../../_resources/7fea12b97c96f64bd8dc043b3fad4e9b.png)

Now that we  have a structure I moved on, almost missed at the beginning but the /docs directory gives us hints on how to login/register etc:
![25d943e9e985a2046d7f0c0cc83017b3.png](../../_resources/25d943e9e985a2046d7f0c0cc83017b3.png)

Let's use our friend BurpSuite and catch a Register request:
![adf3151ebcc309d032747e51b453944a.png](../../_resources/adf3151ebcc309d032747e51b453944a.png)
Now we know we have to use POST in order to register so let's do it:
![35152b74e119ff43375b8ccd607a7dcd.png](../../_resources/35152b74e119ff43375b8ccd607a7dcd.png)

But we get an error because we must ship as Json body and application type:
![ea81e1c60459b928383804433ef1e1b0.png](../../_resources/ea81e1c60459b928383804433ef1e1b0.png)
Almost! I will try another email instead:
![6d06931fa9cbb1711e8b880fb0b2d0e1.png](../../_resources/6d06931fa9cbb1711e8b880fb0b2d0e1.png)
Boom! Now we can login:
![0682549cd9247ebff366164d96d1f6aa.png](../../_resources/0682549cd9247ebff366164d96d1f6aa.png)
But we need to edit to a POST HTTP form with the right JSON body and application type:
![31e2e77776ddcd94f3abebad3303a23e.png](../../_resources/31e2e77776ddcd94f3abebad3303a23e.png)
Again, now we got a JWT token so we can save it!
`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2NDA5YjAzYTY0OWRlMDA0NWFjOGJkZDAiLCJuYW1lIjoieW92ZWNpbyIsImVtYWlsIjoieW92ZWNpb0BkYXNpdGgud29ya3MiLCJpYXQiOjE2NzgzNTY5MjF9.7Iq_qejt8JXRG-9gtBha4rSpsGjYbxYyMawKpfFewtQ`
If we try to decode it: 
![b2065f32d27f47c4298e26fe878bbac7.png](../../_resources/b2065f32d27f47c4298e26fe878bbac7.png)

Now we can check if we are admins:
![baed635298eba3de7cf60d070887becb.png](../../_resources/baed635298eba3de7cf60d070887becb.png)
But we have to ship our JWT token in order to make it work:
![01ba2dfa77281acaf0547d28fb9fdd6a.png](../../_resources/01ba2dfa77281acaf0547d28fb9fdd6a.png)

Ok we could guess we are still normal user. Now we have all the tasks and we need to find a way to craft our JWT tokens in order to gain access as Admins and I think the secret key is somewhere into that source code we downloaded at first.
Here i was manually looking thru all the source files when I found what actually may help us get a foothold in /routes/private.js:
`router.get('/priv', verifytoken, (req, res) => {
   // res.send(req.user)

    const userinfo = { name: req.user }

    const name = userinfo.name.name;

    if (name == 'theadmin'){
        res.json({
            creds:{
                role:"admin",
                username:"theadmin",
                desc : "welcome back admin,"
            }
        })
    }
    else{
        res.json({
            role: {
                role: "you are normal user",
                desc: userinfo.name.name
            }
        })
    }
})`

Basically here seems like it's only the name of the account that matches if you are admin or not! So here I haven't found any secret key which means we can't forge new JTW tokens but knowing from out test that application only checks for email during the registering process we may create a new Admin user and possibly bypass this!
Let's try to add a new user with "theadmin" as name:
![371f720c4309985d2b3f85546b3948cb.png](../../_resources/371f720c4309985d2b3f85546b3948cb.png)
But it says the name already exist which means we have to forge a new ticket. Now we know from help tags under machine description that we had git so I tried to look for .git webdirectories but found nothing so instead I tried to list for hidden files and we had a git folder into the sorce zip:
![8b3efb5a3e5af7c147b1a8cd98766081.png](../../_resources/8b3efb5a3e5af7c147b1a8cd98766081.png)
![aa4438b4e6d2385b1d2f3377814f6dd9.png](../../_resources/aa4438b4e6d2385b1d2f3377814f6dd9.png)

let see if we can grab anything juicy from github commits:
`┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/local-web]
└─# git show 55fe756a29268f9b4e786ae468952ca4a8df1bd8
commit 55fe756a29268f9b4e786ae468952ca4a8df1bd8
Author: dasithsv <dasithsv@gmail.com>
Date:   Fri Sep 3 11:25:52 2021 +0530

    first commit

diff --git a/.env b/.env
new file mode 100644
index 0000000..fb6f587
--- /dev/null
+++ b/.env
@@ -0,0 +1,2 @@
+DB_CONNECT = 'mongodb://127.0.0.1:27017/auth-web'
+TOKEN_SECRET = gXr67TtoQL8TShUc8XYsK2HvsBYfyQSFCFZe4MQp7gRpFuMkKjcM72CNQN4fMfbZEKx4i7YiWuNAkmuTcdEriCMm9vPAYkhpwPTiuVwVhvwE`
Here aswell:
`┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/local-web]
└─# git show 67d8da7a0e53d8fadeb6b36396d86cdcd4f6ec78
commit 67d8da7a0e53d8fadeb6b36396d86cdcd4f6ec78
Author: dasithsv <dasithsv@gmail.com>
Date:   Fri Sep 3 11:30:17 2021 +0530

    removed .env for security reasons

diff --git a/.env b/.env
index fb6f587..31db370 100644
--- a/.env
+++ b/.env
@@ -1,2 +1,2 @@
 DB_CONNECT = 'mongodb://127.0.0.1:27017/auth-web'
-TOKEN_SECRET = gXr67TtoQL8TShUc8XYsK2HvsBYfyQSFCFZe4MQp7gRpFuMkKjcM72CNQN4fMfbZEKx4i7YiWuNAkmuTcdEriCMm9vPAYkhpwPTiuVwVhvwE
+TOKEN_SECRET = secret`

We may have everything we need my friend! Now I have generated a new token with the new name: 
![799390dd94b0a80f9b72c2d0a1bde409.png](../../_resources/799390dd94b0a80f9b72c2d0a1bde409.png)

And now we can check again if we are admins and it works!
![6293479b6d977b19a2a89f39c7deff59.png](../../_resources/6293479b6d977b19a2a89f39c7deff59.png)

Now we have everithing but I was missing the point so i checked for tips and I have missed a route(webdirectory) more specifically /logs into private.js file:
`router.get('/logs', verifytoken, (req, res) => {
    const file = req.query.file;
    const userinfo = { name: req.user }
    const name = userinfo.name.name;

    if (name == 'theadmin'){
        const getLogs = `git log --oneline ${file}`;
        exec(getLogs, (err , output) =>{
            if(err){
                res.status(500).send(err);
                return
            }
            res.json(output);
        })
    }
    else{
        res.json({
            role: {
                role: "you are normal user",
                desc: userinfo.name.name
            }
        })
    }
})`

If we visit it:
![70cef8d96ececa25d61aa4fdd5a36ab4.png](../../_resources/70cef8d96ececa25d61aa4fdd5a36ab4.png)
We know from code that is supposed to be a GET request `router.get('/logs', verifytoken, (req, res) => {` and that the parameter's name is file ` const getLogs = git log --oneline ${file};` 
Let's first try to pass a dummy value:
![6295e0c4c88cd38cb343be8ea013e2d0.png](../../_resources/6295e0c4c88cd38cb343be8ea013e2d0.png)

Good the value gets reflected, but can we use that on our advantage to spawn a Revshell? I will use a bash resvhell from https://www.revshells.com/ but it have to be URL encoded since is passed in the URL as get request:
![206df57107b39c8deafd925b1a8a82e4.png](../../_resources/206df57107b39c8deafd925b1a8a82e4.png)
And after several tries I ended up using this: 
![d6b8eed35111cc13dd291487971cbf12.png](../../_resources/d6b8eed35111cc13dd291487971cbf12.png)
That spawned me a shell <3
* * *
## USER.TXT:
First of all let's convert our Revshell in the NC listener as "Semi" interactive shell with python:  https://fahmifj.medium.com/get-a-fully-interactive-reverse-shell-b7e8d6f5b1c1
And then can we try to get out first flag:
![6df15c08727be44eee690fade54c9ad8.png](../../_resources/6df15c08727be44eee690fade54c9ad8.png)
* * *
## ROOT.TXT:
I see that there is only dasinth user and so we can only be root, so no extra steps in between. To make my life easier(yes i'm lazy) I will upload a linpeas.sh copy and run thru an extensive scan so I can check what privesc are available.
`╔══════════╣ Sudo version
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-version
Sudo version 1.8.31
╔══════════╣ CVEs Check
Vulnerable to CVE-2021-4034
Vulnerable to CVE-2021-3560
Potentially Vulnerable to CVE-2022-2588

-rwsr-xr-x 1 root root 18K Oct  7  2021 /opt/count (Unknown SUID binary!)`


So far what came out from a first filtering was some CVE from  the exploit suggester and a interersting SUID that might be the indended PE to root since we know from machine tags that SUID was there.
![498f93d9c9419db67129d69c0dcb0878.png](../../_resources/498f93d9c9419db67129d69c0dcb0878.png)

Trying to play aroud with that file we can see that it ask you for a file/folder as source and then it will count words/chars and at the end will let you save the report somewhere:
![e2fad579efbfe76e1b571b9337e2b604.png](../../_resources/e2fad579efbfe76e1b571b9337e2b604.png)

Now we know is owned by root but we have suid permissions so can we maybe use that to spawn a new RCE but as root this time? Giving the rce STRING is not working:
![65b82ecf85550245cb7060f7da0143c2.png](../../_resources/65b82ecf85550245cb7060f7da0143c2.png)

What about if we use concatenation instead, giving firstly a real file and then concatenate a RCE payload.
Edit: Here i had to check againg for tips since I didn't know how to bypass it, eventually the source code used by the app was available at:
![69946617104c96b24efdf7ec18b870db.png](../../_resources/69946617104c96b24efdf7ec18b870db.png)

And knowing that coredump is available:
![a90e9172ad20a2c08a057535a2d85ade.png](../../_resources/a90e9172ad20a2c08a057535a2d85ade.png)

We could read the /root/root.txt and when the program ask to save the report, kill the process from another RCE and then read the flag from the core dump

* * *