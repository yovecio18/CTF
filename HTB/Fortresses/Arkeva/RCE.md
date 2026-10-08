Now that we have a Console running with a valid pincode, we can potentially run some commands on the console with a specially wrapped commando in python format and get exceuted them in the backend server. It would be something similar:

![b9c1b003b3f311855369c30ad356cd4b.png](../../../_resources/b9c1b003b3f311855369c30ad356cd4b.png)

This is extremely dangerous cause it can give us our first foothold into machine and gain a working reverse shell to interact with victim!

![6ef2a57ada859ecdc1ef9c7ee5170e0a.png](../../../_resources/6ef2a57ada859ecdc1ef9c7ee5170e0a.png)

Indeed it's working, but before trying to gain a Reverse shell I will try to do some manual enumeration:

![140221f9005e303ff7d1b769890529ea.png](../../../_resources/140221f9005e303ff7d1b769890529ea.png)

And magically we found another flag, plus we can see the .ssh folder, but can we read the id_rsa?

![9bef6b7aaeb0ceadfaac5cb103871e75.png](../../../_resources/9bef6b7aaeb0ceadfaac5cb103871e75.png)

Nah, this means we have to craft a Reverse shell and gain a revshell!

We know that Console handles directly python code, which means it should be easy for us to get a Revshell by executing directly some python code:

![1d5393e2fa06810712b664ee0351ea9d.png](../../../_resources/1d5393e2fa06810712b664ee0351ea9d.png)

And magically we have a shell:

![3c2e303b06c6c0815cc0cbee40edff3a.png](../../../_resources/3c2e303b06c6c0815cc0cbee40edff3a.png)

Now we can move forward with enumeration!

* * *