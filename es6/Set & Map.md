#### 一、Set实例方法
Set实例的属性

Set.property.constructor 返回Set构造函数

Set.property.size  返回Set实例的成员总数

##### 1、操作方法

> add(value) 添加值，返回Set实例本身
> delete(value) 删除值，返回一个布尔值表示删除成功
> has(value) 返回一个布尔值，表示该值是否是Set成员
> clear() 清空所有成员，无返回值


```
let set = new Set()
set.add(1).add(2).add(3)
s.size // 3
s.has(1) // true
s.has(4) //false
s.delete(2) // true
s.has(2) // true
```
*数组去重   

```
[...new Set(array)]  // Set结构不存在全相等的两个成员
```
##### 2、遍历方法
> keys() 遍历键名
> value() 遍历键值
> entires() 返回键值对的遍历器
> forEach() 使用回调遍历每个成员

##### 3、通过扩展运算符实现数组的并集 交集 差集

```
let a = new Set([1, 3, 5])
let b = new Set([4, 7, 9])

// 并集
let collectionSet = [...a,...b]

// 交集
let intersetSet = [...a].filter(value => b.has(value))

//差集
let differenceSet = [...a].filter(value => !b.has(value))

```

##### 4、改变Set结构
###### 1、通过扩展运算符

```
let setNew = new Set([...setOld].map(value) => return{...} )
```
###### 2、通过Array.from

```
let setNew = new Set(Array.from(setOld, (value) => return{...} )

// Array.from 的用法
Array.from(arrayLike, mapFn, thisArg)
// arrayLike 想要转为数组的伪数组对象或可迭代对象
// mapFn 新数组中每一项都会执行的回调函数
// thisArg 执行回调函数mapFn时的this对象
// 返回值是一个数组
```

#### 二、WeakSet
WeakSet与set结构类似，也是不重复的值的集合，但是有2个区别
##### 1、WeakSet成员只能是对象

```
const ws = new WeakSet()
ws.add(1) // TypeError
```
##### 2、WeakSet中的对象是弱引用

WeakSet中的对象是弱引用不在垃圾回收机制考虑中，只要WeakSet中的对象不被其他对象引用，垃圾回收机制就会自动回收WeakSet中的该对象。因此，WeakSet适合保存临时对象，以避免产生内存泄漏。

由于WeakSet是弱引用，无法遍历，所以WeakSet无size、forEach 属性、方法。

注：垃圾回收机制依赖引用计数，如果一个值的引用次数不为0，垃圾回收机制就不会释放这块内存，在结束使用该值后，如果忘记取消对其的引用，导致该内存无法被释放，既发生内存泄漏。

##### 3、WeakSet应用

```
const foos = new WeakSet()

class foo {
    constructor () {
        foos.add(this)
    }
    method () {
        if (!foo.has(this) {
            therow new TypeError('foo.prototype.method只能在foo实例上调用！')
        }
    }
}
// 采用WeakSet的好处是删除foo实例时，不用考虑foos，也不会出现内存泄漏。
```
#### 三、Map

##### 1、基本含义和用法
Map是一种键值对结构，对象的键只能是字符串，但Map的键可以是各种类型。

Map的键是和内存地址绑定的，只要内存地址不一样，就视为不同的键。如果Map的键是一个简单类型，那只要完全相等就视为同一个键。

```
let m = new Map()
map.set([a], 444)
map.get([a]) // undefined
```

##### 2、实例属性、方法
###### 1、size 

```
let m = new Map ()
m.set('a', 3)
m.size // 1
```
###### 2、set(key, value)

如果重复对同一键赋值，后者会替换后者。

###### 3、get(key)
如果键值不存在，返回undefined

###### 4、has(key)
返回布尔值

###### 5、delete(kye)
返回布尔值

###### 6、clear() 
无返回值

##### 3、遍历方法
> keys()  返回所有键名
> values() 返回所有值
> entires() 返回所有键值对
> forEach()  遍历所有成员

```
const map = new Map([3:4, 'a':44])

for (let item of map.keys()) {
    console.log(item)
}

for (let item of map.values()) {
    console.log(item)
}

for (let item of map.entires()) {
    cosnole.log(item[0],item[1])
}
// 或
for (let [key, vlaue] of map.entires()) {
    console.log(key, value)
}
// 或
for (let [key, value] of map) {
    console.log(key, value)
}
// map默认的遍历方法就是entires()
```


