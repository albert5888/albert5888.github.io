---
layout: post
title: "[Security] Prototype Pollution"
date: 2023-09-04 +0800
categories: Security
hidden: false
tags:
  - "JS"
---

## ProtoType

Javascript的Object範例

```javascript
const user={
    username : "albert",
    number   :  5888
}

console.log(typeof(user))  //object
```

可以用.或是['']取值

```javascript
user.username     // "albert"
user['number']    // 5888
```

可以看到.toString可以轉成字串，但是這個fuction是在哪裡呢?

```javascript
const array1 = ["cat","dog"];
console.log(array1.toString());  //"cat,dog"
```

JS有一個屬性叫做__proto__，他是假如在這個Object找不到，就往上找的位置。

![](/images/2023/09/GEQpFMa.png)

他們的根都是NULL

```javascript
"".__proto__                        // String.prototype
"".__proto__.__proto__              // Object.prototype
"".__proto__.__proto__.__proto__    // null
```

![](/images/2023/09/oQ5f72k.png)


## Protect

錯誤使用 defineProperty (沒設定value)
```javascript
let user = {username:"admin"};
Object.defineProperty(user,'username', {configurable:false,writable:false} ); //protect
user.username = 'hacker';    //直接修改值
console.log(user.username);  //admin
```

使用Prototype pollution
```javascript
Object.prototype.value='hacker'; //pollution
let user = {username:"admin"};
Object.defineProperty(user,'username', {configurable:false,writable:false} ); //protect
console.log(user.username);  //hacker
```

正確使用
```javascript
Object.prototype.value='hacker'; //pollution failed

let user = {};
Object.defineProperty(user, 'username', {
   value: 'admin',
   configurable:false,
   writable:false
})

console.log(user.username)

```


Ref: [https://portswigger.net/research/widespread-prototype-pollution-gadgets](https://portswigger.net/research/widespread-prototype-pollution-gadgets)
