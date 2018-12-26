---
title: vue强制刷新
date: 2018-12-20 10:44:51
categories: vue
---

使用vue-router进行路由跳转刷新有时候不管用

    this.$router.push(`/user/role-manage/modify/${this.roleId}`);

可以使用    

    location.reload(); // 强制刷新