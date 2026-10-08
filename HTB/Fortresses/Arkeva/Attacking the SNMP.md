Before going "crazy" on the HTTP service on port 5000 I will go thru the SNMP service on port 161 UDP and check if it can unveil something interesting about the Backend and If any hidden flag? To keep the page slim I will upload the whole log from SMTP enumeration here: [smtp.txt](../../../_resources/smtp.txt)

But from the backlog we could see one of the flags hidden in one of the backup scripts between the running services on the host:

![0e4a0db016b7b0e615c1517e0e4ca60d.png](../../../_resources/0e4a0db016b7b0e615c1517e0e4ca60d.png)

Except that we can see the list of installed packages that don't show us that much and some running scripts on /opt and /html folder that we can target as soon we get a foothold and RCE on the machine.

![a0fa013cdca16cf81ca3472d18745ed6.png](../../../_resources/a0fa013cdca16cf81ca3472d18745ed6.png)

Let's see if there is anything in here...

![17daebddc2e12a01558d82a343e21573.png](../../../_resources/17daebddc2e12a01558d82a343e21573.png)

Not on the first but what about the dev?

![98a1e3c6ff0f3c14c2205dedb538a21f.png](../../../_resources/98a1e3c6ff0f3c14c2205dedb538a21f.png)

Not much... But let's check for some default ENV path first:

![156c29721dec2824af0aa7d5fd7943d6.png](../../../_resources/156c29721dec2824af0aa7d5fd7943d6.png)

![aaaa30e2f43d2eeb5dcf441cf791008a.png](../../../_resources/aaaa30e2f43d2eeb5dcf441cf791008a.png)

And knowing the base path we can try some manual enumerations:

![5d3a7b890093a01e3e42e4f627c3cfb0.png](../../../_resources/5d3a7b890093a01e3e42e4f627c3cfb0.png)And by guessing flag.txt we could read it:

![118813877a461563f6c26d347d92f095.png](../../../_resources/118813877a461563f6c26d347d92f095.png)

But can we see if we can find any SSH keys?

![0fdf9cfa5540512626bc967ff37d1280.png](../../../_resources/0fdf9cfa5540512626bc967ff37d1280.png)

Answer: NO!