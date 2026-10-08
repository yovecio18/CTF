We know we have SMB and RPC so let's try to check what can we grab as informations from those services with Enum4linux:
![bd7da5cad415e81824e7b22f21decbf2.png](../../_resources/bd7da5cad415e81824e7b22f21decbf2.png)
![8139ba64f5eaf8c41e4a870129469413.png](../../_resources/8139ba64f5eaf8c41e4a870129469413.png)

And seems like we can't enumerate the service with a guest user nor with a random one, we may have to come back when we have some credentials later.

I tried manually to list for SMB shares but again, we can't do much more without a credentials..
![e8e4560ef6c1f02904bdb96c58c41f08.png](../../_resources/e8e4560ef6c1f02904bdb96c58c41f08.png)

The only map that's is working is Shares:
![caf11311a9b043c217c8a5f24b8f23c9.png](../../_resources/caf11311a9b043c217c8a5f24b8f23c9.png)

Opening the pdf file shows us a possible username and possible CVEs?
![d511d87711cdc65ce86480a5c7ca31e2.png](../../_resources/d511d87711cdc65ce86480a5c7ca31e2.png)