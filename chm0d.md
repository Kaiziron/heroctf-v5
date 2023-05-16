# Chm0d [227 solves] [50 points]

The credentials for SSH are given : `user:password123`

Just login to the machine and we will see flag.txt :
```
user@c84592c81e1decc6a402f8023b1e7264:/$ ls -la
total 84
drwxr-xr-x   1 root root 4096 May 14 08:49 .
drwxr-xr-x   1 root root 4096 May 14 08:49 ..
-rwxr-xr-x   1 root root    0 May 14 08:49 .dockerenv
drwxr-xr-x   1 root root 4096 May 12 23:14 bin
drwxr-xr-x   2 root root 4096 Apr  2 11:55 boot
drwxr-xr-x   5 root root  340 May 14 08:49 dev
drwxr-xr-x   1 root root 4096 May 14 08:49 etc
----------   1 user user   40 May 12 23:14 flag.txt
drwxr-xr-x   1 root root 4096 May 12 23:13 home
drwxr-xr-x   1 root root 4096 May 12 23:14 lib
drwxr-xr-x   2 root root 4096 May  2 00:00 lib64
drwxr-xr-x   2 root root 4096 May  2 00:00 media
drwxr-xr-x   2 root root 4096 May  2 00:00 mnt
drwxr-xr-x   2 root root 4096 May  2 00:00 opt
dr-xr-xr-x 300 root root    0 May 14 08:49 proc
drwx------   2 root root 4096 May  2 00:00 root
drwxr-xr-x   1 root root 4096 May 14 08:50 run
drwxr-xr-x   1 root root 4096 May 12 23:14 sbin
drwxr-xr-x   2 root root 4096 May  2 00:00 srv
dr-xr-xr-x  13 root root    0 May 12 23:55 sys
drwxrwxrwt   1 root root 4096 May 12 23:14 tmp
drwxr-xr-x   1 root root 4096 May  2 00:00 usr
drwxr-xr-x   1 root root 4096 May  2 00:00 var
```

We own flag.txt but have no permission to read it, however the chmod binary also does not have execute permission :
```
user@c84592c81e1decc6a402f8023b1e7264:/$ ls -l /bin/chmod
---------- 1 root root 64448 Sep 24  2020 /bin/chmod
```

So we can just transfer a chmod binary from our machine to the challenge machine with `scp`, make sure to set the execute permission of the chmod binary before trasnferring, so `scp` will preserve the execute permission

```
scp -P 14233 ./chmod user@dyn-06.heroctf.fr:/tmp/chmod
```

```
user@c84592c81e1decc6a402f8023b1e7264:/tmp$ ls -la
total 72
drwxrwxrwt 1 root root  4096 May 14 08:54 .
drwxr-xr-x 1 root root  4096 May 14 08:49 ..
-rwxr-xr-x 1 user user 64448 May 14 08:54 chmod
```

The execute permission is preserved, so we can just use it to change the permission of /flag.txt and read it

```
user@c84592c81e1decc6a402f8023b1e7264:/tmp$ ./chmod 777 /flag.txt ;cat /flag.txt 
Hero{chmod_1337_would_have_been_easier}
```
