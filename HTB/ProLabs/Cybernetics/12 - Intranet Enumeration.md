Now I felt good to move on knowing that the second flag wasn't scattered somewhere in CYBERNETICS-CYWEBDW and that the other machines on the DMZ network might not be reachable from the outside but only from the inside I decided to move on and use the Powershell to perform a pingSweep scan and identify all the machines available over the 10.9.20.0/24 subnet:

```
PS C:\Temp> 1..254 | % {"10.9.20.$($_): $(Test-Connection -count 1 -comp 10.9.20.$($_) -quiet)"}
10.9.20.1: False
10.9.20.2: False
10.9.20.3: False
10.9.20.4: False
10.9.20.5: False
10.9.20.6: False
10.9.20.7: False
10.9.20.8: False
10.9.20.9: False
10.9.20.10: True
10.9.20.11: True
10.9.20.12: True
10.9.20.13: True
10.9.20.14: False
10.9.20.15: False
```

In this case I see 3 other machines(.13 is the CYBERNETICS-CYWEBDW itselft and must not be counted in)

We can do the same directly from our machine since we have Ligolo-ng with pivoting on!

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/Cybernetics]
└─# fping -asgq 10.9.20.0/24  

10.9.20.10
10.9.20.11
10.9.20.12
10.9.20.13

     254 targets
       4 alive
     250 unreachable
       0 unknown addresses

    1000 timeouts (waiting for response)
    1004 ICMP Echos sent
      40 ICMP Echo Replies received
       0 other ICMP received

 35.2 ms (min round trip time)
 37.8 ms (avg round trip time)
 40.7 ms (max round trip time)
        9.612 sec (elapsed real time)
```

I feel confident about the results so I will move on with singular enumeration