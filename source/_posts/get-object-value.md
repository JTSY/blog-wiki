---
title: 获取数组的key & values
tags:
  - 数组
  - values
categories:
  - [LEAP, 客户端]
thumbnail: 'http://paz1myrij.bkt.clouddn.com/favicon.png'
date: 2018-09-13 15:47:26
---

```js
/**
 * 获取数组的key
 * @param object
 * @returns
 */
function getObjectKeys(object) {
 var keys = [];
 for (var property in object)

keys.push(property);
 return keys;

}

/**
 * 获取数组的values
 * @param object
 * @returns
 */
function getObjectValues(object){
 var values = [];
 for (var property in object)

values.push(object[property]);
 return values;

}
```

