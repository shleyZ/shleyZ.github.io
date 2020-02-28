---
title: '@bigtest/react'
date: 2020-01-08 13:14:24
categories: bigtest
tags: bigtest
---

进行大型测试的 React DOM 助手：@bigtest/react

github 地址： https://github.com/bigtestjs/react

这个软件包的目的是提供一套帮助程序，使得能够更加容易的测试 React 程序

## mount 方法

在一个新的 DOM 节点异步加载一个组件，返回 promise， 一连串的使用将清除所有先前安装的组件。

```js
import { mount } from '@bigtest/react';
import Button from '../src/components/button';

describe('My button', () => {
  // `mount` 返回Promise: 渲染后resolves
  beforeEach(() => mount(() => <Button />));

  it('renders', () => {
    expect(document.querySelector('.button')).to.exist;
  });
});
```

## setupAppForTesting

setupAppForTesting 不仅会 mount 应用程序组件，还将使用该组件的实例进行解析，并提供一个内存重的 history 对象，供 React Router 在测试期间使用。

```js
import { setupAppForTesting } from '@bigtest/react';
import App from '../src/app';

describe('My Application', () => {
  let app;
  beforeEach(async () => {
    app = await setupAppForTesting(App);
  });

  it('renders', () => {
    expect(document.querySelector('#app')).to.exist;
  });

  if('has a history prop', () => {
    // this prop is only provided if defined in `propTypes`
    expect(app.props).to.have.property('history');
  })
})
```

## visit

只有在使用 setupAppForTesting，并与应用程序的 history 属性进行交互以在路由之间导航后，才会起作用。（visit, goBack, goForward, and location 都是这样）

```js
import { setupAppForTesting, visit, location } from '@bigtest/react';
import App from '../src/app';

describe('My Application', () => {
  let app;
  beforeEach(async () => {
    app = await setupAppForTesting(App);
  });

  describe('navigating', () => {
    beforeEach(() => visit('/some-route'));

    it('is at the new route', () => {
      expect(location()).to.have.property('pathname', '/some-route');
    });
  });
});
```

## cleanup

在每次 mount 和 setupAppForTesting 调用开始时都会调用 cleanup，以清除以前安装的任何组件或应用程序。 如果您需要自己清理以前安装的组件，也可以单独使用它。

```js
import { mount, cleanup } from '@bigtest/react';
import Button from '../src/components/button';

describe('My Button', () => {
  beforeEach(() => mount(<Button />));

  // 为了可以在测试运行后调查并使用组件进行调试，最好不要这样做
  afterEach(() => cleanup());
});
```

## 注意

mount 和 setupAppForTesting 都有第二个参数： 一个对象包含一系列的参数。

```js
mount(<Button />, {
  mountId: 'testing-root',  // 作为加载的DOM节点的id
  rootElement: document.body, // 加载的DOM节点的位置
  setup: () => {},  // 在组件加载好之前调用， 如果返回的是promise，则组件会在resolve之后加载
  teardown: () => {}  // 在下一个 cleanup 调用时调用，或者在下一个mount或setupAppForTesting开始时调用，或者在直接调用 cleanup 时调用。 和“ setup”一样，如果返回的是promise，则组件会在resolve之后加载
})
});
```

此外， setupAppForTesting 接受一个 props 选项， 该选项将与所有用户定义的 props 一起传递给应用程序组件。

```js
setupAppForTesting(App, {
  props: {
    store: createStore(),
    // you can provide your own history object as well
    history: createHistory(historyOptions)
  }
});
```

## 可重用性

为了在测试应用程序时获得最佳体验，任何不属于组件生命周期中的必需的逻辑，都应该是可重用的。通过提供必要的 setup 和 teardown 选项，或者将逻辑移入组件生命周期挂钩中。

除此之外，在您自己的 test helper 中使用 setupAppForTesting 可能很有意义，因为它可以处理任何其他必要的设置。 这也使我们不必导入应用程序并在每个测试文件中重复设置逻辑。

```js
// test/helper.js
import { setupAppForTesting } from '@bigtest/react';
export { visit, goBack, goForward, location } from '@bigtest/react';

import App from '../src/app';
import createServer from './mocks/server';

export function setupApplicationForTesting() {
  beforeEach(async function () => {
    this.app = await setupAppForTesting(App, () => {
      setup: () => this.server = createServer(),
      teardown: () => this.server.shutdown()
    });
  });
}
```

## 更好的测试

这些 helpers 使您可以反复安装应用程序进行测试，而与应用程序进行交互也可能会很麻烦。

[@bigtest/interactor](https://github.com/bigtestjs/interactor) 提供了一个更简单的方法，通过任何浏览器与应用的各个部分进行交互。就好像是用户在和你的 app 进行交互。实际上，可组合的 interactors 是测试使用可组合 components 制作的 app 的完美伴侣。

```js
import { setupAppForTesting } from '@bigtest/react';

import App from '../src/app';
import HomePageInteractor from './interactors/home';

describe('My Applicaion', () => {
  const home = new HomePageInteractor();
  beforeEach(() => setupAppForTesting(App));

  it('has a fancy button', () => {
    except(home.button.isPresent).to.be.true;
    except(home.button.isFancy).to.be.true;
  });
});
```
