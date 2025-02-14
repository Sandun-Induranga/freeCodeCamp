---
id: 587d8248367417b2b2512c3a
title: 用 helmet.noSniff() 避免推断出响应的 MIME 类型。
challengeType: 2
forumTopicId: 301574
dashedName: avoid-inferring-the-response-mime-type-with-helmet-nosniff
---

# --description--

As a reminder, this project is being built upon the following starter project on <a href="https://replit.com/github/freeCodeCamp/boilerplate-infosec" target="_blank" rel="noopener noreferrer nofollow">Replit</a>, or cloned from <a href="https://github.com/freeCodeCamp/boilerplate-infosec/" target="_blank" rel="noopener noreferrer nofollow">GitHub</a>. 浏览器可以使用内容或 MIME 嗅探来覆盖响应的 `Content-Type` 头，猜测并使用隐含的内容类型来处理数据。 虽然这在某些情况下可能很方便，但也可能导致一些危险的攻击。 该中间件将 X-Content-Type-Options 头设置为 `nosniff`，指示浏览器不要绕过所提供的 `Content-Type`。

# --instructions--

在你的服务器上使用 `helmet.noSniff()` 方法。

# --hints--

helmet.noSniff() 中间件应该被正确安装

```js
(getUserInput) =>
  $.get(getUserInput('url') + '/_api/app-info').then(
    (data) => {
      assert.include(data.appStack, 'nosniff');
      assert.equal(data.headers['x-content-type-options'], 'nosniff');
    },
    (xhr) => {
      throw new Error(xhr.responseText);
    }
  );
```

# --solutions--

```js
/**
  Backend challenges don't need solutions, 
  because they would need to be tested against a full working project. 
  Please check our contributing guidelines to learn more.
*/
```
