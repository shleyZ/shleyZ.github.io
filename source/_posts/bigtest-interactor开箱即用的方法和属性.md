---
title: '@bigtest/interactor开箱即用的方法和属性'
date: 2020-01-07 17:36:46
categories: bigtest
tags: bigtest
---

以下是 interactor 开箱即用可用的各种交互的简要说明。

## Default Interactions

这些默认的交互属性和方法在所有 interactor 上都可用，但是在定义自己的自定义 interactor 时可以安全地覆盖它们。

### Properties

如果元素不能位于 Interactions 的根元素中，则所有属性（isPresent 除外）都将引发错误。

1. #text： 返回元素 trimmed 后的 textContent 属性

```js
new Interactor('p').text;
```

2. #value: 返回元素的 value 属性

```js
new Interactor('input').value;
```

3. #isVisible: 如果元素在文档中可见，则返回 true 否则 false

```js
new Interactor('#foo').isVisible; //=> true
new Interactor('#bar').isVisible; //=> false
```

4. #isHidden： 如果元素存在于文档中但在视觉上被隐藏，则返回 true 否则 false

```js
new Interactor('#foo').isHidden; //=> false
new Interactor('#bar').isHidden; //=> true
```

5. #isPresent： 如果可以在文档中找到一个元素，则返回 true 否则 false。

```js
new Interactor('#foo').isPresent; //=> true
new Interactor('#bar').isPresent; //=> false
```

### Methods

所有默认的交互方法都接受一个可选的选择器作为第一个参数。给定一个选择器，将在父交互器的根元素内的匹配元素上进行交互。

注意：交互方法将返回交互器的新实例，并且在新实例上调用#run（）方法之前不会进行任何交互。

1. #click([selector]) 触发元素上的点击事件。

```js
await new Interactor('button').click();
await new Interactor('form').click('[type="submit"]');
```

2. #fill([selector], value) 更改元素的值并触发输入和更改事件。

```js
await new Interactor('input').fill('value');
await new Interactor('form').fill('input#name', 'value');
```

3. #select([selector], option) 通过文本值选择一个选项并触发输入和更改事件。

```js
await new Interactor('select').select('February');
await new Interactor('form').fill('select#month', 'March');
```

4. #focus([selector]) 在元素上触发焦点事件。

```js
await new Interactor('input').focus();
await new Interactor('form').focus('input[type="email"]');
```

5. #blur([selector]) 在元素上触发失去焦点事件

```js
await new Interactor('input').blur();
await new Interactor('form').blur('input[type="email"]');
```

6. #scroll([selector], { top, left }) 设置元素的 scrollTop 和 scrollLeft 属性并触发滚动事件。顶部和左侧的值指定在该方向上滚动到多少像素；必须至少指定一个方向。

```js
await new Interactor('#page').scroll({ top: 100 });
await new Interactor('#page').scroll('.nested-view', { left: 100 });
```

7. #trigger([selector], name[, options]) 使用任何指定的事件选项在元素上触发任意事件，名称。默认情况下，气泡和可取消选项设置为 true。

```js
await new Interactor('#foo').trigger('customEvent');
await new Interactor('#foo').trigger('customEvent', { ... });
await new Interactor('#foo').trigger('#bar', 'customEvent');
await new Interactor('#foo').trigger('#bar', 'customEvent', { ... });
```

## Property Creators

使用 @interactor 类装饰器创建自己的自定义 interactors 时，可以利用 Property creators。
就像默认方法一样，creator 函数接受一个可选的选择器。但是，生成的实例方法没有可选的 selector 参数。

### Properties

interactor Property creators 返回一个 getter。就像默认属性一样，除了 isPresent 之外，如果元素不能位于 interactor 的根元素中，则所有属性都会引发错误。

1. text([selector]) 返回元素 trimmed 后的 textContent 属性

```js
@interactor
class PageInteractor {
  heading = text('h1');
}

new PageInteractor().heading; //=> "Hello World!"
```

2. value([selector]) 返回元素的 value 属性。

```js
@interactor
class FormInteractor {
  name = value('input#name');
}

new FormInteractor('form').name; //=> "Foo Bar"
```

3. isVisible([selector]) 如果元素在文档中可见，则返回 true 否则 false

```html
<div id="foo">...</div>
<div id="bar" style="display: none">...</div>
```

