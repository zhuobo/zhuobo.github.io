---
title: struts2解决表单重复提交
date: 2019-06-11 16:29:20
tags: [struts2,Web]
categories: web
---

## 一、表单的重复提交

在用户填写完表单之后、提交表单之后，有可能会出现表单的重复提交，出现这种情况有可能有以下几种原因：

1. 网络状态不好，多次点击提交按钮；
2. 提交表单后，点击浏览器的刷新按钮；
3. 提交表单后，点击浏览器的后退按钮，再次点击提交按钮。

## 二、表单重复提交的解决方案

1. 使用JavaScript设置一个状态位，第一次点击之前设为`true`，表示用户可以提交表单，第一次点击之后设置为`false`，不允许用户提交或者直接将提交按钮`disabled`；但是这种方法只针对以上三种情况中的第一种有效，无法避免第二、第三种情况的发生；

2. 使用验证码的方式：用户填写的表单包括服务器生成的验证码，当表单数据提交到服务器之后，服务器从`session`中获取服务器自己生成的验证码，获取之后从`session`中删除验证码，以保证该验证码的一次性；这样，服务器每次收到客户端的请求都先判断验证码是否一致，如果不一致自然便是重复提交（或者验证码错误）而无需处理数据；

3. struts的token可以解决这个问题：使用`<s:token>`生成一个令牌，再配合 `tokensession`拦截器

   1. 创建一个表单，在表单里添加一个隐藏的`<s:token>`标签;

      ```html
      <form action="${pageContext.request.contextPath}/student/login.do">
          <s:textfield name="username" label="用户名"/>
          <s:textfield name="password" label="密码"/>
          <%--token标签--%>
          <s:token />
          <input value="登录" type="submit">
      </form>
      ```

   2. 在struts.xml中配置 `tokensession`拦截器

      ```xml
      <action name="login" class="cn.zhuobo.web.action.StudentAction" method="login" >
          <!--由于配置了自定义的不在默认拦截器里面的拦截器，因此要手动加上默认拦截器defaultStack-->
          <interceptor-ref name="defaultStack"/>
          <interceptor-ref name="tokenSession"/>
          <result name="success">/success.jsp</result>
          <result name="invalid.token">/invalid.jsp</result>
      </action>
      ```

