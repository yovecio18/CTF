## Rustscan
`PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey:
|   3072 845e13a8e31e20661d235550f63047d2 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDEAPxqUubE88njHItE+mjeWJXOLu5reIBmQHCYh2ETYO5zatgel+LjcYdgaa4KLFyw8CfDbRL9swlmGTaf4iUbao4jD73HV9/Vrnby7zP04OH3U/wVbAKbPJrjnva/czuuV6uNz4SVA3qk0bp6wOrxQFzCn5OvY3FTcceH1jrjrJmUKpGZJBZZO6cp0HkZWs/eQi8F7anVoMDKiiuP0VX28q/yR1AFB4vR5ej8iV/X73z3GOs3ZckQMhOiBmu1FF77c7VW1zqln480/AbvHJDULtRdZ5xrYH1nFynnPi6+VU/PIfVMpHbYu7t0mEFeI5HxMPNUvtYRRDC14jEtH6RpZxd7PhwYiBctiybZbonM5UP0lP85OuMMPcSMll65+8hzMMY2aejjHTYqgzd7M6HxcEMrJW7n7s5eCJqMoUXkL8RSBEQSmMUV8iWzHW0XkVUfYT5Ko6Xsnb+DiiLvFNUlFwO6hWz2WG8rlZ3voQ/gv8BLVCU1ziaVGerd61PODck=
|   256 a2ef7b9665ce4161c467ee4e96c7c892 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFScv6lLa14Uczimjt1W7qyH6OvXIyJGrznL1JXzgVFdABwi/oWWxUzEvwP5OMki1SW9QKX7kKVznWgFNOp815Y=
|   256 33053dcd7ab798458239e7ae3c91a658 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIH+JGiTFGOgn/iJUoLhZeybUvKeADIlm0fHnP/oZ66Qb
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0
|_http-title: Did not follow redirect to http://precious.htb/
|_http-server-header: nginx/1.18.0
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS`
* * *
## HTTP
No subdomains have been found so far:
![b9d02eeb938de0ee594413f7702af009.png](../../_resources/b9d02eeb938de0ee594413f7702af009.png)

No extra Webdirectories have been discovered so far:
![0c6ba1bd580c892a1eaf57c35b2a3937.png](../../_resources/0c6ba1bd580c892a1eaf57c35b2a3937.png)

Loggin into main website we are presented with a form that apparently fetch URL and convert them into a pdf, let's see on the magnifier what can we grab more with Burpsuite.
To test the url fetch we need to setup a dummy html page in our machine served by http.server module on python:
![506f91621f80841c669c59a5bdb0f1a1.png](../../_resources/506f91621f80841c669c59a5bdb0f1a1.png)

Giving the full URL to the page a pdf is generated:
![fbd651cfe6b1f892f9e72021ac35054d.png](../../_resources/fbd651cfe6b1f892f9e72021ac35054d.png)

Downloading the pdf and checking it with exiftool we got something important in the creator field:
![74c4c6a598e1f7aa5e47c76872fc2a96.png](../../_resources/74c4c6a598e1f7aa5e47c76872fc2a96.png)

* * *
## REVSHELL
Knowing the plugin used to generate pdfs we can use this as leverage point to our first shell: https://github.com/CyberArchitect1/CVE-2022-25765-pdfkit-Exploit-Reverse-Shell
Setting up a listener and we have everithing ready to invoke our shell:
![7fff624e664aad98e60f8d602fd4b67a.png](../../_resources/7fff624e664aad98e60f8d602fd4b67a.png)

And baam we have a shell:
![24370a0a99cd754a72e7c6a5f89c80dc.png](../../_resources/24370a0a99cd754a72e7c6a5f89c80dc.png)
OBS: the code on the website is missing a apostrofe as closing url query otherwise it will not work.

* * *
## SSH as Henry
As soon we get the shell we are presented as ruby user, scrolling thru all the folders seems like user flag is in henry's home and of course we can't read it.
I decided to list all files(included the hidden ones) and found something interesting in .bundle:
![c6825d551efc1ca60d1d2f88e7b3e5fd.png](../../_resources/c6825d551efc1ca60d1d2f88e7b3e5fd.png)

Now that we have henry's password we can ssh and grab out first flag:
![a767548d49422d63b7c91e5bc2b1f37c.png](../../_resources/a767548d49422d63b7c91e5bc2b1f37c.png)
* * *
## PRIVESC
Now comes the fun part, we want to escalate our way up to Root so starting with some standarr manual enumeration we get that henry can run ruby as sudo:
![ee523821d7530aed7141137d518a1c41.png](../../_resources/ee523821d7530aed7141137d518a1c41.png)

let's check if we have write permissions on that file in /opt:
![a8dbb6b2e85fd23b6524ce53336dbb92.png](../../_resources/a8dbb6b2e85fd23b6524ce53336dbb92.png)

We dont't but let's check source code and see what can we do more:
`henry@precious:/opt$ cat update_dependencies.rb
# Compare installed dependencies with those specified in "dependencies.yml"
require "yaml"
require 'rubygems'

# TODO: update versions automatically
def update_gems()
end

def list_from_file
    YAML.load(File.read("dependencies.yml"))
end

def list_local_gems
    Gem::Specification.sort_by{ |g| [g.name.downcase, g.version] }.map{|g| [g.name, g.version.to_s]}
end

gems_file = list_from_file
gems_local = list_local_gems

gems_file.each do |file_name, file_version|
    gems_local.each do |local_name, local_version|
        if(file_name == local_name)
            if(file_version != local_version)
                puts "Installed version differs from the one specified in file: " + local_name
            else
                puts "Installed version is equals to the one specified in file: " + local_name
            end
        end
    end
end`

In the source code is defined a file called dependecies.yml that is nowhere in the map, we may create that with a malicious ruby revershe shell instead:
![cfe48a4d3c94087f33602efd98635587.png](../../_resources/cfe48a4d3c94087f33602efd98635587.png)

Running the application show us exactly that the file is missing cause it's not using absolute paths we can definitely exploit this.
We don't have write permissions in /opt:
![e1a013e378ec92ae97860bc72c552047.png](../../_resources/e1a013e378ec92ae97860bc72c552047.png)

We need to plant this file in /tmp and then add folder to path so it will be loaded from there instead. So adding our shell in ruby:
![8e6f947243a8a99c70f180d1d5971042.png](../../_resources/8e6f947243a8a99c70f180d1d5971042.png)
Adding /tmp into $PATH:
![f06e9ce29ec6f25e4ea24fa143157cef.png](../../_resources/f06e9ce29ec6f25e4ea24fa143157cef.png)

Edit: seems like /tmp gets deleted after a minute or so..
Here i had to check for tips since i was not able to get a revshell to work and apparently i was not on the right track, the solution is callled ruby yaml deserialization: https://blog.stratumsecurity.com/2021/06/09/blind-remote-code-execution-through-yaml-deserialization/
Using this code we will be able to spawn a shell(snippet taken from the article and edited to match our IP):
![8d5bc7f80dfc78592b2e3e56e151b8ac.png](../../_resources/8d5bc7f80dfc78592b2e3e56e151b8ac.png)

And we have our last flag:
![ac21aada1382fc520ccad917a275a87a.png](../../_resources/ac21aada1382fc520ccad917a275a87a.png)

Here we could get more about it: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Insecure%20Deserialization/Ruby.md#yamlload

* * *