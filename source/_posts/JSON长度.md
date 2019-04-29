---
title: JSON长度
date: 2017-09-01 16:38:28
categories: javascript
tags: JSON
---

JSON没有length属性

``` js

        var json = {name:"xiaoming",sex:"male"}

        function getJsonLength(jsonData){

            var jsonLength = 0;

            for(var item in jsonData){

                jsonLength++;

            }

            return jsonLength;

        }

        getJsonLength(json);
        //2

```        