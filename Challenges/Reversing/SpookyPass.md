Link : [SpookyPass](https://app.hackthebox.com/challenges/SpookyPass?tab=play_challenge)

First, download the zip file and unzip it with default password, I got a folder called `rev_spookypass` containing `pass`

Let's do triage check first
```
file pass
<SNIP>
Welcome to the 
[1;3mSPOOKIEST
[0m party of the year.
Before we let you in, you'll need to give us the password: 
s3cr3t_p455_f0r_gh05t5_4nd_gh0ul5
Welcome inside!
You're not a real ghost; clear off!
;*3$"
GCC: (GNU) 14.2.1 20240805
<SNIP>
```
So I can easily detect that, `s3cr3t_p455_f0r_gh05t5_4nd_gh0ul5` is  the password needed to unlock
```
$ ./pass         
Welcome to the SPOOKIEST party of the year.
Before we let you in, you'll need to give us the password: s3cr3t_p455_f0r_gh05t5_4nd_gh0ul5
Welcome inside!
HTB{un0bfu5c4t3d_5tr1ng5}
```

Happy Hacking!@#!@#
