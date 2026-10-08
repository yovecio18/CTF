As usual I will start by performing a full scan of all the open TCP services on this machine:

```bash
ORT     STATE SERVICE    REASON         VERSION
22/tcp   open  ssh        syn-ack ttl 64 OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 91:e8:dc:47:0b:8f:23:d8:7a:f9:8c:b7:49:8f:81:27 (ECDSA)
|_ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEFYNl0PIMgmVOIUPHUyJoiBfjbRsXqVwJpJ1wajUcZCFW1cejH1SG7I1Y7rphEiaSGG2gFbAyuxL1mQKBrKpCY=
80/tcp   open  http       syn-ack ttl 64 nginx
| http-robots.txt: 58 disallowed entries (40 shown)
| / /autocomplete/users /autocomplete/projects /search 
| /admin /profile /dashboard /users /api/v* /help /s/ /-/profile 
| /-/user_settings/profile /-/ide/ /-/experiment /*/new /*/edit /*/raw 
| /*/realtime_changes /groups/*/analytics 
| /groups/*/contribution_analytics /groups/*/group_members /groups/*/-/saml/sso 
| /*/*.git$ /*/archive/ /*/repository/archive* /*/activity 
| /*/blame /*/commits /*/commit /*/commit/*.patch 
| /*/commit/*.diff /*/compare /*/network /*/graphs 
| /*/merge_requests/*.patch /*/merge_requests/*.diff /*/merge_requests/*/diffs 
|_/*/deploy_keys /*/hooks
|_http-favicon: Unknown favicon MD5: 66F9A1C3F2CFD0DF1B570990E86D3095
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-trane-info: Problem with XML parsing of /evox/about
| http-title: Sign in \xC2\xB7 GitLab
|_Requested resource was http://172.16.41.150/users/sign_in
81/tcp   open  http       syn-ack ttl 64 nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Welcome to nginx!
| http-methods: 
|_  Supported Methods: GET HEAD
3128/tcp open  http-proxy syn-ack ttl 64 Squid http proxy 5.9
|_http-server-header: squid/5.9
|_http-title: ERROR: The requested URL could not be retrieved
8220/tcp open  ssl/http   syn-ack ttl 64 Golang net/http server
|_ssl-date: TLS randomness does not represent time
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 404 Not Found
|     Content-Type: text/plain; charset=utf-8
|     X-Content-Type-Options: nosniff
|     X-Request-Id: c08a173c-d85e-499c-b594-037c020128be
|     Date: Wed, 01 Apr 2026 13:17:14 GMT
|     Content-Length: 19
|     page not found
|   GenericLines, Help, LPDString, RTSPRequest, SIPOptions, SSLSessionReq, Socks5: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 404 Not Found
|     Content-Type: text/plain; charset=utf-8
|     X-Content-Type-Options: nosniff
|     X-Request-Id: 033cb3ee-700e-4fc5-a301-2cf0a266deac
|     Date: Wed, 01 Apr 2026 13:16:56 GMT
|     Content-Length: 19
|     page not found
|   HTTPOptions: 
|     HTTP/1.0 404 Not Found
|     Content-Type: text/plain; charset=utf-8
|     X-Content-Type-Options: nosniff
|     X-Request-Id: 03a17748-b765-4b2a-b11d-ddc403998a94
|     Date: Wed, 01 Apr 2026 13:16:56 GMT
|     Content-Length: 19
|_    page not found
| ssl-cert: Subject: commonName=siem/organizationName=elastic-fleet
| Subject Alternative Name: DNS:siem
| Issuer: commonName=localhost/organizationName=elastic-fleet
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-07-14T08:07:12
| Not valid after:  2034-07-14T08:07:12
| MD5:     11c1 8128 4a1b 3c3c 836a 097d f65b a219
| SHA-1:   b87d dc75 2008 a316 a049 5fa1 549b dbb5 5552 8b90
| SHA-256: 3526 25dc 3183 1238 6118 335d 4774 6bf1 a45d 3f15 b789 2bfa bed7 5a51 5292 65a0
| -----BEGIN CERTIFICATE-----
| MIIDMjCCAhqgAwIBAgICBnowDQYJKoZIhvcNAQELBQAwLDEWMBQGA1UEChMNZWxh
| c3RpYy1mbGVldDESMBAGA1UEAxMJbG9jYWxob3N0MB4XDTI0MDcxNDA4MDcxMloX
| DTM0MDcxNDA4MDcxMlowJzEWMBQGA1UEChMNZWxhc3RpYy1mbGVldDENMAsGA1UE
| AxMEc2llbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKO7eoBWW/mA
| mY5Sg3dZdcvZiF3dHrzZWikuGyMNsIBLRaJPOzrHvv2OD3hIsTQKiU57fHxNxkkR
| 2ElcNaamA/goyFH3CrNJ3b0qlItJaHt3/CpU4aXdDgKDohFA4BwGDCPl2AlxOxPP
| 0IjcfSGNIHVBI5ToxrkFMTjk9tr9P3hd9LEEPqzTWtWVWSn3vl6VCuFREU0s1hNT
| DqA+v1NIALTjh4n8GKjKrwAJBQZHwx9AgsjcDQSl/UlNZjQk+6/WzDVh72n8jRGJ
| od5sn1ohU7nO0sq3h1NxhBB9mGUXx69W39ubgdwqjqkgQErSmcaFHkK114NJCLNG
| 1u1n26b2cdkCAwEAAaNjMGEwDgYDVR0PAQH/BAQDAgeAMB0GA1UdJQQWMBQGCCsG
| AQUFBwMCBggrBgEFBQcDATAfBgNVHSMEGDAWgBTb2HLBilFvYpcx3a5ZjkA0rt0M
| hjAPBgNVHREECDAGggRzaWVtMA0GCSqGSIb3DQEBCwUAA4IBAQCrA6ZsA6EdFc1K
| VFIyNONa86+QxEG1J80Cjajw7zZRdD1evIr/9JSpREzT39F9qAwRjpSrDRNPXJQi
| xPgqkexZGKrzYkIZ9HSAbK4rSE6Lni2JG01Eolm4V4K0yWIv8hxLe7KuBt3JTbWY
| kDrwLawPHOWuJYPktoUPyXxedTl/YZ5ezYso5glk/0qFFLvAh/9rB4dsLT+lzj3H
| PPYbM/SluTD2mndSdkcFH9lMMSj7KfzdOc6lHwCd1dUKfoHseG4PwwOrtlnknVuN
| LLmVjiNgXdVUGpucu+c3omdNMWVVGjFzqLHT+6wK+g+PixUtNbVb18gu/vKYWugJ
| TdGalwF+
|_-----END CERTIFICATE-----
9300/tcp open  ssl/vrace? syn-ack ttl 64
| ssl-cert: Subject: commonName=siem
| Issuer: commonName=Elasticsearch security auto-configuration HTTP CA
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-07-13T15:17:30
| Not valid after:  2123-06-20T15:17:30
| MD5:     6fc6 e7dc f585 3a39 4b90 73e6 6630 28f2
| SHA-1:   9e51 cf0f 5042 3f08 88a1 6197 bf02 a161 3e7d 32ec
| SHA-256: c5b2 1649 fe2a 6b17 6d68 c36f 07fd 8f62 e2ce be9f ee1e 2820 d772 7dec 37e2 0022
| -----BEGIN CERTIFICATE-----
| MIIFKTCCAxGgAwIBAgIVAPZQu/Wkr4iuzUGvjT9JlPeHKoUqMA0GCSqGSIb3DQEB
| CwUAMDwxOjA4BgNVBAMTMUVsYXN0aWNzZWFyY2ggc2VjdXJpdHkgYXV0by1jb25m
| aWd1cmF0aW9uIEhUVFAgQ0EwIBcNMjQwNzEzMTUxNzMwWhgPMjEyMzA2MjAxNTE3
| MzBaMA8xDTALBgNVBAMTBHNpZW0wggIiMA0GCSqGSIb3DQEBAQUAA4ICDwAwggIK
| AoICAQDjcxGmZLQnDAqtgYskV34dJYmX+RcK6mma3ol+kTSeoq38k7qYN6QkjpRc
| 3caS8LvGKumBRReUCBtLFdRXmeDo/yWxou6LizPiDFH3Pwt6PC4YZ9i1AX6Dx3mB
| 61CCU7ScB7qD94U/k+AypQDZrRikk80719/nVyvWq4iWeT9zroJcRn23uKjICn/L
| LBHEdsRhFCKonRsVSMTSrNBOqMJ3N6jBNdIPB/2rON0DOBxUriGHppzk00rWMZzG
| wiEbPiebFgVdOexGOL0c18LPx24/KjAic7WY1U2JlbLX2diNe6Nn3oqwphU+acyu
| +HMtZWLQnG0/zsUf4Ssk40im/MqBI76Hy8G9s5mv/S7FvrBoIsS5Nw2T9BKhvZrr
| d7GxhmlXVznmfVT40T+ZjIyg2g4LsBF3DjHKooiAcrMFpsOldCLNfz0Q2u1VpYkV
| GHSEmzwHecye7TQ7nckEDoFYdug4SnpsEW3nbtZ05c9Vl3lqt0yaibeN8kDsqsJg
| tGdbMYwImGUkMbifvBlU6FRv/Rb/SHw6UaTXOT5eUmuoI296oW2VBC3MXiHh78vu
| LVl+jkxETUe6JvzuRWQSDxhIOTsij/6i3ILyd754zKyFXvSkrfZpQ6pB/PKv/2ey
| Hfm3dbU0xVNQrbJwERi72mTWejk7nbbFg0VtD5fUTxvxDPTsPwIDAQABo00wSzAd
| BgNVHQ4EFgQUISkhk8kUmZ5y208jqI+nb6mceEkwHwYDVR0jBBgwFoAU9ZCF7+Og
| 9aSVlUTLIMcj5srwEbQwCQYDVR0TBAIwADANBgkqhkiG9w0BAQsFAAOCAgEAjpnL
| adx0M9EgColky+riN/Pg18D+zo74Crnd8hBp7/NPu5Lvcu48yzydtU2OeHypzglw
| KOvQsQGnQCGwrcN15vFncQ7opX0FFGx/gLW2LNiMOLFUBHyWpuvSklX0SgFA5OHQ
| Q9AeSkkEh+HAeTJk46yACr5hy4Z+ReIX2i97B3lYpKxsf17OVh8Fk3OK5mRjrY75
| 2YNn51AIcXd4IzU5C/467jzYmh7KEWEjNJFNc+U/JeO0uhlhA5BydL60hn/ZzOaz
| OMdeRdEq0ti/NB7irMBUjk9aAbPkOUbehPKbEuZ6Bxd5siB2wtTEPk+tvSPiD5FM
| tmxs9Bi2ciMCmy7MEL2LvCw8wryv+Ayi1iTCI7eufr9FmCJY2a+eqxZ9Am7G8qMQ
| SRFDl48blHJz1ZaQg29IC58qsIzEXQ21SsjEXZnsrTM7ca7eU/zIM6n1aNY1A48R
| sauHGuFL5UWgC+tqv267LuORrWqK4HL7qK+4ug+thlfVPkxqXvdumC1sULOXTo6d
| dk7zsI9+FkXbJqGeZCmUfscyR/Panvpuplg5ifQVCUc7RQ+iioeslCDNaRtjqwdH
| SkJ1RAIDRHTm+ZJPUQDAdWD75muHmAPQKydKX36XiH8w7viB3JLJ1kn8YnGuDH9W
| Y4euNpXOJ4/dUHfSKmh1/jhSb+DT2a40IhDiRyc=
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8220-TCP:V=7.98%T=SSL%I=7%D=4/1%Time=69CD1AC7%P=x86_64-pc-linux-gnu
SF:%r(GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:
SF:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20
SF:Bad\x20Request")%r(GetRequest,E4,"HTTP/1\.0\x20404\x20Not\x20Found\r\nC
SF:ontent-Type:\x20text/plain;\x20charset=utf-8\r\nX-Content-Type-Options:
SF:\x20nosniff\r\nX-Request-Id:\x20033cb3ee-700e-4fc5-a301-2cf0a266deac\r\
SF:nDate:\x20Wed,\x2001\x20Apr\x202026\x2013:16:56\x20GMT\r\nContent-Lengt
SF:h:\x2019\r\n\r\n404\x20page\x20not\x20found\n")%r(HTTPOptions,E4,"HTTP/
SF:1\.0\x20404\x20Not\x20Found\r\nContent-Type:\x20text/plain;\x20charset=
SF:utf-8\r\nX-Content-Type-Options:\x20nosniff\r\nX-Request-Id:\x2003a1774
SF:8-b765-4b2a-b11d-ddc403998a94\r\nDate:\x20Wed,\x2001\x20Apr\x202026\x20
SF:13:16:56\x20GMT\r\nContent-Length:\x2019\r\n\r\n404\x20page\x20not\x20f
SF:ound\n")%r(RTSPRequest,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nConten
SF:t-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n
SF:400\x20Bad\x20Request")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r
SF:\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close
SF:\r\n\r\n400\x20Bad\x20Request")%r(SSLSessionReq,67,"HTTP/1\.1\x20400\x2
SF:0Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nCon
SF:nection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(FourOhFourRequest,E4
SF:,"HTTP/1\.0\x20404\x20Not\x20Found\r\nContent-Type:\x20text/plain;\x20c
SF:harset=utf-8\r\nX-Content-Type-Options:\x20nosniff\r\nX-Request-Id:\x20
SF:c08a173c-d85e-499c-b594-037c020128be\r\nDate:\x20Wed,\x2001\x20Apr\x202
SF:026\x2013:17:14\x20GMT\r\nContent-Length:\x2019\r\n\r\n404\x20page\x20n
SF:ot\x20found\n")%r(LPDString,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nC
SF:ontent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\
SF:n\r\n400\x20Bad\x20Request")%r(SIPOptions,67,"HTTP/1\.1\x20400\x20Bad\x
SF:20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnectio
SF:n:\x20close\r\n\r\n400\x20Bad\x20Request")%r(Socks5,67,"HTTP/1\.1\x2040
SF:0\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\
SF:nConnection:\x20close\r\n\r\n400\x20Bad\x20Request");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete

```

&nbsp;