```js
@interactor
class PageInteractor {
  isFooVisible = isVisible('#foo');
  isBarVisible = isVisible('#bar');
}

new PageInteractor().isFooVisible; // => true
new PageInteractor().isBarVisible; // => false
```

4. isHidden([selector]) 如果元素存在于文档中但在视觉上被隐藏，则返回 true 否则 false。

```html
<div id="foo">...</div>
<div id="bar" style="display: none">...</div>
```

```js
@interactor
class PageInteractor {
  isFooHidden = isHidden('#foo');
  isBarHidden = isHidden('#bar');
}

new PageInteractor().isFooHidden; // => false
new PageInteractor().isBarHidden; // => true
```

5. isPresent([selector]) 如果可以在文档中找到一个元素，则返回 true 否则 false。

```html
<div id="foo">...</div>
```

```js
@interactor
class PageInteractor {
  isFooPresent = isPresent('#foo');
  isBarPresent = isPresent('#bar');
}

new PageInteractor().isFooPresent; // => true
new PageInteractor().isBarPresent; // => false
```

6. attribute([selector], attr) 通过 getAttribute 返回元素的指定属性。

```html
<div class="card" id="foo">
  ...
  <a class="card-link" href="https://example.com">...</a>
</div>
```

```js
@interactor
class CardInteractor {
  id = attribute('id');
  url = attribute('.card-link', 'href');
}

new CardInteractor('.card').id; //=> "foo"
new CardInteractor('.card').url; //=> "https://example.com"
```

7. property([selector], prop) 返回元素的指定属性值。

```html
<div class="card" style="height: 100px">
  ...
  <button class="card-cta" disabled>...</button>
</div>
```

```js
@interactor
class CardInteractor {
  height = property('offsetHeight');
  isDisabled = property('button.card-cta', 'disabled');
}

new CardInteractor('.card').height; //=> 100
new CardInteractor('.card').isDisabled; //=> true
```

8. hasClass([selector], className) 如果元素的 classList 包含指定的类名，则返回 true 否则 false。

```html
<form class="error" ...>
  <input id="name" class="error" />
  <input type="email" id="email" />
</form>
```

```js
@interactor
class FormInteractor {
  hasErrors = hasClass('error'); // applies to the root
  hasNameError = hasClass('input#name', 'error');
  hasEmailError = hasClass('input#email', 'error');
}

new FormInteractor('form').hasErrors; //=> true
new FormInteractor('form').hasNameError; //=> true
new FormInteractor('form').hasEmailError; //=> false
```

9. is([selector], match) 如果指定的匹配选择器可以通过 Element.matches（）选择一个元素，则返回 true 否则 false。

```html
<ul class="list">
  <li id="foo">...</li>
  <li id="bar">...</li>
  <li id="baz">...</li>
</ul>
```

```js
@interactor
class ListInteractor {
  isList = is('.list');
  isFooFirst = is('#foo', ':first-child');
  isBarLast = is('#bar', ':last-child');
}

new FormInteractor('form').hasErrors; //=> true
new FormInteractor('form').hasNameError; //=> true
new FormInteractor('form').hasEmailError; //=> false
```

### Methods

interactor methods Creators 返回链式函数，这些函数随后返回具有添加到其队列中的特定交互的新实例。直到调用交互程序的 #run() 方法或使用 async/await 语法时，interactions 才会运行。

1. clickable([selector]) => click() 触发元素的 click 事件。

```html
<div class="card">
  ...
  <a class="card-link" href="https://example.com">...</a>
</div>
```

```js
@interactor
class CardInteractor {
  clickThrough = clickable('.card-link');
}

await new CardInteractor('.card').clickThrough();
```

2. fillable([selector]) => fill(value) 更改元素的 value 值并触发 input 和 change 事件

```html
<form ...>
  <input id="name" />
  ...
</form>
```

```js
@interactor
class FormInteractor {
  fillName = fillable('input#name');
}

new FormInteractor('form').fillName('value');
```

3. selectable([selector]) => select(option) 通过 text 值选择一个选项并触发 input 和 change 事件。

```html
<form ...>
  <select id="month">
    <option value="1">January</option>
    <option value="2">February</option>
    <option value="3">March</option>
    ...
  </select>
  ...
</form>
```

```js
@interactor
class FormInteractor {
  selectMonth = selectable('select#month');
}

new FormInteractor('form').selectMonth('February');
```

