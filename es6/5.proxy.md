#### 一、什么是proxy
proxy是在目标对象前架设一个拦截，外界对该对象的访问，都会先经过这层拦截，所有可以对外界的访问进行修改和过滤。


```
let proxy = new Proxy(target, handler)

// new Proxy 表示实例化一个proxy对象，target是目标对象，handler是用来执行过滤或修改操作的对象。

let proxy = new Proxy({}, {
    get: (target, property) => {
        console.log(target) // {}
        console.log(property) // age
        return 34
    }
})

console.log(proxy.age) // 34
```
注： 要是proxy有效，必须针对proxy实例操作，直接操作目标对象无效。

#### 二、Proxy.revocable()
Proxy.revocable()返回一个对象，proxy属性是Proxy实例，revoke属性是一个函数，用来取消proxy实例

```
let target = {}
let handler = {}

let {proxy, revoke} = Proxy.revocable(target, handler)

proxy.age = 123
proxy.age // 123

revoke()
proxy.age // TypeError: Revoked

// 保证一旦访问结束，就收回代理权。
```

#### 三、this问题
在做proxy代理时，目标对象方法里的this在代理中会指向proxy实例，导致目标对象的某些方法无法正确执行。

```
const target = {
    data: 2,
    m: function () {
        cosnsole.log(this === proxy)
    }
}

let proxy = new Proxy(target, {})
target.m() // false
proxy.m() // true

const wm = new WeakMap()
class target {
    constructor () {
        age: wm.set(this, 25)
    }
    get () {
       return wm.get(this) 
    }
}

const targetInstance = new target()

const proxy = new Proxy(targetInstance, {})
targetInstance.get() // 25
proxy.get() // TypeError

// 通过bind()解决

const wm = new WeakMap()
class target {
    constructor () {
        age: wm.set(this, 25)
        this.get = this.get.bind(this)
    }
    get () {
       return wm.get(this)
    }
}

const targetInstance = new target()

const proxy = new Proxy(targetInstance, {})
targetInstance.get() // 25
proxy.get() // 25

```




