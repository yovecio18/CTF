# Introduction

You are tasked with performing a penetration test on Sidecar, involving a Windows only Active Directory environment.

Sidecar is a small Active Directory chain that contains 2 Windows machines, however, attacks are not for beginners on Active Directory Pentesting.

Sidecar is designed for penetration testers and red teamers in search of an advanced lab.

**This Red Team Operator Level 1** lab will expose players to:

- Active Directory attacks
- Shadow Credential attacks
- Kerberos attacks
- Abusing SeTcbPrivilege privilege
- .lnk file abuses.

&nbsp;

# Entry Point

As usual I am also provided 2 private IPs as entry points for this challenge:

![09f7b15d9eb7c7f09835b9b73eecb9a0.png](../../../_resources/09f7b15d9eb7c7f09835b9b73eecb9a0.png)

And I can get a quick glimpse of what is the tech stack used by the challenge as well.

![60a93211d64c13628ac982b01bd9284e.png](../../../_resources/60a93211d64c13628ac982b01bd9284e.png)

Without further due, I will move on to the IPs for further analysis.

&nbsp;