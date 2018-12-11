---
title: AJAX与Fetch
date: 2018-11-22 22:00
tags:
- AJAX
- Fetch
toc: true
reward: true
---
之前总是用封装好的异步请求方法，对最原始的AJAX和Fetch并不是很熟知，现在整理一波

<!-- more -->

## AJAX

AJAX 是异步的JavaScript和XML(Asynchronous JavaScript And XML)，简单来说，就是使用XMLHttpRequest对象与服务器通信。

* AJAX可以使用JSON、XML、HTML、text等格式发送和接受数据
* 因为它的“异步”特性，可以在不刷新页面情况下与服务器通信

### XMLHTTPRequest.readyState 的几种状态

* 0 (未初始化) or (请求还未初始化)
* 1 (正在加载) or (已建立服务器链接)
* 2 (加载成功) or (请求已接受)
* 3 (交互) or (正在处理请求)
* 4 (完成) or (请求已完成并且响应已准备好)

### POST 请求几种编码方式（Content-type）

* Content-Type: application/x-www-form-urlencoded (default):

  ```url
  foo=bar&baz=The+first+line.&#37;0D%0AThe+second+line.%0D%0A
  ```

* Content-Type: text/plain

  ```url
  foo=bar
  baz=The first line.
  The second line.
  ```

* Content-Type: multipart/form-data

  ```url
  Content-Disposition: form-data; name="foo"

  bar
  -----------------------------314911788813839
  Content-Disposition: form-data; name="baz"

  The first line.
  The second line.
  ```

### 一个简单的Ajax请求例子

* Ajax方法

  ```javascript
  // option 为对象
  function ajax(option) {
    let xhr;
    // 兼容各个浏览器
    if (window.XMLHttpRequest) {   // 主流浏览器 IE7+
      xhr = new XMLHttpRequest(); 
    } else if (window.ActiveXObject) {  // IE6-
      xhr = new ActiveXObject("Microsoft.XMLHTTP");
    }

    const method = option.method.toUpperCase();
    const async = option.async | true;
    let url = option.url;
    xhr.timeout = option.timeout | 3000;

    let paramArr = [];
    if (option.params instanceof Object){
      for (let param of Object.keys(option.params)) {
        paramArr.push(`${encodeURIComponent(param)}=${encodeURIComponent(option.params[param])}`);
      }
      if (method === 'GET') {
        url = `${url}?${paramArr.join('&')}`;
      }
    }

    return new Promise((resolve, reject) => {
      xhr.ontimeout = () => reject && reject('请求超时！');

      xhr.onreadystatechange = () => {
        // 判断XMLHttpRequest状态
        if (xhr.readyState === 4) {
          // 返回数据状态
          if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
            return resolve && resolve(xhr.responseText)
          } else {
            reject('系统异常！')
          }
        }
      };

      xhr.onerror = err => reject && reject(err)

      xhr.open(method, url, async);

      if (method === 'GET') {
        xhr.send(null);
      } else {
        xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded;charset=UTF-8');
        xhr.send(paramArr.join('&'));
      }
    });
  }
  ```

* 调用

  ```javascript
  ajax({
    url: '/getSomething',
    method: 'get',
    params: {
      a: '11',
      b: 1
    }
  }).then((res) => {
    console.log('res-get', JSON.parse(res));
  })

  ajax({
    url: '/sendSomething',
    method: 'post',
    params: {
      a: '有吗',
      b: 1
    }
  }).then((res) => {
    console.log('res-post', res);
  })

  ```

## Fetch

Fetch API 提供了一个 JavaScript接口，用于访问和操纵HTTP管道的部分，例如请求和响应。它还提供了一个全局 fetch()方法，该方法提供了一种简单，合理的方式来跨网络异步获取资源。

与XMLHttpRequest有两个不同点：

* 当接收到一个代表错误的 HTTP 状态码时，从 fetch()返回的 Promise 不会被标记为 reject， 即使该 HTTP 响应的状态码是 404 或 500。相反，它会将 Promise 状态标记为 resolve （但是会将 resolve 的返回值的 ok 属性设置为 false ），仅当网络故障时或请求被阻止时，才会标记为 reject。
* 默认情况下，fetch 不会从服务端发送或接收任何 cookies, 如果站点依赖于用户 session，则会导致未经认证的请求（要发送 cookies，必须设置 credentials 选项）

credentials有三种值：

* `credentials: 'include'` : 浏览器发送包含凭据的请求（即使是跨域源）
* `credentials: 'same-origin'` : 在请求URL与调用脚本位于同一起源处时发送凭据
* `credentials: 'omit'` : 确保浏览器不在请求中包含凭据

### 简单的fetch请求

```javascript
fetch('http://example.com/movies.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(myJson);
  });
```