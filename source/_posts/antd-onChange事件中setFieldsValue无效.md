---
title: antd onChange事件中setFieldsValue无效
date: 2018-11-15 16:35:43
categories: antd
---

antd Form 表单中onChange事件中setFieldsValue 无效

参考 http://ju.outofmemory.cn/entry/348216

出现的场景：

想要在Input输入框中，禁止输入空格等特殊字符

``` js

    {
      key: 'name',
      label: '姓名',
      labelCol: { span: 7 },
      wrapperCol: { span: 13 },
      style: {
        display: 'inline-block',
        marginTop: '18px',
        paddingLeft: '20px'
      },
      node: (
        <Input
          style={{ width: '110px' }}
          placeholder="请输入姓名"
          onChange={this.onNameSelect}
        />
      ),
      options: {
        initialValue: this.state.name
      }
    }

```

想要在onChange方法中正则替换非法字符为空
可是最后发现无效

参照上面的链接，里面有onChange的源码解说，可以用options.normalize方法，处理

官方文档说明： 

  转换默认的 value 给控件，
  一个例子： https://codepen.io/afc163/pen/JJVXzG?editors=001

综上实现方法：

``` js

    {
      key: 'name',
      label: '姓名',
      labelCol: { span: 7 },
      wrapperCol: { span: 13 },
      style: {
        display: 'inline-block',
        marginTop: '18px',
        paddingLeft: '20px'
      },
      node: (
        <Input
          style={{ width: '110px' }}
          placeholder="请输入姓名"
          onChange={this.onNameSelect}
        />
      ),
      options: {
        initialValue: this.state.name,
        normalize: this.onNameSelect1
      }
    }

```

    // 禁止输入空格/表情/特殊字符

``` js

    onNameSelect1 = (value: any, preValue: any) => {
      let res = value.replace(/(^\s+)|(\s+$)/g, ''); // 去掉空格
      res = res.replace(/[&\|\\\*^%$#@\-]/g, ''); // 去掉特殊字符
      res = res.replace(
        /[\uD83C|\uD83D|\uD83E][\uDC00-\uDFFF][\u200D|\uFE0F]|[\uD83C|\uD83D|\uD83E][\uDC00-\uDFFF]|[0-9|*|#]\uFE0F\u20E3|[0-9|#]\u20E3|[\u203C-\u3299]\uFE0F\u200D|[\u203C-\u3299]\uFE0F|[\u2122-\u2B55]|\u303D|[\A9|\AE]\u3030|\uA9|\uAE|\u3030/gi,
        ''
      ); //去除表情
      return res;
    };

```

