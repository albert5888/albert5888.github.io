---
layout: post
title: "[Security] Web Settings"
date: 2023-09-04 +0800
categories: Security
hidden: false
tags: 
  - "CORS"
  - "CSP"
---

## Cookie vs Session

### Cookie 
  - 敏感資料可能洩漏
 ```
 isAdmin = true
 ```
 
### Session
 - 使用id，將敏感資料放在Server
 ```
 SessionID = 123456
 ```

## Cookie Security

### 第三方Cookie ?
 - 第一方Cookie發送請求網址與當前網頁的網域一致，我們稱帶在請求上的 cookie 為第一方Cookie
 - 如果是網頁上一些置放在第三方網域底下的資源所發出的請求 (ex:追蹤程式碼)的cookie，稱為第三方Cookie
 - 只要domain相同，當作同一site

 ```
 new.yahoo.com 和 finance.yahoo.com 是同一個 site
 cats.github.io 和 dogs.github.io 不是同一個 site
 ```
- 當瀏覽器在決定能不能對某個 domain 設置 cookie 時，會參照一個清單叫做 public suffix list，出現在上面的 domain，其 subdomain 都沒辦法直接設定該 domain 的 cookie。
 - 參考 [publicsuffix](https://publicsuffix.org/)

### SameSite
 - 在設定Cookie可以附加的屬性，有三種，如下

#### SameSite=None
 - 不管是不是同一Site都可以連帶Cookie送過去

#### SameSite=Strict
 - 只有同一Site才可以連帶Cookie送過去

#### SameSite=Lax
 - 同一Site可以連帶Cookie送過去
 - 不同Site"部分"可以連帶Cookie送過去
 - 特定情況如下
```
點擊連結 <a href="...">
送出表單 <form method="GET">
背景轉譯 <link rel="prerender" href="...">
```

### HttpOnly
 - 限制Cookie只能由http存取
 - 禁止javascript存取Cookie

### SuperCookies & EverCookies

## Token Auth vs Cookie


## SOP (Same-Origin Policy)
 - 瀏覽器專屬，無須設定

### DOM 同源
  - 只有相同"源"的網站，才能存取網站的資源
  - 需要scheme（ex:http,https），domain（ex:github.io），port（ex:8000）都相同。

#### Example
    - 部分允許 (嵌入和寫入) :  \<form\> 表單送出, \<img\> 插入圖片,\<script\> 插入Script
    - 封鎖 (讀取) : XMLHttpRequest , Fetch

### Cookie 同源
  - domain 和 path 相同就視為同源
  - domain 和 subdomain 為共用
  - scheme 是否同源須設定 
  ```
  // 設定 Secure 只能用 HTTPS 傳送 (只有HTTPS同源)
  Set-Cookie: id=5888; domain=github.io; Secure
  ```

## HTTP Header
### X-Frame-Options
  - 是否允許這個網頁讓別人使用iframe (Clickjacking)
  - 例如: YouTube就不會設定X-Frame-Options，因為他要讓別人用iframe，來嵌入飲片

### CSP

### HSTS
  - 為了HTTPS，但首次載入仍會先從HTTP => HTTPS 

### CORS
  - CORS 削弱 SOP (小心CORS misconfiguration)
  - 由於SOP限制網頁請求，因此有CORS來突破SOP

### X-XSS-Protection
  - 瀏覽器內建過濾XSS

### X-Content-Type-Options
  - 建議設置 : X-Content-Type-Options: nosniff
  - 告訴瀏覽器不要猜測所提供內容的MIME類型，而是信任"Content-Type"標頭
  - 例如: 內容是 \<script\>，但是Content Type是plain/text，就當成是plain/text，防止XSS



## JSONP