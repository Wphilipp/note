#### 一、什么是Promise
Promise是用来处理异步操作的对象，有3种状态，pedding(初始状态) fulfilled(操作成功状态) rejected(操作失败状态)，
在构造函数中接受一个函数参数，该函数有两个参数，resolve 和reject,它们是js内部提供的函数，调用resolve，可以让promise的状态从pedding到fulfilled,调用reject，可以让promise的状态从pedding到reject。

#### 二、基本用法

```
let promise = new Promise(function (resolve, reject) {
    if (/** 异步操作成功 **/) {
        resolve(value)
    } else {
        reject(error)
    }
} )

promise.then(function(value) {
    // success    
}, function (error) {
    // error
})

//dmeo
function timeout (ms) {
  return new Promise((resovle, reject) => setTimeout(() => resovle() ,ms) )
}

timeout(2000).then(() => console.log('hello promise'))
```
promise版ajax

```
function promiseAjax (url) {
  let promise = new Promise(function (resolve, reject) {
    function handler (url) {
      if (this.readState =! 4) {
        return
      }
      if (this.status === 200) {
        resolve('this.responseText')
      } else {
        reject('response error')
      }
    }

    let client = new XMLHttpRequest()
    client.open('GET', url)
    client.onreadystatechange = handler
    client.responseType = "json"
    client.setRequestHeader("Accept", "application/json")
    client.send()
      })
  return promise
}

promiseAjax('url').then((data) => {console.log(data)}
  , (error) => {
    console.log(error)
  })
```
##### alinode版ajax

```
// alinode版ajax
var ajax = (function () {
  'use strict'
  let done = 4,
      ok = 200;
  // get方法中编码参数
  function serializ (obj) {
    let arr = []
    for (item in obj) {
      if (obj.hasOwnProperty(item)) {
        arr.push(encodeURIComponent(item) + '=' + encodeURIComponent(obj[item]))
      }
    }
    return arr.join('&')
  }
  // ajax核心功能
  function core (method, url, data, headers) {
    if (method === 'GET' && data) {
     url += '?' + serializ(data)
    }

    return new Promise(function (resolve, reject) {
      let xhr = new XMLHttpRequest()

      if (headers) {
        for (item in headers) {
          if (headers.hasOwnProperty(item)) {
            xhr.setRequestHeader(item, headers[item])
          }
        }
      }

      xhr.open(method, url, true)
      xhr.readystatechange = function () {
        if (this.readState === done) {
          if (this.status === ok) {
            resolve('success  this.responseText')
          } else {
            resolve('error  this.responseText')
          }
        }
      }
      xhr.onerror = function () {
        reject('error')
      }
      xhr.setRequestHeader()
      if (method === 'GET' || !data) {
        xhr.send()
      } else {
        xhr.send(data)
      }
    })
  }

  return {
    'GET': core('GET', url, data, headers),
    'POST': core('POST', url, data, headers),
    'PUT': core('PUT', url, data, headers),
    'DELETE': core('DELETE', url, data, headers)
  }

}())
```

#### 三、Promise实例方法

##### 1、Promise.all(iterable)
参数中所有的promise resolve之后，then里面返回一个包含这些promise的future(数组类型)

```
Promise.all([promise1, promise2, promise3]).then((futures) => {console.log(futures) // [future1, future2, future3]  })
```

##### 2、Promise.race(iterable) 
then里面返回最快resolve的promise的future
```
Promise.race([promise1, promise2, promise3]).then((futures) => {console.log(future)})
```

##### 3、Promise.resolve()

快速创建一个resolve的Promise  等同于 new Promise((resolve, reject) => {resove() })

```
Promise.resolve().then((future) => { })
```


##### 4、Promise.reject()
快速创建一个reject的Promise 等同于 new Promise((resolve, reject) => {reject()})


```
Promise.reject('error').catch((error) => {console.log(error)})
```
