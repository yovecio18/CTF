From the intro we have so far:

| Allowed IP/URL/OTHER |     |
| --- | --- |
| **10.10.110.0/24** | Entry point subnet |
| **172.168.1.0/24** | Internal Subnet accessible from NIX01 |
| **172.16.2.0/24** | Internal ADMIN subnet accessible from DC01 and other different machines. |

| Denied IP/URL/OTHER |     |
| --- | --- |
| **10.10.110.2** | Firewall (probably used by AWS backbone) |

We must follow this scoping given from the provider, I will move forward with enumeration.