We are given only one IP: 10.13.37.12

Let's start by starting with a Enumeration about of our entry point:

```BAsh
PORT     STATE SERVICE       REASON          VERSION
443/tcp  open  ssl/https     syn-ack ttl 127
| http-methods: 
|_  Supported Methods: GET
| ssl-cert: Subject: commonName=WMSvc-SHA2-WEB
| Issuer: commonName=WMSvc-SHA2-WEB
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-10-12T18:31:49
| Not valid after:  2030-10-10T18:31:49
| MD5:   c589:6bbb:2543:2625:af6d:b035:3d43:022f
| SHA-1: 1c2d:2993:7485:b9d7:c1c4:ad24:413d:298e:0860:6c6e
| -----BEGIN CERTIFICATE-----
| MIIC4zCCAcugAwIBAgIQbB3YxCogyIFPZivcnEqnwzANBgkqhkiG9w0BAQsFADAZ
| MRcwFQYDVQQDEw5XTVN2Yy1TSEEyLVdFQjAeFw0yMDEwMTIxODMxNDlaFw0zMDEw
| MTAxODMxNDlaMBkxFzAVBgNVBAMTDldNU3ZjLVNIQTItV0VCMIIBIjANBgkqhkiG
| 9w0BAQEFAAOCAQ8AMIIBCgKCAQEAx7/HBeJVXUn+RPPwUBVMvCAJYKQPiX/nRxZX
| Xhpfpv8Y+AP6Gd4Ou/tAjUomLaxnj06OBuu9K2I2Mh4zFMucMEsrWKsaQ++y5iKM
| CUkSoVI7XbPKt+7N2zWuQOxWmLsEHII+yCT/blhz3C/j+jj7iv7Y4nRg1XHn+D21
| nOJ4Aq1ZS6o4hN1zwYv3d0+CeYpR2TjPwT3FKAIBNmuBKQ2ZFCgW6723FjhGcxTL
| N9ob19zbPsE9d6CF9Lnm3dQrNnTVI1YledDF917jxx/NdjnAScyBWeqkTsnBU163
| p6QJDwFW8g5wOuXQLdnn1i6V4GxmJvXyUpc/NGPKJJ9GSwv/vQIDAQABoycwJTAT
| BgNVHSUEDDAKBggrBgEFBQcDATAOBgNVHQ8EBwMFALAAAAAwDQYJKoZIhvcNAQEL
| BQADggEBACnBPEjeAS/Dj6M31gRydL5o2cRwWAwV5SvGSgya42fyebvlvA5H9jPv
| rIe6x+K62DFG7j4AqiHDw27FRPcm5fklXWvMkoJ+ffFqytEJkennf1iJL16J7vlg
| ZBaOarzBIsq5cuAngefdQONrDuJpMblA5wRW05NcYm3LsFBPhVfYFMHljThHV2Fj
| a6jYHW63PcINcNx8djwIWvA6LWY6J2Ul3oGprtnNdbuWc6MnQgTHwLlv2M5tiBHS
| FLnBDdBh5oHxMDv2P0P7VUoLn+oCWpVjjOBYd6VGYkflSHTkEARhfHkeP6UOKHj3
| CI5x2n+P7ZymbKMF3fcOYeSy/K3Z1qs=
|_-----END CERTIFICATE-----
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Home page - Home
1433/tcp open  ms-sql-s      syn-ack ttl 127 Microsoft SQL Server 2019 15.00.2070.00; GDR1
| ms-sql-ntlm-info: 
|   10.13.37.12:1433: 
|     Target_Name: TEIGNTON
|     NetBIOS_Domain_Name: TEIGNTON
|     NetBIOS_Computer_Name: WEB
|     DNS_Domain_Name: TEIGNTON.HTB
|     DNS_Computer_Name: WEB.TEIGNTON.HTB
|     DNS_Tree_Name: TEIGNTON.HTB
|_    Product_Version: 10.0.17763
| ms-sql-info: 
|   10.13.37.12:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 GDR1
|       number: 15.00.2070.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: GDR1
|       Post-SP patches applied: false
|_    TCP port: 1433
3389/tcp open  ms-wbt-server syn-ack ttl 127 Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: TEIGNTON
|   NetBIOS_Domain_Name: TEIGNTON
|   NetBIOS_Computer_Name: WEB
|   DNS_Domain_Name: TEIGNTON.HTB
|   DNS_Computer_Name: WEB.TEIGNTON.HTB
|   DNS_Tree_Name: TEIGNTON.HTB
|   Product_Version: 10.0.17763
|_  System_Time: 2023-07-28T07:40:07+00:00
| ssl-cert: Subject: commonName=WEB.TEIGNTON.HTB
| Issuer: commonName=WEB.TEIGNTON.HTB
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-07-11T12:01:16
| Not valid after:  2024-01-10T12:01:16
| MD5:   ed98:7836:c8f1:5179:ce36:0f84:5b05:00ce
| SHA-1: ab19:4409:ae71:33ae:36ea:23c8:6430:d590:6319:2e6b
| -----BEGIN CERTIFICATE-----
| MIIC5DCCAcygAwIBAgIQUVQiLo/QLoNLQ/38f6s1tzANBgkqhkiG9w0BAQsFADAb
| MRkwFwYDVQQDExBXRUIuVEVJR05UT04uSFRCMB4XDTIzMDcxMTEyMDExNloXDTI0
| MDExMDEyMDExNlowGzEZMBcGA1UEAxMQV0VCLlRFSUdOVE9OLkhUQjCCASIwDQYJ
| KoZIhvcNAQEBBQADggEPADCCAQoCggEBAK6K2pj8kRHhYjCOtBLmLRHdXCx6Lobi
| JQ0QA7innEdXT95oy0JScjoIiz1mPFTGV8PzMVMWRcYE13nNREUAYS/lc8OJzCqt
| adMWyu256qECXOsAUvizN3eMvuKmdoQno1XuFrHTbDwuKWC+Uw21QGQI2VuVPfdZ
| PN9WDcBKlrlmbCS7OQc2B+4KgJddjczNDy4k/o7FOEyKiW6bv2LlzpEZk0I7Yio7
| OGoU7pFqMl8DljR3h24mOQhFn232yi8qpx0dGi4mQOF0enrV2h7rhv9B9XBgBkFI
| jElqJ9kNMdGPhaP6fKHhBzklWaEkqRUiy9eUZEgrpD4h4GsMLZGj9r0CAwEAAaMk
| MCIwEwYDVR0lBAwwCgYIKwYBBQUHAwEwCwYDVR0PBAQDAgQwMA0GCSqGSIb3DQEB
| CwUAA4IBAQCXh1r46WWxSCo5Y1yMqKyU0e5pX0yw/fnyi7J9vgVo1uArbHrzrt2x
| Gj7ibcNOzOFLdkm1g7F6CnFkY9J5/iKI+zJnhDXiXWDmjxEyccOxYovFpvrS/PQn
| 6+WZRF5pjbW5fwxp+fzeQTSiQ8VkozfH7I+dPvDDGJrX3tZSuKr0sZa4PkOBjFiu
| 3Pyi+Dgq6Ii4sYeODiXmt6f+2MP0+7PcXPNS62AEPz5nNWIp2ovPxXfP4XhW+ZQa
| hSotMcqJWcXpg0WNH0t4xnlwlqjqF0B9MjhFBYWKvkMCFz5ADphoFKE7Ezjt2CoH
| zdvlbUqYVvfVoAwyI0BUbwJGuLlhmF4l
|_-----END CERTIFICATE-----
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2019 (88%)
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Microsoft Windows Server 2019 (88%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94%E=4%D=7/28%OT=443%CT=%CU=%PV=Y%DS=2%DC=T%G=N%TM=64C370F4%P=x86_64-pc-linux-gnu)
SEQ(SP=103%GCD=1%ISR=101%TI=I%II=I%SS=S%TS=U)
OPS(O1=M53CNW8NNS%O2=M53CNW8NNS%O3=M53CNW8%O4=M53CNW8NNS%O5=M53CNW8NNS%O6=M53CNNS)
WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W6=FF70)
ECN(R=Y%DF=Y%TG=80%W=FFFF%O=M53CNW8NNS%CC=Y%Q=)
T1(R=Y%DF=Y%TG=80%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=N)
U1(R=N)
IE(R=Y%DFI=N%TG=80%CD=Z)
```

In same matter I executed a network port scan on UPD services but nothing really came out:

```Bash
──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/CONTEXT]
└─# nmap -sU -T4 --top-ports 100 PORT     STATE SERVICE       REASON          VERSION                                                                                                        
└─# rustscan -a 10.13.37.12 -- -A -T4
└─# nmap -sU -T4 --top-ports 100 10.13.37.12
Starting Nmap 7.94 ( https://nmap.org ) at 2023-07-28 09:58 CEST
Nmap scan report for 10.13.37.12
Host is up (0.031s latency).
All 100 scanned ports on 10.13.37.12 are in ignored states.
Not shown: 100 open|filtered udp ports (no-response)

Nmap done: 1 IP address (1 host up) scanned in 5.58 seconds

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/CONTEXT]
```

And resuming the result we have found RDP, MSSQL instance, Win-RM, and a HTTPS site.

Now further analyzing the NMAP results we can see that server should be called WEB.TEIGNTON.HTB so let's add that to our hosts file and move forward with enumeration.