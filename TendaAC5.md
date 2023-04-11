## Basic Information

Vendor:Tenda (https://www.tenda.com.cn/default.html)

product:AC5(https://www.tenda.com.cn/product/AC5.html)

version:V15.03.06.28(https://www.tenda.com.cn/download/detail-2740.html) 

type:Arbitrary Remote Command Execution 

author:yanbushuan

## Vulnerability description and trigger location

I found an Arbitrary Command Execution vulnerability in the router's web server-- /bin/httpd of squashfs filesystem. While processing the mac parameters for a post request(when an attacker accesses ip/goform/WriteFacMac), the value is directly passed to doSystem, which causes a RCE. The details are shown below:

