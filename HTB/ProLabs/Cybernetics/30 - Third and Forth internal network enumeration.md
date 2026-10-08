We can sart with FPING to check all the hosts alive on the D3V.LOCAL domain:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Cybernetics/CVE-2023-6553]
└─# fping -asgq 10.9.30.0/24
10.9.30.1
10.9.30.10
10.9.30.11
10.9.30.12
10.9.30.13
10.9.30.200
```

I see 4 machines!

And what about the Inception.local network?

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Cybernetics/CVE-2023-6553]
└─# fping -asgq 10.9.40.0/24                                                     
10.9.40.1
10.9.40.5
10.9.40.11
10.9.40.12
10.9.40.200
10.9.40.201

     254 targets
       6 alive
     248 unreachable
       0 unknown addresses

     992 timeouts (waiting for response)
     998 ICMP Echos sent
       6 ICMP Echo Replies received
       0 other ICMP received

 31.7 ms (min round trip time)
 35.5 ms (avg round trip time)
 46.2 ms (max round trip time)
        9.719 sec (elapsed real time)
```

Same story here, I will mvoe to