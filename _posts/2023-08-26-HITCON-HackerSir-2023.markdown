---
layout: post
title: "[CTF] HackerSir 靶機 in HITCON 2023"
date: 2023-08-26 +0800
categories: CTF
hidden: false
---


## 前言
這個Lab是在HITCON攤位上看到的，當時沒時間解，回來解解看，順便寫個writeup。

## Writeup

### 任意讀取
- 這是商品的介面 (抱枕好可愛XD)
    ![](/images/2023/08/ewEGoFP.png){:h="80%" w="80%"}

- 這是圖片的連結，可以任意讀取在?file=的地方
    ```url
    https://lab.hackersir.org/uploads?file=b87eedddb23f4993bbc8f25a0def05d2
    ```
- 在搜尋的地方輸入'，就能在報錯頁面看到source code位置
    ![](/images/2023/08/a5pEizt.png){:h="80%" w="80%"}


- 有這些source code

    ```url
    https://lab.hackersir.org/uploads?file=../../app/main.py
    https://lab.hackersir.org/uploads?file=../../app/api/cart.py
    https://lab.hackersir.org/uploads?file=../../app/api/item.py
    ```

- 有一個像flag
    ![](/images/2023/08/M8y0Ukp.png){:h="80%" w="80%"}

### SQLI

- 查看 item.py，可以sqli
    !![](/images/2023/08/4E2jotG.png){:h="80%" w="80%"}

- 嘗試sqli
    ![](/images/2023/08/mdYrOtg.png){:h="80%" w="80%"}

- 成功
    ![](/images/2023/08/lV3RO3w.png){:h="80%" w="80%"}

## End
我覺得是一個有趣的小靶機，但是不知道還有沒有其他可以攻擊的地方。
