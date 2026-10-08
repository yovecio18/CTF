We found out in early stage that server PNT-SRVSVC had traces of a internal DC layin on 192.168.210.16 and from a easy ping it showed like we could reach that subnet so I will procede to enumerate all that subnet via ping sweep.

We must setup autoroute on the shell on SRVSVC to be able to reach it via Metasploit:

&nbsp;

```
msf6 post(multi/gather/ping_sweep) > show options 

Module options (post/multi/gather/ping_sweep):

   Name     Current Setting   Required  Description
   ----     ---------------   --------  -----------
   RHOSTS   192.168.110.0/24  yes       IP Range to perform ping sweep against.
   SESSION  1                 yes       The session to run this module on


View the full module info with the info, or info -d command.

msf6 post(multi/gather/ping_sweep) > sessions 

Active sessions
===============

  Id  Name  Type                     Information                       Connection
  --  ----  ----                     -----------                       ----------
  2         meterpreter x64/linux    riley @ 192.168.110.51            10.10.17.55:443 -> 10.10.110.35:26272 (::1)
  3         meterpreter x64/windows  NT AUTHORITY\SYSTEM @ PNT-SVRSVC  10.10.17.55:53 -> 10.10.110.35:43521 (192.168.110.52)

msf6 post(multi/gather/ping_sweep) > set session 3
session => 3
msf6 post(multi/gather/ping_sweep) > set rhosts 192.168.210.0/24
rhosts => 192.168.210.0/24
msf6 post(multi/gather/ping_sweep) > run

[*] Performing ping sweep for IP range 192.168.210.0/24
[+]     192.168.210.1 host found
[+]     192.168.210.10 host found
[+]     192.168.210.13 host found
[+]     192.168.210.12 host found
[+]     192.168.210.16 host found
[*] Post module execution completed
```

Nice we found out much more devices.

I will run a ping sweep directly from a device on this network to double check the results are identical and we are potentially not missing anything usefull during enumeration process.

```
*Evil-WinRM* PS C:\temp> 1..254 | % {"192.168.210.$($_): $(Test-Connection -count 1 -comp 192.168.210.$($_) -quiet)"}
192.168.210.1: True
192.168.210.10: True
192.168.210.12: True
192.168.210.13: True
192.168.210.16: True
```

Same here the the .14 is not answering back from ping but we know it's alive since it points to the ADFS service.

&nbsp;

&nbsp;