---
layout: post
title: "[Security] Cross-site Scripting (DOM Clobbering)"
date: 2023-08-25 +0800
categories: Security
hidden: false
tags:
  - "XSS"
  - "Cross-site Scripting"
---

## What is DOM

- 將HTML parser成一個樹狀結構

![](/images/2023/08/JtpEUrG.png){:h="80%" w="80%"}

[Day03-深入理解網頁架構：DOM - ithome](https://ithelp.ithome.com.tw/articles/10202689)

## DOM Clobbering

 - HTML元素可以有一個ID，而且JS可以進行存取，DOM增加了html和javascript的交互。

![](/images/2023/08/W7TFfz3.png){:h="80%" w="80%"}

### HTML元素可以影響javascript

- 確認是否有isAdmin

![](/images/2023/08/XznMtsc.png){:h="80%" w="80%"}

- 新增一個html元素id為isAdmin

![](/images/2023/08/iARCEAd.png){:h="80%" w="80%"}

- 假如已經有給定值，則無法影響

![](/images/2023/08/ePfcuo2.png){:h="80%" w="80%"}
