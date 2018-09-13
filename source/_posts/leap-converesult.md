---
title: Js直接获取返回值的数组
tags:
  - LEAP
  - ConvertResult 
categories:
  - [LEAP, 客户端]
date: 2018-09-13 15:32:23
thumbnail: http://paz1myrij.bkt.clouddn.com/favicon.png
---

- `this.request`获取`EntityBeanSet`的返回值 
![](http://paz1myrij.bkt.clouddn.com/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744e633739677931667265656e65633332726a333137323072777767312e6a7067.jpeg)

- `LEAP.ConvertResult(res)` 可以直接获取返回的数组
![](http://paz1myrij.bkt.clouddn.com/111.jpeg)

- `LEAP.ConvertResult` 源码

```javaScript
/**
 * 转换ResultSet为object[]
 * 
 * @param {ResultSet}
 *            object
 * @return {object[]}
 */
LEAP.convertResult = function(object)
{
    if (object != null && object.javaClass != null)
    {
        if (object.javaClass == commfields.rsc)
        {
            if (object == null || object.result == null || object.result.length == 0)
                return null;

            var _objs = new Array();
            var l = object.result.length;
            var hascvs = object.codeValues != null;
            for(var i = 0;i < l;i++)
            {
                var _obj = new Object();
                var cells = object.result[i];
                if (!cells)
                    continue;
                var ll = object.metaData.length;
                for(var j = 0;j < ll;j++)
                {
                    var _md = object.metaData[j].name;
                    var _type = object.metaData[j].type;
                    var _value = cells[j];
                    if (hascvs)
                    {
                        var _tt = object.codeValues[i];
                        if (_tt != null)
                        {
                            _cv = _tt[j]
                            if (_cv != null)
                            {
                                _obj[commfields.rsccv + _md] = _cv;
                            }
                        }
                    }

                    //|| _type == 2 || _type == 3
                    if (_type == -5 || _type == -6 || _type == -7 || _type == 4 || _type == 5 || _type == 6
                            || _type == 7 || _type == 8)
                    {
                        if (String.isEmpty(_value))
                            _value = null;
                        else _value = LEAP.tonum(_value);
                    }
                    _obj[_md] = _value;
                }
                _objs.add(_obj);
            }
            return _objs;
        }
        else if (object.javaClass == 'com.longrise.LEAP.Base.Objects.EntitySet')
        {
            return object.result;
        }
        else if (object.javaClass == 'com.longrise.LEAP.Base.Objects.EntityBeanSet')
        {
            if (object == null || object.result == null || object.result.length == 0)
                return null;

            var _objs = new Array();
            var l = object.result.length;
            var hascvs = object.codeValues != null;
            if (hascvs)
            {
                for(var i = 0;i < l;i++)
                {
                    var _obj = object.result[i];
                    var cells = object.result[i];
                    if (!cells)
                        continue;
                    var ll = object.metaData.length;
                    for(var j = 0;j < ll;j++)
                    {
                        var _md = object.metaData[j].name;
                        var _type = object.metaData[j].type;
                        var _value = cells[_md];
                        if (hascvs)
                        {
                            var _tt = object.codeValues[i];
                            if (_tt != null)
                            {
                                _cv = _tt[j]
                                if (_cv != null)
                                {
                                    _obj[commfields.rsccv + _md] = _cv;
                                }
                            }
                        }

                        if (_type == -5 || _type == -6 || _type == -7 || _type == 4 || _type == 5
                                || _type == 6 || _type == 7 || _type == 8)
                        {
                            if (String.isEmpty(_value))
                                _value = null;
                            else _value = LEAP.tonum(_value);
                        }
                        _obj[_md] = _value;
                    }
                    _objs.add(_obj);
                }
                return _objs;
            }
            else return object.result;
        }
    }
    if (object != null && object.result != null)
        return object.result;
    return object;
}
```


