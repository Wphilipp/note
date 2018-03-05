#### 一、什么是Generator
Generator是一种特殊的函数，内部封装了多个状态，执行它会返回一个遍历器对象，可以一次遍历内部的每个状态。

调用Generator后，该函数不会立即执行，而是返回一个指向内部状态的指针对象，当调用Generator的next方法时，会使指针指向下一个状态，既每个调用next()方法，函数会从函数头部或上一次指针指向的状态执行 到 下一个yield或return表达式的地方。
所以Generator函数是分段执行的，yield是暂停执行的标志，next()可以让它继续执行。
```
function* gen () {
    yield 1
    yield 2
    return 'end'
}

const g = gen()
g.next() // 1
g.next() // 2
g.next() // 'end'
```

##### yield表达式

yield返回的是一个对象{value:xx, done: Boolean} 如果遍历没有结束，done为false，结束为true

如果yield用在一个表达式中需要用()将yield包起来，但是在表达式的右边或作为函数参数则不用。

```
function* gen () {
    console.log('asdf' + (yield))
    return
}

function foo (params) {
    ...
}

function* gen() {
    let message = 'asdf' + yield
    foo(yield 'a', yield 'b')
    
}
```

#### 二、next()方法的参数
yield表达式本身没有返回值或返回undefined，next()方法中的 参数会被作为上一个yield表达式的返回值，因此，第一次调用next()方法时，传参是无效的。

```
function* gen (x) {
    let y = 2*(yield(x + 1))
    let z = yield + y
    return x + y + z
}

let g = gen(1)

g.next() // {value: 2, done: false}
g.next() // {value: undefined, done: false}
g.next() // {value: undefined, done: true}

g.next() // {value: 2, done: false}
g.next(3) // {value: 6, done: false}
g.next(4) // {value: 10, done: true}
```

如果想要第一次调用next()时，可以有效传参，可以通过一个函数将包裹起来，内部进行第一次调用generator。

```
function wraper (generatorFunction) {
    return function (...args) {
      let g = generatorFunction(...args)
      g.next()
      return g
    }
}

let g = wraper(function* (...args) {
    console.log('first call next() with' + (yield))
    
} )

g().next()
```

#### 三、for of 遍历Generator
除了next()方法， for of 也可以遍历Generator

```
Generator实现菲波那切数列
function* fibonacci () {
    let [pre, cur] = [0, 1]
    for (; ;) {
        [pre, cur] = [cur, pre + cur]
        yield cur
    }
}

let fibo = fibonacci()
for (item of fibo) {
    if (i > 100) break
    console.log(item)
}
```
#### 四、Generator.prototype.throw() 

Generator返回的遍历器对象都有一个throw方法，可在函数体外抛出错误，在Generator函数体内被捕获。

```
function* gen () {
    try {
        yield
    } catch (e) {
        console.log('内部捕获错误' + e)
    }
}

let g = gen()
g.next()
g.throw('a')
```
另外，throw方法被捕获后，会附带执行到下一条yield表达式。

```
function* gen () {
    try {
        yield console.log(1)
    } catch (e) {
        ....
    }
    yield console.log(2)
    yield console.log(3)
}
let g = gen()
g.next() // 1
g.throw // 2
g.next() // 3
```
如果Generator内部抛出的错误没在内部被捕获，则不会继续执行下去，下次调用next()方法会返回{value: undefined, done: true}

```
var gen = function* gen(){
  yield cosole('a')
  yield console.log('b');
  yield console.log('c');
}


var g = gen();
// g.next() // a
try {
  g.next()
} catch (e) {
  console.log('error console ' + e)
}
g.next()
```

#### 五 Generator.prototyoe.return
该方法会返回给定的值，并且终止函数的执行。

```
function* gen () {
    yield 1
    yield 2
    yield 3
    yield 4
}

let g = gen()
g.next() // {value: 1, done: false}
g.return('a') // {value: 'a', done: true}
```

