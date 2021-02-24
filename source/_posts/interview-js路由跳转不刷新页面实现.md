---
title: js路由跳转不刷新页面实现
date: 2020-02-02 13:18:39
categories: javascript
tags: js基础
---

利用 history API： state, pushState, replaceState。

## history.state

当前 url 的状态信息。如果当前 url 不是通过 pushState, replaceState 跳转过来的，那么 history.state 就是 null。


## history.pushState(state, title, url)

将当前 URL 和 history.state 加入到 history 中，并用新的 state 和 URL 替换当前。不会造成页面刷新。
state：与要跳转到的URL对应的状态信息。
title：当前大多数浏览器都忽略此参数，但是将来可能会使用它。 在此处传递空字符串应该可以防止将来对方法的更改。 或者，您可以为要跳转到的地址传递简短标题。 如果需要更改标题，可以使用document.title。
url：要跳转到的URL地址，不能跨域。


## history.replaceState

用新的state和URL替换当前。不会造成页面刷新。
state：与要跳转到的 URL 对应的状态信息。
title：当前大多数浏览器都忽略此参数，但是将来可能会使用它。 在此处传递空字符串应该可以防止将来对方法的更改。 或者，您可以为要跳转到的地址传递简短标题。 如果需要更改标题，可以使用document.title。
url：要跳转到的URL地址，不能跨域。

## window.onpop