---
title: '@bigtest/interactor介绍'
date: 2020-01-07 16:20:57
categories: bigtest
tags: bigtest
---

在生物学中，interactor 被定义为自然选择所作用的有机体的一部分。 BigTest interactor 定义了要对其进行测试的应用程序的一部分。您可以将 interactor 视为现代组件的可组合页面对象。

```js
import { interactor } from '@bigtest/interactor';
// an interactor can be scoped directly to an element
let input = new interactor('[data-test-input]');
let submit = new interactor('[data-test-submit]');
```

## Interactor Properties

Interactor 属性是惰性的，在访问之前不查找元素或属性。如果找不到该元素，则会引发错误。
这比典型的期望失败更有用，因为我们知道期望失败的原因，而不仅仅是知道期望值不正确而失败了。

```js
// when the elements do not exist in the DOM
input.value; //=> Error: unable to find "[data-test-input]"
submit.text; //=> Error: unable to find "[data-test-submit]"

// when the elements do exist in the DOM
input.value; //=> "foo"
submit.text; //=> "Submit"
```

## Immutable Methods

Interactor 是不可变的。方法返回 Interactor 的新实例，并在队列中附加一个 interaction。
然后您可以通过调用 #run() 方法来在 Interactor 的队列中运行所有 Interactor。

```js
// this does not actually focus the input yet
let focusInput = input.focus();

// chaining additional interactions will add to the new instance queue
let focusAndFill = focusInput.fill('some value');

// an interactor will wait until the element exists before interacting with it
focusAndFill.run()
.then(...)  // the input was succesfully focused and filled with "some value"
.catch(...) // something went wrong, likely the input was not found
```

Interactor 也可以相互组合以产生更复杂的交互。而且由于它们是不可变的，因此可以反复使用它们。

```js
// remember, this does not actually interact with the elements yet
let fillInput = input
  .focus()
  .fill('some value')
  .blur();
let submitForm = submit.click();

// when this is run, it will focus, fill, and blur the input
// before finally clicking on the submit button
let fillAndSubmit = fillInput.append(submitForm);

// in one test...
fillAndSubmit.run();

// ... in another
fillAndSubmit.run();
```

默认的 interaction 方法可以选择接受一个选择器作为它们的第一个参数，以便您可以与作用域内的 interaction 中的元素进行交互。

interaction 也是可异步的，它可以立即调用#run（），从而允许 interaction 使用 async/await 语法。

```js
await new Interactor('[data-test-form]')
  .fill('[data-test-input]', 'some value')
  .click('[data-test-submit]');
```

有关可用的默认属性和方法，请参阅[Available Interactions](https://bigtestjs.io/guides/interactors/available-interactions/)。
您可以通过创建自定义 interaction 来覆盖或定义自己的 interaction。
这可以通过直接扩展 Interactor 类或使用@interactor 类装饰器和 interaction creators 来完成。
