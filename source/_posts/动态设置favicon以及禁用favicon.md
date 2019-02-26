---
title: 动态设置favicon以及禁用favicon
date: 2019-02-26 08:57:38
categories: javascript
---

### 动态加载企业logo和全名

    (function() {
      var link =
        document.querySelector("link[rel*='icon']") ||
        document.createElement('link');

      link.type = 'image/x-icon';
      link.rel = 'shortcut icon';

      // 从localstorage里面获取客户信息
      let customerInfo = JSON.parse(localStorage.getItem('userInfo')); 
      // 客户logo
      let customerIcon =
        customerInfo &&
        customerInfo.platformCustomerModel &&
        customerInfo.platformCustomerModel.customerIcon
          ? customerInfo.platformCustomerModel.customerIcon
          : null;

      // 客户全名
      let customerTitle =
        customerInfo &&
        customerInfo.platformCustomerModel &&
        customerInfo.platformCustomerModel.fullName
          ? customerInfo.platformCustomerModel.fullName
          : '城市盾牌企业门户系统';

      let path0 = window.location.pathname.split('/')[1];
      if (path0.indexOf('login') !== -1) {
        //如果是登陆页面
        link.href = 'static/favicon.ico';
      } else {
        //如果不是登陆页面
        link.href = customerIcon
          ? '/cityshield-pic/' + customerIcon
          : 'static/favicon.ico';
      }
      document.getElementsByTagName('head')[0].appendChild(link);
      document.title = customerTitle;
    })();

### 禁止favicon

    <link rel="icon" href="data:;base64,=">
    // 或者详细一点
    <link rel="icon" href="data:image/ico;base64,aWNv">