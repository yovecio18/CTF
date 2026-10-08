Same as usual I will perform a full scan of all the TCP ports open on the machine.

```bash
PORT     STATE SERVICE  REASON         VERSION
22/tcp   open  ssh      syn-ack ttl 63 OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 9a:87:07:c6:bf:b1:2a:17:ed:3e:f1:83:a7:06:82:f8 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDdjkpOpM7WQdyGVnvOpucgUxiIb0NIOyxrTve+gi0MnOJJNxAuiqbp2JdMbxg0NVh7/pFP4nOSbgpaoAw0oAYIObo75B+mPUwWZfzhv1eo9MBJmrNd8e5RRF1ReUBfBBZQ/tcptO4mIE3wzxW8JFtaJSG1jyJE+N+F/yLR7m4ezGZZ7jldGQjv+s80X686aMYDqhwpQfHxfDKJLymPxqvvZGihdKsEsL76Ar1rt+dXh54oS6jxN7nyysR2XiBX7Nrt3FTdQKq9R8Lcm27jaCjSt19OUAPiodFdRI0url+zlBQMsdC33tTFLmFU5hMPuqKOysXnWWzauBIgdCLjvhK8tGVUXOhu6iyaNAojAo8kfHeYEyQhiGM38rfIQsFLz1QOB5ThjOort0iESZj1mgwTNmF6ARYygcxbmZ8n0xZjV7fiag5p+l/MGae9hlpcEVBHrMq8l2uEylF1NP3PVoyctRumyU3c8Z8Tyxc8HsZoi/UAJhf0M76kYz0xo/KnAmM=
|   256 b2:9b:a6:04:75:21:49:0d:89:9d:31:f3:e1:f2:28:0b (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBELSpLWd0V5/uw9A7YeqVHXfpq2he6zyy00ZksGP4ogP3rSruETrfqaRPYBzV8XzHsffMryurwH1fkLtCopusV8=
|   256 7f:fc:43:34:25:54:8b:3e:a7:68:f7:3f:d8:3c:49:72 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGCuFka1YKISO3J2pmTFj7keLEGzuMhTSNjkJHzF5Bt8
1194/tcp open  openvpn? syn-ack ttl 63
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|router
Running: Linux 4.X|5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 4.15 - 5.19, Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
TCP/IP fingerprint:
OS:SCAN(V=7.98%E=4%D=4/10%OT=22%CT=%CU=36556%PV=Y%DS=2%DC=T%G=N%TM=69D8FFA8
OS:%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=10C%TI=Z%CI=Z%II=I%TS=A)OPS(
OS:O1=M552ST11NW7%O2=M552ST11NW7%O3=M552NNT11NW7%O4=M552ST11NW7%O5=M552ST11
OS:NW7%O6=M552ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)ECN(
OS:R=Y%DF=Y%T=40%W=FAF0%O=M552NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS
OS:%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=
OS:Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=
OS:R%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%R
OS:UD=G)IE(R=Y%DFI=N%T=40%CD=S)

```

# OVPN

This server is only used to get an access to the OT network via the openvpn connection so for this reason it is time to move on.