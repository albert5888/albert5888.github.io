---
layout: post
title: "[CTF] Devcore-Wargame-2023 Writeup"
date: 2023-09-19 +0800
categories: CTF
hidden: false
---

## What's my IP
 - 透過 ```1.1.1.1', 'test') -- -```SQL injection
 - 再用information_schema慢慢撈出db name, table, columns, FLAG
 - 輸出有長度限制，可以用substr之類的
![](/images/2023/09/vK1GVc5.png){:h="80%" w="80%"}

## Submit flag
 - Race Condition (看source)
 - packet送快一點就對了
 - 裡面大概會像這樣

 ![](/images/2023/09/rN7MG3E.png)

 - source有```sleep(2)```成功機率會高一點，也可以看到就是那邊有時間差

```php
if ((bool) $row) {
        $session['flash'] = 'You already submitted this flag.';
        save_session($session);
        header('Location: /index.php');
        exit();
    }

    sleep(2);

    $stmt = $pdo->prepare('INSERT INTO submit (user, flag) VALUES (?, ?)');
    $stmt->bindParam(1, $user);
    $stmt->bindParam(2, $flag);
    if ($stmt->execute()) {
        $session['flash'] = 'You successfully submit the flag.';
        save_session($session);
        header('Location: /index.php');
        exit();
    } else {
        $session['flash'] = 'Submit error.';
        save_session($session);
        header('Location: /index.php');
        exit();
    }

```

## Wargame-2021

### flag1

- 封面的圖片位置是base64，用 ```./../../../../etc/passwd``` base64 encode就可以讀取，但提示要讀取php source code
```
http://127.0.0.1:58080/image.php?id=Li8uLi8uLi8uLi8uLi9ldGMvcGFzc3dk
```
![](/images/2023/09/JTjGYAX.png)

 - /proc/$(id) 是用來看某個PID下的內核的東西，位於memory裡面
 - 其中 /proc/self ，是用目前的process來看，因此不同的process看的會不同
 - /proc/$(id)/cwd 是當前process執行的目錄連結
 - 所以可以透過 /proc/self/cwd 來連結到目前執行process的目錄 ， /proc/self/cwd/include.php

![](/images/2023/09/h6yPiM4.png)

### flag2

 - 將sig轉成array，跳過驗證讀取其他編號
 ![](/images/2023/09/6uO1jRo.png)

 ### flag3

 - print.php 存在sqli
 ![](/images/2023/09/62LkzFg.png)

 - 確認sqli
 ![](/images/2023/09/L4lIrrU.png)

 - 撈出FLAG
 ![](/images/2023/09/HV2C3S2.png)

```
table: backend_user
id: 1
username: admin
password: u=479_p5jV:Fsq(2
description: The administrator ( DEVCORE{no.3_sql_injection_my_passw0rd_is_y0urs} ).
```

### flag4

   - 在image.php可以讀取/proc/mounts查看mount資訊

     ![](/images/2023/09/dl61V31.png)

   - 發現有```/b8ch3nd```，後台的網址```http://127.0.0.1:58080/b8ck3nd/index.php``` (我也不知道為什麼是/b8ck3nd/index.php)，打開網址後就會redirect回首頁
   - 用XFF header偽造，就可以進入登入頁面，就可以用flag3的帳密登入

     ![](/images/2023/09/Cj24Gxn.png)

### flag5

  - 有圖片上傳的位置```/b8ck3nd/upload.php```，上傳後會像這樣的位置```http://127.0.0.1:58080/image.php?id=aHBfbTI4M2Zkdy5qcGc=```
  - 可以上傳php，但是要找怎麼上傳到其他地方
  - 用image.php看source code，位置```/usr/share/nginx/b8ck3nd/upload.php```，rename和folder可以上傳到其他地方

    ![](/images/2023/09/2AXtVBy.png)

  - 加上rename和folder，就可以上傳到其他地方

    ![](/images/2023/09/CerZ7f2.png)

  - 這時會先有一個flag

    ![](/images/2023/09/ILZkjSa.png)

### flag6

 - 上傳shell後，要怎麼RCE
 - 在source code ， 可以看到include.php會引入 langs/ $_SESSION['lang'] .php

    ![](/images/2023/09/L9qFl1Y.png)

 - 這時只要將$_SESSION['lang']指定為 ```../../../../../tmp/nice```，就會執行langs/../../../../../tmp/nice.php
 - 那麼在生成再上傳一個```/tmp/sess_hacked```，指定$_SESSION['lang']為 ```../../../../../tmp/nice```，再指定PHPSESSID到hacked

   ![](/images/2023/09/cnzeOX8.png)