4. focusable([selector]) => focus() 在元素上触发 focus 事件。

```html
<form ...>
  <input type="email" />
  ...
</form>
```

```js
@interactor
class FormInteractor {
  focusEmail = focusable('input[type="email"]');
}

await new FormInteractor('form').focusEmail();
```

5. blurrable([selector]) => blur() 在元素上触发 blur 事件。

```html
<form ...>
  <input type="email" />
  ...
</form>
```

```js
@interactor
class FormInteractor {
  blurEmail = blurrable('input[type="email"]');
}

await new FormInteractor('form').blurEmail();
```

6. scrollable([selector]) => scroll({ top, left }) 置元素的 scrollTop 和 scrollLeft 属性并触发 scroll 事件。top 和 left 的值指定在该方向上滚动到多少像素；必须至少指定一个方向

```html
<div id="page">
  <div class="scrollview"></div>
</div>
```

```js
@interactor
class PageInteractor {
  scrollSection = scrollable('.scrollview');
}

new PageInteractor('#page').scrollSection({ top: 100 });
```

7. triggerable([selector], name[, options]) => trigger([options]) 使用任何指定的事件 options 在元素上触发任意事件，名称。默认情况下，bubbles 和 cancelable 选项设置为 true。运行交互程序时，两个 options 参数将合并。

```js
@interactor
class PageInteractor {
  triggerEvent = triggerable('customEvent', { ... });
  triggerFooEvent = triggerable('#foo', 'customEvent');
}

await new PageInteractor().triggerEvent();
await new PageInteractor().triggerEvent({ ... });
await new PageInteractor().triggerFooEvent();
await new PageInteractor().triggerFooEvent({ ... });
```

### Interactors

嵌套的 interactor creators 返回范围为父 interactor 根元素的 interactor 实例。第二个参数是要添加到返回的 interactor 中的可选的参数。第二个参数也可能是一个 interactor class，在创建嵌套的，作用域的 interactor 时会使用它自己的方法和属性。

1. scoped(selector[, properties]) 单个嵌套 interactor 的 Interactor creator。

```html
<form class="login-form">
  <input type="text" name="username" />
  <input type="email" name="email" />
  <button type="submit">Login</button>
</form>
```

```js
@interactor
class LoginFormInteractor {
  username = scope('input[name="username"]');
  email = scope('input[name="email"]');
  submit = clickable('button[type="submit"]');
}

await new LoginFormInteractor('form')
  .username.fill('darklord1926')
  .email.fill('tom.riddle@hogwarts.edu')
  .email.blur()
  .submit();

await new LoginFormInteractor('form')
  .username.fill('h4x0r')
  .email.only().fill('not@an@email')
  .blur();
```

2. collection(selector[, properties]) => fn([index]) 一组嵌套 interactor 的 Interactor creator。集合 interactor 将索引作为参数，并返回作用域为该元素的 interactor。没有索引，将返回相应的 interactor 数组

```html
<ul class="checkboxes">
  <li><input type="checkbox" .../></li>
  <li><input type="checkbox" .../></li>
  <li><input type="checkbox" .../></li>
</ul>
```

```js
@interactor
class CheckboxGroupInteractor {
  items = collection('input[type="checkbox"]');
}

await checkboxGroup
  .items(0).click()
  .items(1).click();)

await checkboxGroup
  .items(0).click()
  .items(1).only()
  .focus()
  .trigger('keydown', { which: 32 })
```

### Helpers

Property creator 可用于定义您自己的自定义 interactions。支持直接在 interactor class上定义 methods 和 getters。但是，使用这些帮助程序可以让您创建可重复使用的Property creator ，例如上述交互。

1. computed(getter) 返回 Interactor Property 的 property descriptor。

```js
function data(key, selector) {
  return computed(function() {
    return this.$(selector).dataset[key];
  })
}

@interactor 
class PageInteractor {
  username = data('user', '#user-info');
}
```

2. action(method) 返回一个交互方法的 property descriptor。

```js
function check(selector) {
  return action(function(name) {
    return this.click(`${selector}[name="${name}"]`);
  })
}

@interactor 
class CheckboxGroupInteractor {
  check = check('input[type="checkbox"]');
}

new CheckboxGroupinteractor('.checkboxes').check('option-1');
```