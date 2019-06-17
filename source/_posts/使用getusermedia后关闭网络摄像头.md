---
title: 使用getusermedia后关闭网络摄像头
date: 2019-05-08 14:02:02
categories: javascript
tags: 调用摄像头
---

使用js 的navigator.getUserMedia打开了摄像头，应该怎样关闭呢
下面是摄像头的开启和关闭
需要注意的是浏览器调用摄像头必须是安全访问的情况下才可以，即localhost或者https访问的页面才可以，否则不能获取到浏览器的getUserMedia方法。

``` javascript

<html>
  <head>
    <script>
      var localstream;

      if (navigator.mediaDevices.getUserMedia !== null) {
        var options = { 
          video:true, 
          audio:false 
        };  
        navigator.webkitGetUserMedia(options, function(stream) { 
          vid.src = window.URL.createObjectURL(stream);
          localstream = stream;
          vid.play();
          console.log("streaming");
        }, function(e) { 
          console.log("background error : " + e.name);
        }); 
      }

      function vidOff() {
        vid.pause();
        vid.src = "";
        localstream.getTracks()[0].stop();
        console.log("Vid off");
      }
    </script>
  </head>
  <body>
    <video id="vid" height="120" width="160" muted="muted" autoplay></video><br>
    <button onclick="vidOff()">vidOff!</button><br>
    <div id="div"></div>
  </body>
</html>

```

localstream.stop() 已被弃用，不再有效
实际上将 

    localstream.stop() 
    
更改为 

    localstream.getTracks()[0].stop();
    
就可以用了