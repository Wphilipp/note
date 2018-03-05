#### 一、什么是Symbol
##### 1、Symbol是ES6新增的一种基本数据类型，表示独一无二的值。

##### 2、Symbol()可以接受一个字符串参数作为对Symbol实例的描述。目的是转为字符串时比较容易区分。

```
let s = Symbol('foo')
console.log(s) // Symbol(foo)
```
##### 3、如果Symbol()接受的是一个对象，会调用该对象的toString方法，将其转为字符串，然后才生成一个Symbol值。

由于Symbol()接受的参数只是起描述作用，因此，即使传入相同参数的Symbol() 实力后的变量也是不相等的。

```
let s1 = Symbol('foo')
let s2 = Symbol('bar')

console.log(s1 === s2)
```

##### 4、Symbol()值不能与其他值进行运算

```
let s1 = Symbol('foo')
console.log('this is a' + s1)  // 报错
```
##### 5、Symbol()值可以转为字符串或布尔值

```
let s1 = Symbol('foo') 
console.log(String(s1) // Symbol(foo)
console.log(s1.toString())  // Symbol(foo)

let s1 = Symbol('foo')
console.log(Boolean(s1)) // true
```
#### 二、作为属性名
由于Symbol的每个值都是不相等的，适合作为标识符，因此将其作为对象的属性名可以保证不会出现属性名冲突。

```
let name = Symbol()

let a = {
    [name]: 'hello'
}

// 或
let a = {}
Object.defineProperty(a, mySymbol, {value: 'hello'})

console.log(a[name]) // hello
```

注：Symbol做对象属性名时，不能用点运算符。

#### 三、消除魔术字符串
魔术字符串是指在代码中多次出现，与代码强耦合的某个具体的字符串或数值，这种情况不利于后期维护和修改。


```
const shapeType = {
    triangle: Symbol()
}

function getArea (shape, options) {
    let area = 0
    switch (shape) {
        case shape[triangle]:
          area = .5*options.width*options.height
          break
    }
    return area
}

getArea(shapeType[triangle], {width: 299, height: 422})

// 对象解构写法
function getArea ([shape, {width, height}]) {
    let area = 0
    switch (shape) {
        case shape[triangle]:
          area = .5*options.width*options.height
          break
    }
    return area
}

getArea([shapeType[triangle], {width: 299, height: 422}])

```

#### 4、属性名的遍历
Symbol作为属性名，不会出现在for-in  for-of 的循环中，也不会被Object.keys()  Object.getOwnPropertyNames() JSON.stringify() 返回。 但是通过Object.getOwnPropertySymbols()方法可以返回该对象的所有Symbol属性。

```
const obj = {}
let name = Symbol('name')
let age = Symbol('age')

obj[a] = 'hello'
obj[b] = 'world'

const SymbolPropertyArray = Object.getOwnPropertySymbol(obj)
console.log(SymbolPropertyArray) // [Symbol(name), Symbol(age)]
```

#### 5、Symbol.for() Symbol.keyFor()
Symbol.for() 接受一个字符串作为参数，先搜索有没有以该参数作为名称的Symbol值，如果有，就返回这个Symbol值，否则就新建并返回一个以该字符串为名称的Symbol值。该方法生成的Symbol值会被登记在全局环境中供搜索。

Symbol.keyFor() 返回一个已登记的Symbol类型的名称


```
let s1 = Symbol.for('name')
let s2 = Symbol.for('name')

s1 === s2 // true

let key = Symbol.keyFor(s2) // name
```

```

```

let s1 = Symbol.for()
