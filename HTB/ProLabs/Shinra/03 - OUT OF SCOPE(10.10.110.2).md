Judgind from the insuccess of the NMAP scan I suspect this might be the firewall used to manage all the traffic:

```bash
└─$ rustscan -a 10.10.110.2 -- -A -T4
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Real hackers hack time ⌛

[~] The config file is expected to be at "/home/millycash/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
[!] Looks like I didn't find any open ports for 10.10.110.2. This is usually caused by a high batch size.
        
*I used 4500 batch size, consider lowering it with 'rustscan -b <batch_size> -a <ip address>' or a comfortable number for your system.
        
 Alternatively, increase the timeout if your ping is high. Rustscan -t 2000 for 2000 milliseconds (2s) timeout.

```

For this reason I will abbandon this IP(for now at least!).