---
title: 关于JS中函数内部this的指向
date: 2020-06-04 14:33:56
tags: [前端,javaScript]
---

### 一、this

- this是JavaScript中的一个关键字，当一个函数被调用时，除了传入函数的显式参数以外，名为this的隐式参数也被传入了函数。
- this参数指向了一个自动生成的内部对象，这个内部对象被称为`函数上下文`。
- JavaScript中的this依赖于函数的调用方式。

### 二、设计目的

- 由于函数可以在不同的运行环境执行，所以需要有一种机制，能够在函数体内部获得当前的运行环境（context）。
- 所以，this就出现了，它的设计目的就是在函数体内部，指代函数当前的运行环境。

### 三、this的指向

- this的指向是在函数执行时确定的，即：调用函数的那个对象。

### 四、普通场景

#### 1. 函数直接调用时（自然执行时）

> this指向全局或undefined。

- 自然执行：不被点出来/不被挂在对象上/不被访问控制符访问到。
- 指向全局：非严格模式下，在node环境下为global，在浏览器环境下为window。
- 指向undefined：严格模式下。

##### 例1：

``` javascript
  function work(){
    console.log('this:', this)
  }
  work() // this 指向 全局
```

##### 例2：

``` javascript
  var worker = {
    name: 'worker',
    work: function () {
      console.log('this:', this)
    }
  }
  var work = worker.work
  work() // this 指向 全局
```


#### 2.函数被对象调用时

> this指向点出它的那个对象

##### 例1：

``` javascript
  var worker = {
    name: 'worker',
    work: function () {
      console.log('this:', this)
    }
  }
  worker.work() // this 指向 worker
```

##### 例2：

``` javascript
  var worker = {
    name: 'worker',
    hand:{
      name:'hand',
      work: function () {
        console.log('this:', this)
      }
    }    
  }
  worker.hand.work() // this 指向 hand

  var workerWife = {}
  workerWife.work = worker.hand.work
  workerWife.work() // this 指向 workerWife

  var work = worker.hand.work
  work() // this 指向 全局
```

#### 3.new一个实例时

> new执行时this指向这个实例

##### 例：

``` javascript
  function Person(name){
    this.name = name
    console.log(this)
  }

  var worker = new Person('workers') // this 指向 worker 这个实例
```

#### 4.其他栗子

##### 例1：

``` javascript
  window.addEventListener('click', worker.work) // this 指向 window
```

##### 例2：

``` javascript
  (1,worker.work)() // this 指向 全局
```

##### *例3：
```javascript
function Person () {
  this.buy = function () {
    console.log('this::', this)
  }
}
var teacher = new Person()
wife.buy = teacher.buy
wife.buy() // this指向wife
```

### 五、特殊场景

#### 1.apply/call/bind时

> this指向第一个参数

##### apply

- 接收数组形式的参数
- 立即执行

```javascript
  function work(first, second){
    console.log('this', this, first, second)
  }
  var worker = { name: 'worker'}

  work.apply(worker, ['a', 'b']) // 会立即执行, this 指向 worker
```

##### call

- 分别接收参数
- 立即执行

```javascript
  function work(first, second){
    console.log('this', this, first, second)
  }
  var worker = { name: 'worker'}

  work.call(worker, 'a', 'b') // 会立即执行, this 指向 worker
```

##### bind

- 分别接收参数
- 不立即执行，返回一个新函数（不会改变原函数），这个函数绑定了新对象

```javascript
  function work(first, second){
    console.log('this', this, first, second)
  }
  var worker = { name: 'worker'}
  var workerWife = { name: 'workerWife'}

  var workerWork = work.bind(worker, 'a', 'b') // 不会立即执行
  workerWork() // this 指向 worker
  
  workerWife.workerWork = workerWork
  workerWife.workerWork() // this 依然指向 worker，不受前边点出它的对象影响

  // 也可以在bind的时候不传参，执行的时候传
  var workerWork = work.bind(worker) // 不会立即执行
  workerWork('a', 'b') // this 指向 worker
```

##### 对比

```javascript
  eat.call(dog, a, b) = eat.apply(dog, [a, b]) = (eat.bind(dog, a, b))() = dog.eat(a,b)
```

#### 2.箭头函数时

> 箭头函数的this指向：箭头函数`定义时` 离箭头函数最近的 非箭头函数的 执行上下文 的this。

##### 例1：

```javascript
  function outer(){
    console.log('outer this', this)

    const work = () => {
      console.log('work this', this)
    }
    work()
  }

  var worker = {
    name: 'worker'
  }

  worker.outer = outer
  worker.outer() // outer this 和 work this 都指向 worker
```

##### 例2：

```javascript
  function outer(){
    console.log('outer this', this)

    const work = () => {
      console.log('work this', this)
    }

    function inner(){
      console.log('inner this', this)
      work()
    }
    inner()
  }

  var worker = {
    name: 'worker'
  }

  worker.outer = outer
  worker.outer() // outer this -> worker,  inner this -> 全局, work this -> worker
```

#### 3.组合栗子

##### 例1：

```javascript
  const work = () => {
    console.log('this', this)
  }

  var worker = {
    name: 'worker'
  }

  work.call(worker) // this 指向 全局
```

##### 例2：

```javascript
function a() {
  setTimeout(() => {
      console.log('this1::',this);
  }, 1000);
  setTimeout(function () {
      console.log('this2::',this);
  }, 1000);
}
var b = {name:'张三'}
a.apply(b) // this1指向b，this2指向全局
```

#### 4.class静态方法中的this

> class静态方法中的this指向类，而不是实例。

##### 静态方法

- 类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

- 如果在实例上调用静态方法，会抛出一个错误，表示不存在该方法。

##### 例：

```javascript
class Person{
  static eat(){
    console.log('this',this)
  }
}
Person.eat() // this指向Person
```

### 六、参考资料

- 《JavaScript this 讲解》 —— https://zhuanlan.zhihu.com/p/23023311
- 《JavaScript 的 this 原理》 阮一峰 —— http://www.ruanyifeng.com/blog/2018/06/javascript-this.html
- 《ECMAScript 6 入门》 阮一峰 —— https://es6.ruanyifeng.com/#docs/class#静态方法
