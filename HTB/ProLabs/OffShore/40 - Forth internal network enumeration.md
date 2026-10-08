We know so far that the `DC03` can talk to another domain at `client.offshore.com`.

![3cd3d8a475238fd35f63c7464ab5e962.png](../../../_resources/3cd3d8a475238fd35f63c7464ab5e962.png)

![a0f79ed951973bb8e491842203b3c253.png](../../../_resources/a0f79ed951973bb8e491842203b3c253.png)

Can we see something again by using our credentials from the ADMIN domain? So again setting up the connection and we can start with FPING to check all the available hosts on the new network:

```
┌──(root㉿kali-bello)-[/home/millycash/Downloads/OffShore/admin.offshore.com]
└─# fping -asgq 172.16.4.0/24
172.16.4.5
172.16.4.31

     254 targets
       2 alive
     252 unreachable
       0 unknown addresses

    1008 timeouts (waiting for response)
    1010 ICMP Echos sent
       2 ICMP Echo Replies received
       0 other ICMP received

 44.1 ms (min round trip time)
 61.0 ms (avg round trip time)
 78.0 ms (max round trip time)
        9.808 sec (elapsed real time)
```

I see only 2 on the meantime I will launch a similar command but natively in powershell:

![7d40a36fd0ff4ab6c97d5afe9a29cb11.png](../../../_resources/7d40a36fd0ff4ab6c97d5afe9a29cb11.png)

&nbsp;

&nbsp;