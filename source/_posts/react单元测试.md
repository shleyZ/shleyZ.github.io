---
title: 单元测试
date: 2018-12-14 09:03:05
categories: test
---

#### Jest 被 Facebook 用来测试包括 React 应用在内的所有 JavaScript 代码。Jest 的一个理念是提供一套完整集成的 “零配置” 测试体验。


#### jsdom


#### Enzyme 模拟react浅render


#### nock.js模拟请求返回的数据


#### sinon 用于做函数的跟踪，集成在jest里面


#### Istanbul 检测覆盖率


#### mocha

  Mocha的作用是运行测试脚本，首先必须学会写测试脚本。所谓"测试脚本"，就是用来测试源码的脚本。

待测试的源码

    // add.js
    function add(x, y) {
      return x + y;
    }

    module.exports = add;

测试脚本

    // add.test.js
    var add = require('./add.js');   // 引入待测试函数
    var expect = require('chai').expect;  // 引入断言库

    describe('加法函数的测试', function() {
      it('1 加 1 应该等于 2', function() {  // it是一个最小的测试单元，测试用例
        expect(add(1, 1)).to.be.equal(2); // 断言
      });
    });

运行测试脚本


    mocha --recursive  

    Mocha默认运行test子目录里面的测试脚本。所以，一般都会把测试脚本放在test目录里面，然后执行mocha就不需要参数了。
    Mocha默认只执行test子目录下面第一层的测试用例，不会执行更下层的用例。
    加上--recursive参数，这时test子目录下面所有的测试用例----不管在哪一层----都会执行。

    命令行指定测试脚本时，可以使用通配符，同时指定多个文件。

    mocha spec/{my,awesome}.js
    mocha test/unit/*.js


     mocha --reporters 可以查看生成的测试报告格式

     mocha --reporter tap   //默认形式
     mocha --reporter spec  
     mocha --reporter markdown


监视测试脚本

    mocha --watch

    --watch参数用来监视指定的测试脚本。只要测试脚本有变化，就会自动运行Mocha。    
