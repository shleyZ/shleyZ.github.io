---
title: stripes 问题汇总
date: 2019-10-31 09:01:35
tags: FOLIO
---

1. 自定义的组件库需要放在@folio 文件夹下面

2. platform-complete 里面的 stripes 版本必须大于各个模块的 stripes 版本

3. 启动或者打包报内存溢出的问题，启动或者打包的时候需要添加参数--max-old-space-size

4. 自定义组件的国际化语言包找不到的问题：

   自定义组件的包名以 stripes 开头，要和 package.json 里面的包名一致，也要和 tranlations 文件夹下面的文件夹名称一致。
   在 local 启动的时候单个模块需要额外配置 stripes.config.js.local 和.stripesclirc 以使得 stripes-core 模块内部能够找到语言包。

文件 stripes.config.js.local 如下：

```js
module.exports = {
  okapi: {
    url: 'https://folio-testing-okapi.aws.indexdata.com',
    tenant: 'diku'
  },
  config: {
    logCategories: 'core,path,action,xhr',
    logPrefix: '--',
    showPerms: false,
    hasAllPerms: false,
    languages: ['en']
  },
  modules: {
    '@folio/stripes-your-components': {},
    '@folio/users': {}
  }
};
```

文件 .stripesclirc 如下:

```json
{
  "configFile": "stripes.config.js.local",
  "port": 3000,
  "aliases": {
    "@folio/stripes-smart-components-jt": "../@folio/stripes-your-components"
  }
}
```

5. svgo 报错：

方法一： 可以在 package.json 锁定， "svgo": "1.3.0"

方法二： svgo 1.3.2 已经发布，应该也可以解决该问题。
