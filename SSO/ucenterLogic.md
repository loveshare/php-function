#ucenter逻辑

ucenter 作为用户中心 保存用户信息
其他对接的站点要设置和ucenter的用户密码加密格式一致
密码不一致复制用户信息会不方便，但是也有其他解决方法(登录失败，使用用户提交的密码转化其他加密方式来比对，适用站点类型少的架构)
当用户注册，注册所在对接站点称谓注册站，注册站会保存一份用户信息并同时注册到ucenter中，实现了用户的信息共用。
当用户登录，登录所在对接站点称谓登录站，登录站会先判断自己的数据库是否有相应的信息：

- [x] 1.无信息则直接请求ucenter查询用户信息，若ucenter存在相应信息，则拷贝用户数据到本地，然后登录成功，ucenter通过注入到登录站的js，轮寻注入其他对接站登陆信息。
  但是仅限与其他对接站已经有相应的用户信息的站点，因为找不到用户id。
  
- [x] 2.有信息则登录，向ucenter发送登录成功信息，ucenter通过注入到登录站的js，轮寻注入其他对接站登陆信息。
  但是仅限与其他对接站已经有相应的用户信息的站点，因为找不到用户id。

## 推出的操作没有那么复杂，这里就不记录了。
  
问题：

- [ ] 同步登录会存在用户登录存活时间同步问题（一般都不会去设置）

- [ ] 用户信息修改同步问题。（可接收）

- [ ] 密码同步问题（可接收）

## 示例代码
```html
<!--登录页面-->
<script src="http://www.a.com/test.php"></script>
```

```php
<?php
  // 设置 cookie 和 查看cookie
  if($_GET['id'] == 1){
      print_r($_COOKIE);
      exit;
  }
  header('P3P: CP="CURa ADMa DEVa PSAo PSDo OUR BUS UNI PUR INT DEM STA PRE COM NAV OTC NOI DSP COR"');
  setcookie("test", 111, time()+3600);
```

## 注：
部分浏览器可能会有问题</br>
ie8测试是可以通过好多说会有问题,如果有问题可以设置 XDomainRequestAllowed = 1 </br>
safari 需要设置一下,也可以试一下post提交来实现体验不是很好，增加提示或者抛弃他们</br>
safari 有些奇怪 可以参考 http://www.cnblogs.com/qidian10/p/3316410.html</br>
safari 使用js加载 设置页面无反应，使用iframe可以 但是需要触发才可以设置cookie，打开目标网页或者目标网页已经有了cookie。</br>
safari 问题不是很好解决。
