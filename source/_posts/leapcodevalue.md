---
title: 结果集遍历Code值
tags:
  - codevalue
categories:
  - [LEAP, 服务端]
thumbnail: 'http://paz1myrij.bkt.clouddn.com/favicon.png'
date: 2018-09-17 11:46:51
---

- 首先获取所有的代码值

``` java
leapcodevalue[] sexvalues = CodeTypeCache.getInstance().getCodeValues("ltpujnjs_sex");
```

- 遍历结果集

```java
 for (EntityBean data : datas)
{
    String sex = data.getString("sex");
        if (StringUtils.isNoneEmpty(sex)) 
        {
            data.set("cnsex", LTPUjnjs_getCodevalue(sexvalues, sex));
        }
 }
```

- 将结果集中的值带入遍历
```java
 /**
  * values 所有的代码值
  * code 结果集中的值
  */
 private String LTPUjnjs_getCodevalue(leapcodevalue[] values, String code)
    {
        if (values != null && StringUtils.isNoneEmpty(code)) 
        {
           for (leapcodevalue value : values) 
           {
               if (code.equals(value.getcodeid())) 
               {
                  return value.getcodevalue(); 
               }
           }
        }
        return "";
    }
```

