## Enumerating the Perimeter
We should start by enumerating both /24 subnets and see which host are alive

* * *
## 192.168.100.0/24
Our first enumeration is not showing any live host on this network but we know that at least one VM is on the network **L-SRV02: 192.168.100.100**
I guess there is some VLAN implementation that is not letting us enumerate the network.

* * *
## 10.200.111.0/24
The hostscan found only 2 IP one is from **L-SRV01: 10.200.111.33** and the other is **10.200.111.250** but we need to understand more about the server.

* * *
