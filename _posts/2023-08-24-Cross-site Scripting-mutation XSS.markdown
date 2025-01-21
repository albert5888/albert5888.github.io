---
layout: post
title: "[Security] Cross-site Scripting (mutation XSS)"
date: 2023-08-24 +0800
categories: Security
hidden: false
tags:
  - "XSS"
  - "Cross-site Scripting"
---

## Parser

在不同Browser中(Ex: safari / chrome / firefox ...)，解析出來HTML/JS也可能有所不同，可能同樣的XSS，只能發生在特定的Browser當中。

## HTML parser (Chrome)

 - html parse (開頭div)

    ![](/images/2023/08/ZyxTtrM.png){:h="80%" w="80%"}

 - </div>設為script title, script和div也自動閉合

    ![](/images/2023/08/UGVORnM.png){:h="80%" w="80%"}

## JS parser (Chrome)

 - js parse (開頭script)

    ![](/images/2023/08/92W4gdQ.png){:h="80%" w="80%"}

 - script在head, <div title=" 被放在script

    ![](/images/2023/08/kiguYfr.png){:h="80%" w="80%"}


## Sanitize (消毒)

消毒是指在使用者輸入的字當中，過濾掉有害的東西。

EX:

```html
<img src=x onerror=alert()>
<script>
```
(消毒中)
```html
<img src=x>
```

## DOMPurifiy

 - 用來過濾(消毒)使用者輸入的不安全的html，刪除可能導致XSS的元素

 - 由於每個瀏覽器的parser不同，因此會在Client進行消毒。

[cure53/DOMPurify - Github](https://github.com/cure53/DOMPurify)


## mutation XSS

原因就發生在 innerHTML，攻擊者注入HTML，透過DOMPurifiy過濾後，但是因為沒過濾好，反而從安全形式轉換為不安全形式。

## mutation XSS in Google

\<noscript\>在啟用JS和停用JS的parser是不同的，因此在Sanitize(消毒)的時候是停用JS，當然在Browser當中是啟用的，因此就造成解析不同，造成XSS。

![](/images/2023/08/sHI7nI6.png){:h="80%" w="80%"}

[XSS on Google - Youtube](https://www.youtube.com/watch?v=lG7U3fuNw3A&ab_channel=LiveOverflow)
