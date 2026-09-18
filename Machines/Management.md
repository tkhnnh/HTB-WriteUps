Link: [Management](https://app.hackthebox.com/machines/Management?sort_by=created_at&sort_type=desc)
Difficulty: Easy
OS: Linux

# Recon
Start the reconaisance phase with `nmap` to find the existing open ports on the target
```
$ nmap -sV -sC -vv -p- -A -T4  10.129.20.116
<SNIP>
PORT      STATE SERVICE     REASON         VERSION
22/tcp    open  ssh         syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.19 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBN9Ju3bTZsFozwXY1B2KIlEY4BA+RcNM57w4C5EjOw1QegUUyCJoO4TVOKfzy/9kd3WrPEj/FYKT2agja9/PM44=
|   256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIH9qI0OvMyp03dAGXR0UPdxw7hjSwMR773Yb9Sne+7vD
80/tcp    open  http        syn-ack ttl 63 nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to https://10.129.20.116/
443/tcp   open  ssl/http    syn-ack ttl 63 nginx 1.24.0 (Ubuntu)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=management.htb/organizationName=Management Managed Services Ltd
| Subject Alternative Name: DNS:management.htb, DNS:*.management.htb
| Issuer: commonName=management.htb/organizationName=Management Managed Services Ltd
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-06-02T01:21:44
| Not valid after:  2126-05-09T01:21:44
| MD5:     340b 4117 daca 4dc5 ba7e 6724 7406 4f09
| SHA-1:   2a4c d0c3 53fb 774f a3fe d6df 6e5f 0309 5911 670c
| SHA-256: 408e ab66 04ba 0d8c ae55 c9c7 6d7c 860f 03c1 72ea fff1 fa64 8322 bc04 ab63 23a8
| -----BEGIN CERTIFICATE-----
| MIIDlzCCAn+gAwIBAgIUFd8+MwDDYS1HjKuI8X5R7vL7oBcwDQYJKoZIhvcNAQEL
| BQAwQzEXMBUGA1UEAwwObWFuYWdlbWVudC5odGIxKDAmBgNVBAoMH01hbmFnZW1l
| bnQgTWFuYWdlZCBTZXJ2aWNlcyBMdGQwIBcNMjYwNjAyMDEyMTQ0WhgPMjEyNjA1
| MDkwMTIxNDRaMEMxFzAVBgNVBAMMDm1hbmFnZW1lbnQuaHRiMSgwJgYDVQQKDB9N
| YW5hZ2VtZW50IE1hbmFnZWQgU2VydmljZXMgTHRkMIIBIjANBgkqhkiG9w0BAQEF
| AAOCAQ8AMIIBCgKCAQEA4Ifdrz0nHuGG0rjZE/Vc0Hf1iHHR1mk9j2IQPnHBHuZK
| c+ofamIJwOA4LsYyxW4k2E8XiXx0UGZ5oc8v26zNCg3xNk8UmMDt9cpFtNou393v
| VAB0V8ZANNk0LAzja/kzq8MBtOmcmRfQyUnxbTep8MloOdHFQIxWK5Ok0ejGtg2b
| AHTWF+R3cs4XEzOR/u2dhbB6+yR1xxEBxYpSg7mb2yDXy754NFe1pWabrZQBXmVk
| 3RHsCLvsZaHS4djBXW5+G7h6NUcFT6ITxQICD20eFDT3R5G7TZ4/GAs8co2x2GpI
| VRUmlj+JEuqi8U7H6DUR9dLLqAoF0dGocQGPP639vQIDAQABo4GAMH4wHQYDVR0O
| BBYEFB0XvLYnvemOLJArfcYtqq9rkuSHMB8GA1UdIwQYMBaAFB0XvLYnvemOLJAr
| fcYtqq9rkuSHMA8GA1UdEwEB/wQFMAMBAf8wKwYDVR0RBCQwIoIObWFuYWdlbWVu
| dC5odGKCECoubWFuYWdlbWVudC5odGIwDQYJKoZIhvcNAQELBQADggEBACMea0UH
| omu+dhxnrcLZ0bxrzd5n8ZGnHucFQCtCuvKFira5FwcX+R9BGaPAVe43gzMpXwwn
| JoUWW36/QTkHC/eEu10xavgYXJsqZFG6g+rq3orVDrO37pU7ILHlIW5cITs073sw
| +DHHwcMTkvfvcqajO6nJYf/RL7NFISBvxSVBVmsWlRKj4av0qiKPeEchiWYMkxoA
| F0C80/WliBqw1c37G/acZTnh4gMKK+jE65EpAXDD6MZmoJ/pkSSPoOkv0Hi+RyCj
| NFHFNO8Y/Ut94WiUPMDS+jMnfK3+0elKEU831oJ9RMajVFu9P5n2KDIIaiaQ1VTw
| z1eFKtcjA8Ob2pE=
|_-----END CERTIFICATE-----
|_http-server-header: nginx/1.24.0 (Ubuntu)
| tls-alpn: 
|   http/1.1
|   http/1.0
|_  http/0.9
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to https://management.htb/
1689/tcp  open  java-rmi    syn-ack ttl 63 Java RMI
| rmi-dumpregistry: 
|   org.opends.server.protocols.jmx.client-unknown
|     javax.management.remote.rmi.RMIServerImpl_Stub
|     @127.0.1.1:43639
|     extends
|       java.rmi.server.RemoteStub
|       extends
|_        java.rmi.server.RemoteObject
4444/tcp  open  ssl/krb524? syn-ack ttl 63
| fingerprint-strings: 
|   LDAPSearchReq: 
|     0<0:
|     objectClass1+
|     ds-root-dse
|_    ds-cfg-root-dse-backend0
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=sso.management.htb/organizationName=Administration Connector RSA Self-Signed Certificate
| Issuer: commonName=sso.management.htb/organizationName=Administration Connector RSA Self-Signed Certificate
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-06-02T01:23:59
| Not valid after:  2046-05-28T01:23:59
| MD5:     f9f7 2d79 688a 7e18 848d 3c4a e5a7 928a
| SHA-1:   587d 40bb 52b3 4e25 fcf1 4237 8eda 45ad 7f9e 7e3f
| SHA-256: a213 911c 7e66 33ff b4d4 8daf 6a2e 1ab4 e535 4462 b798 19b4 626d 69b5 871d e591
| -----BEGIN CERTIFICATE-----
| MIIDOTCCAiGgAwIBAgIJAIFW7GaOYtM6MA0GCSqGSIb3DQEBCwUAMFwxGzAZBgNV
| BAMMEnNzby5tYW5hZ2VtZW50Lmh0YjE9MDsGA1UECgw0QWRtaW5pc3RyYXRpb24g
| Q29ubmVjdG9yIFJTQSBTZWxmLVNpZ25lZCBDZXJ0aWZpY2F0ZTAeFw0yNjA2MDIw
| MTIzNTlaFw00NjA1MjgwMTIzNTlaMFwxGzAZBgNVBAMMEnNzby5tYW5hZ2VtZW50
| Lmh0YjE9MDsGA1UECgw0QWRtaW5pc3RyYXRpb24gQ29ubmVjdG9yIFJTQSBTZWxm
| LVNpZ25lZCBDZXJ0aWZpY2F0ZTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoC
| ggEBAKbUhh7nmQ/EAOdaHrUxKFnfyiNmPP7amMAHikYiJ2A3GtZ+PsSX+adZyOBP
| GvSpa/i9jiUdkLQ2VMCGH/RTu7QFBLYXqj8g2RmRWuyPYVeizWcOOURrCAVr0BZu
| hBqrlrU1kN4Rsmhx6vIBSEL0fM6bW/yFt8L0oc2jRDQPV7ufwger9TesK0KyhCz4
| WcR1LBCSe+DLFtPvYh+I4vAUp/CBaB4JUyzlhQscpVC/Pm2Yz0ZNLx4QXn9Mifg9
| 2Cre/pj0h+Uj8/H/ebAyusrz7E1ss2HLrYTdDpo3J4BNYNA5g2J+t/S7E7jLwQDX
| vOcp9hDP0CwN8l2RSw3EH69LD/MCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAZAaF
| Pw7cSMv1V2oedWG100naJt7hrd9TMzqGCV/KDl8vaG2ZCR//yhqsNKVHfxc8sSMH
| zMAEUA8T3wMheHQcwAnCKPHPkQmlvGtpZgsqPuMYNx4wwdDhLh8kcwtbDY4sJbwL
| mNrY7x6wovcrMcs74x4AhoxLkVJ8AQBNW3FcDp4GCtmORWhIfpzKhcrtc9zgEAdw
| dilIWPT688ZNQ3T+uPnw0VybpZTvO/PRKE2qidFZ/TTjsI7SK6CEWl09ayQKJcVW
| UAs4R66Vd/nLg+FxL18vpivyvDJIs0u8GEy3YuTKlK+mT+GVOY6F306n2EDrAbHL
| UOOjtnxeafnwwOSBnw==
|_-----END CERTIFICATE-----
43639/tcp open  java-rmi    syn-ack ttl 63 Java RMI
50389/tcp open  ldap        syn-ack ttl 63 (Anonymous bind OK)
<SNIP>
```

Open ports:
- 22 : SSH
- 80 : HTTP
- 443 : SSL/HTTP
- 1689 : java-rmi
- 4444 : ssl/krb524
- 43639 : java-rmi
- 50389 : ldap

## Port 80 : HTTP
<img width="1906" height="838" alt="Screenshot 2026-09-18 160746" src="https://github.com/user-attachments/assets/747001bb-b141-45af-ab01-066e7fb8cddd" />

Thank to requesting the HTTP service via the browser, I need to add domain name into this IP in order to access the service of the target
just add this in the `/etc/hosts` file
```
<IP> management.htb
```
This is the target HTTP service looks like
<img width="1897" height="840" alt="Screenshot 2026-09-18 161200" src="https://github.com/user-attachments/assets/d7ba91d4-a189-4fda-b0c8-361a2ccf274e" />

I have to add subdomain `sso.management.htb` in order to access `Client login` function
```
<IP> management.htb sso.management.htb
```
<img width="1907" height="837" alt="Screenshot 2026-09-18 161604" src="https://github.com/user-attachments/assets/48496ebd-32da-41f9-8eb7-89f50e55eef2" />

