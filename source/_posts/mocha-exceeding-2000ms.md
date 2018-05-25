---
title: mocha exceeding 2000ms timeout问题解决办法
catalog: false
date: 2018-05-25 11:23:40
subtitle:
header-img: "/img/header_img/header-bg-railway.jpg"
tags:
- mocha
- timeout
---
今天写mocha单元测试的时候遇到mocha timeout 问题，具体报错如下
> Error: Timeout of 2000ms exceeded. For async tests and hooks, ensure "done()" is called; if returning a Promise, ensure it resolves.

原因是mocha默认测试超时设置为2000ms，刚好我测试的function需要保存文件，所以有时会超过2000ms,就报失败了。

解决的办法有两种，一种是在mocha命令后面加上超时参数
```command
mocha --timeout 5000
```
但这样每次跑这个测试的时候都要记得加上参数
另一种方法是在测试函数用例上加上this.timeout(5000),但我加上后发现会报错this.timeout is not a function
原来是这种方法不能加在箭头函数中，即以下例子
```javascript
it('should generate cer file and key file', (done)=> {
        this.timeout(mochaTimeOut)
        certUtils.generateUserCert('test');
        done();
    });
```
必须改成下面的写法
```javascript
it('should generate cer file and key file', function(done) {
        this.timeout(mochaTimeOut)
        certUtils.generateUserCert('test');
        done();
    });
```