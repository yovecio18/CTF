## RUSTSCAN
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 e22473bbfbdf5cb520b66876748ab58d (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCwlzrcH3g6+RJ9JSdH4fFJPibAIpAZXAl7vCJA+98jmlaLCsANWQXth3UsQ+TCEf9YydmNXO2QAIocVR8y1NUEYBlN2xG4/7txjoXr9QShFwd10HNbULQyrGzPaFEN2O/7R90uP6lxQIDsoKJu2Ihs/4YFit79oSsCPMDPn8XS1fX/BRRhz1BDqKlLPdRIzvbkauo6QEhOiaOG1pxqOj50JVWO3XNpnzPxB01fo1GiaE4q5laGbktQagtqhz87SX7vWBwJXXKA/IennJIBPcyD1G6YUK0k6lDow+OUdXlmoxw+n370Knl6PYxyDwuDnvkPabPhkCnSvlgGKkjxvqks9axnQYxkieDqIgOmIrMheEqF6GXO5zz6WtN62UAIKAgxRPgIW0SjRw2sWBnT9GnLag74cmhpGaIoWunklT2c94J7t+kpLAcsES6+yFp9Wzbk1vsqThAss0BkVsyxzvL0U9HvcyyDKLGFlFPbsiFH7br/PuxGbqdO9Jbrrs9nx60=
|   256 04e3ac6e184e1b7effac4fe39dd21bae (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBrVE9flXamwUY+wiBc9IhaQJRE40YpDsbOGPxLWCKKjNAnSBYA9CPsdgZhoV8rtORq/4n+SO0T80x1wW3g19Ew=
|   256 20e05d8cba71f08c3a1819f24011d29e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEp8nHKD5peyVy3X3MsJCmH/HIUvJT+MONekDg5xYZ6D
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://photobomb.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
* * *
## HTTP
So far no subdomains have been discovered:
![e967742d4049f72d4b5232450e5bc5e8.png](../../_resources/e967742d4049f72d4b5232450e5bc5e8.png)

Only one folder have been found:
![f15340fa3d25f57643fba50f743163d2.png](../../_resources/f15340fa3d25f57643fba50f743163d2.png)

Loggin into website we see that printer is pointing to an auth page, and we should get login into welcome package?
![7a40fa1b3d4fbfc1ee0410e9266b8a64.png](../../_resources/7a40fa1b3d4fbfc1ee0410e9266b8a64.png)

Going back i decided to enumerate with bigger list the site and I've found several directories:
![9bd0a778a8ec756e8232dc9f53c9451a.png](../../_resources/9bd0a778a8ec756e8232dc9f53c9451a.png)

Ok seems like we have a js script that can be used as leverage:
![69c7a30550b520949446b13d37afa6a3.png](../../_resources/69c7a30550b520949446b13d37afa6a3.png)
And we have credentials:
![84ced00f2b11dfbea4e7c22c6542dc4e.png](../../_resources/84ced00f2b11dfbea4e7c22c6542dc4e.png)
Using the link we get login to another website: 
![3e18189b965ad4bdac9d296f67e950b3.png](../../_resources/3e18189b965ad4bdac9d296f67e950b3.png)

Now here i had to check for tips and apparently we had to surf to http://photobomb.htb/printer/welcome
![cabf633ab9a9eb0bc33294689394c7ad.png](../../_resources/cabf633ab9a9eb0bc33294689394c7ad.png)
Edit: that was a rabbit hole.

Checking back on tips on forum i was doing the right thing in the beginning, trying to fuzz for Download parameters i tried to do add a revshell to file fortmat but was not URL encoding it. 
Choosing a revshell fro  Payloadallthethings, and doing photo=eleanor-brooke-w-TLY0Ym4rM-unsplash.jpg&filetype=jpg;<revshell_url_encoded>&dimensions=1000x1500
![2d84d2600dc420402cd60ce612b6e1d5.png](../../_resources/2d84d2600dc420402cd60ce612b6e1d5.png)

Now we have a shell in our NC listener:
![85e2d94c870b58fcacaba642dc35768d.png](../../_resources/85e2d94c870b58fcacaba642dc35768d.png)
* * *
## USER
And we grab our first flag:
![8ea77343ec0f965d704b078352e5021d.png](../../_resources/8ea77343ec0f965d704b078352e5021d.png)

Now we don't have wizards password and we don't have any ssh keys so we need to be fast and clean to escalate to Root without a proper ssh session, my suggestion is to upload and run linpeas to se what can we find as root priv esc?

`
╔══════════╣ Sudo version
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-version
Sudo version 1.8.31
╔══════════╣ CVEs Check
Vulnerable to CVE-2021-3560
Potentially Vulnerable to CVE-2022-2588
╔══════════╣ Checking 'sudo -l', /etc/sudoers, and /etc/sudoers.d
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-and-suid
Matching Defaults entries for wizard on photobomb:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
User wizard may run the following commands on photobomb:
    (root) SETENV: NOPASSWD: /opt/cleanup.sh
	-rw-r--r-- 1 root root 302 Mar 23  2022 /etc/nginx/sites-available/photobomb.htb.conf
server {
    listen       80;
    server_name  photobomb.htb;
    location / {
      proxy_pass http://127.0.0.1:4567;
    }
    location /printer {
      auth_basic           "Admin Area";
      auth_basic_user_file /home/wizard/photobomb/.htpasswd;
      proxy_pass http://127.0.0.1:4567;
    }
}
`

Now here i thought that we could path highjack and add find since it was not defined as absolute path but it didn't lead us nowehere. Then i had to watch back what was exactly the sudo -l message and showed that:
   (root) SETENV: NOPASSWD: /opt/cleanup.sh
   
   That SETENV means that we can set environment path:
   https://book.hacktricks.xyz/linux-hardening/privilege-escalation#setenv
   
   But we have no Python in sudo -l sir.. But then i had to check for tips and apparently this was the way to go:
   https://book.hacktricks.xyz/linux-hardening/privilege-escalation#setenv
   
   Compile the shell with code from the tutorial on your machine:
   ![7e57d21c336e3de798f0d7cc87f2c357.png](../../_resources/7e57d21c336e3de798f0d7cc87f2c357.png)
   
   Upload it and add executable permissions in /tmp folder. Then preload the file and run the sh script, now you should be able to get root on same shell:
![4807c05c3f22595324087130f4145b31.png](../../_resources/4807c05c3f22595324087130f4145b31.png)

Now hurry and catch last flag:
![51df0dc5db5a61349f21710f8fe9a8f1.png](../../_resources/51df0dc5db5a61349f21710f8fe9a8f1.png)