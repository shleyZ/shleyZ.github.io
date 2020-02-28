---
title: '@bigtest/interactor使用'
date: 2020-01-07 16:42:05
categories: bigtest
tags: bigtest
---

## 自定义 interactor

虽然使用默认的 Interactor 对于简单和较小的组件可能很好，但是使用类装饰器和 interactor interaction creators 很容易创建自定义 interaction。

自定义 Interactor 使我们能够将所有选择器都放在一个位置，并具有与其他 Interactor 可组合以与更多复杂结构交互的好处。

```js
import {
  interactor,
  text,
  value,
  property,
  fillable,
  focusable,
  blurrable
} from '@bigtest/interactor';

@interactor
class FiledInteractor {
  // custom properties are lazy like the default properties
  label = text('[data-test-label]');
  value = value('[data-test-input]');
  type = property('[data-test-input]', 'type');

  // `*able` property creators create chainable interactor methods
  focus = focusable('[data-test-input]');
  fill = fillable('[data-test-input]');
  blur = blurrable('[data-test-input]');
}
```

默认的属性和方法集具有相应的 interactor creators。还有其他几个可用的 interactor creators 可供选择。

## 自定义方法和属性

也可以在修饰的类上直接定义其他方法和获取属性的方法。但是，请谨慎使用 property initializers 和 this，因为它会引用未修饰的类实例，而不是您可能期望的 interactor 实例。

```js
@interactor
class FieldInteractor {
  // methods that return new instances of itself will be chainable with other methods
  fillIn(value) {
    return this.focus()
      .fill(value)
      .blur();
  }

  // using getters ensures that properties will not be invoked until necessary
  get isPassword() {
    return this.type === 'password';
  }
  // the following will not work because `this` references the undecorated class
  // foo = () => this.doesntWork()
}
```

## 自定义 Interaction Creators

对于常用的自定义属性和方法，存在两个 helpers，可用于定义自己的可重用 interaction creators：

- 针对属性的 computed
- 针对方法的 action

除了默认方法外，interaction 还具有一些自己的辅助方法。

1. this.\$(selector)

使用 interaction 的根元素和 querySelector 查找嵌套元素。当找不到元素时，将引发错误。如果未定义选择器，则返回根元素。

2. this.\$\$(selector)

也使用 interaction 的根元素，但使用 querySelectorAll 并返回匹配元素的数组。仅当找不到根元素时才会引发错误。

```js
import { computed } from '@bigtest/interactor';

// returns a specific data attribute of an element
export function data(selector, key) {
  // to align with other interaction creators, `selector` is optional
  if (!key) {
    key = selector;
    selector = null;
  }

  // the `computed` helper creates a getter
  return computed(function() {
    return this.$(selector).dataset[key];
  });
}
```

Interactor methods: #find（selector）和#findAll（selector）的行为与上述方法相同，只是它们返回用于链式调用的新实例。找到的元素将传递到链中的下一个函数。

```js
import { action } from '@bigtest/interactor';

// triggers a keypress event for each character in a given string
export function typeable(selector) {
  // the `action` helper returns an interactor method
  return action(function(string) {
    return (
      this.find(selector)
        // `#do` executes a callback within the queue
        .do($node => {
          for (let char of string) {
            $node.dispatchEvent(
              new Event('keypress', {
                charChode: char.charCodeAt(),
                cancelable: true,
                bubbles: true
              })
            );
          }
        })
    );
  });
}
```

## 使用自定义 interactor

可以像使用普通 interactor 一样使用自定义 interactor。

```js
let username = new FieldInteractor('[data-test-username-field]');
let password = new FieldInteractor('[data-test-password-field]');

// focuses, fills, and blurs the field input
await username.fillIn('bigtester');

// property access could trigger "element not found" errors like default properties
expect(username.type).to.equal('text');
expect(password.isPassword).to.be.true;
```

如果一个 interactor 通常只属于一个元素，则定义一个静态的 defaultScope 属性使我们不必每次都使用范围选择器初始化该 interactor。

```js
@interactor
class HomePageInteractor {
  static defaultScope = '[data-test-home-page]';
  // ...
}

// defaults scope to "[data-test-home-page]"
let homePage = new HomePageInteractor();
```

自定义 interactor 也可以通过相互嵌套来构成其他 interactor。诸如 scoped 和 collection 之类的帮助程序还允许您将一个或一组 interactor 范围限定于父 interactor 中的特定元素。

## Interactors 组合

在定义自定义 Interactors 时，可以通过组合较小的 Interactors 来创建更复杂的 Interactors。默认情况下，嵌套 Interactors 的作用域不限于其父 Interactors。这是因为在某些情况下，例如模式或弹出窗口，元素已被附加到文档的不同部分。

```js
@interactor
class ModalInteractor {
  // ...
}

@interactor
class SignUpFormInteractor {
  // the default scope is used when a new instance's selector is omitted
  static defaultScope = '[data-test-sign-up-form]';

  // the modal element exists outside of the form
  confirmation = new ModalInteractor('[data-test-modal]');

  // ...
}
```

## Scoped Interactors 范围交互器

使用 scoped 属性创建器，您可以创建在父 Interactors 范围内的嵌套 Interactors。第二个参数允许我们完全指定其他嵌套属性或甚至另一个 Interactors 类。

```js
@interactor
class SignUpFormInteractor {
  // ...

  // scoped interactors look for an element within the parent element
  submit = scoped('[data-test-submit]');

  // a hash of other properties may be provided as the second argument
  email = scoped('[data-test-email-field] input', {
    placeholder: property('placeholder')
  });

  // using a class is preferred for maximum composability
  password = scoped('[data-test-password-field]', FieldInteractor);
}
```

collection 属性创建器返回一个函数，该函数接受一个索引，并将为该索引处的元素返回一个嵌套的 Interactors。就像普通的 Interactors 一样，此 Interactors 是惰性的，在交互运行或访问属性之前，它不会尝试在索引处查找元素。如果不提供任何参数，则 collection 函数将返回一个 Interactors 数组；调用时找到的每个匹配元素都会添加一个。

```js
@interactor
class SignUpFormInteractor {
  // ...

  // collections also accept additional properties or a class as the second argument
  interests = collection('[data-test-interests-item]"]', {
    toggle: click('input[type="checkbox"]'),
    label: text('[data-test-label]')
  });
}
```

## 嵌套方法和属性

嵌套的 Interactors 方法（无论是否作用域）都会返回根 Interactors 的实例以进行其他链接。嵌套属性在访问时仍会延迟计算。

```js
let signUp = new SignUpFormInteractor();

// `await` will immediately invoke `#run()`, we could also save a reference
// to this specific interaction to re-use elsewhere
await signUp.email
  .fill('foo@bar.baz')
  .password.fillIn('53cr3t')
  .interests(3)
  .toggle()
  .submit();

// nested interactors may be broken from the parent chain using `#only()`
await signUp.email
  .only()
  .focus()
  .fill('foo@bar')
  .blur();

// nested properties may throw one of a few "element not found" errors
signUp.interests(10).label;
// => Error: unable to find "[data-test-label]"
// => Error: unable to find "[data-test-interest-item]" at index 10
// => Error: unable to find "[data-test-sign-up-form]"
```
