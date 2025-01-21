---
layout: post
title: "[Security] Subdomain-Takeover"
date: 2023-08-06 +0800
categories: Security
hidden: false
---

## 原理

平常我們通過DNS解析會取得一個IP

但我們有時也會設定CNAME，將結果設定為一個domain

通常漏洞的發生會在subdomain設定一個CNAME指定在一個雲端服務上(AWS,Azure,GCP等)

然而之後因為某些原因將雲端服務的主機刪除，進而導致CNAME指向一個空的位置

這時我們就可以趁虛而入註冊一個相同名稱的雲端服務的主機，導致Subdomain-Takeover

---

例如 :

subdomain.albert5888.tw 使用CNAME指向 cloudalbert5888.azurewebsites.net

之後我們將雲端主機上的cloudalbert5888.azurewebsites.net取消註冊

在CNAME沒有刪除的情況下，其他使用者趁機註冊cloudalbert5888.azurewebsites.net

這時subdomain.albert5888.tw就會顯示其他人的內容

---

## 工具檢測

這個工具[Github-sub404](https://github.com/r3curs1v3-pr0xy/sub404)可以幫我們協助發現這些漏洞

透過subfinder、sublister列舉出subdomain，再透過DNS查詢是否指向空的CNAME

但是這項工具有些誤差，就算列舉出有問題的subdoomain，還是需要實際在網路上驗證是否可以註冊

![](/images/2023/08/6L8zSLW.png)

---

## 相關Bug Bounty Reports

 - Starbucks

    [https://hackerone.com/reports/276269](https://hackerone.com/reports/276269)

 - Uber

    [https://hackerone.com/reports/219205](https://hackerone.com/reports/219205)

 - Snapchat

    [https://hackerone.com/reports/154425](https://hackerone.com/reports/154425)

 - Twitter

    [https://hackerone.com/reports/32825](https://hackerone.com/reports/32825)

---

## Ref

 - [Wikipeda - CNAME](https://zh.wikipedia.org/zh-tw/CNAME%E8%AE%B0%E5%BD%95_)

 - [Github - sub404](https://github.com/r3curs1v3-pr0xy/sub404)
