---
title: TypeScript基础
date: 2023-07-09 16:31:55
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202307101641411.webp
tags: 
  - 前端
  - TypeScript
updated:
categories: 学习笔记
description: Java前端技术基础，TypeScript
mathjax:
toc: true
toc_number: true
---

# 动态类型的问题

 js 属于动态类型语言，例如

```js
function test(obj) {    
}
```

obj 可能只是个字符串

```js
test('hello, world')
```

obj 也有可能是个函数

```js
test(()=>console.log('hello, world'))
```

obj 类型不确定，就给后期使用者带来了麻烦，一旦参数传不对，代码就崩溃了



动态类型意味着

* 运行代码时才知道发生什么 (running the code to see what happens)

静态类型意味着

* 在代码运行前，就对它的行为做出预测 (make predications about what code is expected before it runs)



下面的 typescript 代码，就在代码运行前对参数加入了约束限制

```typescript
function test(msg : string) {
}
```

* 限制了参数只能做 string 那些事

```typescript
function test(msg : Function) {
  msg()
}
```

* 限制了参数只能做函数那些事

<br>

# 入门

安装 typescript 编译器

```cmd
npm install -g typescript
```

编写 ts 代码

```typescript
function hello(msg: string) {
  console.log(msg)
}

hello('hello,world')
```

执行 tsc 编译命令

```cmd
tsc xxx.ts
```

编译生成 js 代码，编译后进行了类型擦除

```js
function hello(msg) {
    console.log(msg);
}
hello('hello,world');
```



再来一个例子，用 interface 定义用户类型

```ts
interface User {
  name: string,
  age: number
}

function test(u: User): void {
  console.log(u.name)
  console.log(u.age)
}

test({ name: 'zhangs', age: 18 })
```

编译后

```js
function test(u) {
    console.log(u.name);
    console.log(u.age);
}
test({ name: 'zhangs', age: 18 });
```

可见，typescript 属于编译时实施类型检查（静态类型）的技术

<br>

# 类型

| 类型        | 例                                    | 备注                         |
| ----------- | ------------------------------------- | ---------------------------- |
| 字符串类型  | string                                |                              |
| 数字类型    | number                                |                              |
| 布尔类型    | boolean                               |                              |
| 数组类型    | number[],string[], boolean[] 依此类推 |                              |
| 任意类型    | any                                   | 相当于又回到了没有类型的时代 |
| 复杂类型    | type 与 interface                     |                              |
| 函数类型    | () => void                            | 对函数的参数和返回值进行说明 |
| 字面量类型  | "a"\|"b"\|"c"                         | 限制变量或参数的取值         |
| nullish类型 | null 与 undefined                     |                              |
| 泛型        | `<T>`，`<T extends 父类型>`           |                              |

<br>

## 标注位置

**标注变量**

```typescript
let message: string = 'hello,world'
```

* 一般可以省略，因为可以根据后面的字面量推断出前面变量类型

```typescript
let message = 'hello,world'
```

<br>

**标注参数**

```typescript
function greet(name: string) {
    
}
```

很多时候，都能够推断出参数类型

```typescript
const names = ['Alice', 'Bob', 'Eve']
const lowercaseNames = names.map((e: string) => e.toLowerCase())
```

* 可以用类型推断，推断出 e 是 string 类型

<br>

**标注返回值**

```typescript
function add(a: number, b: number) : number {
    return a + b
}
```

* 一般也可以省略，因为可以根据返回值做类型推断

<br>

## 复杂类型

**type**

```typescript
type Cat = {
  name: string,
  age: number
}

const c1: Cat = { name: '小白', age: 1 }
const c2: Cat = { name: '小花' }					  // 错误: 缺少 age 属性
const c3: Cat = { name: '小黑', age: 1, sex: '公' } // 错误: 多出 sex 属性
```

<br>

**interface**

```typescript
interface Cat {
  name: string,
  age: number
}

const c1: Cat = { name: '小白', age: 1 }
const c2: Cat = { name: '小花' }					  // 错误: 缺少 age 属性
const c3: Cat = { name: '小黑', age: 1, sex: '公' } // 错误: 多出 sex 属性
```

<br>

**可选属性**

如果需要某个属性可选，可以用下面的语法

```typescript
interface Cat {
  name: string,
  age?: number
}

const c1: Cat = { name: '小白', age: 1 }
const c2: Cat = { name: '小花' }					  // 正确: age 属性可选
```

* 可选属性要注意处理 undefined 值

<br>

**鸭子类型**

```typescript
interface Cat {
  name: string
}

function test(cat: Cat) {
  console.log(cat.name)
}

const c1 = { name: '小白', age: 1 } 
test(c1)
```

* const c1 并没有声明类型为 Cat，但它与 Cat 类型有一样的属性，也可以被当作是 Cat 类型

<br> 

## 方法类型

```typescript
interface Api {
  foo(): void,
  bar(str: string): string
}

function test(api: Api) {
  api.foo()
  console.log(api.bar('hello'))
}

test({
  foo() { console.log('ok') },
  bar(str: string) { return str.toUpperCase() }
})
```



## 字面量类型

```typescript
function printText(s: string, alignment: "left" | "right" | "center") {
  console.log(s, alignment)
}

printText('hello', 'left')
printText('hello', 'aaa') // 错误: 取值只能是 left | right | center
```



## nullish 类型

```typescript
function test(x?: string | null) {
  console.log(x?.toUpperCase())
}

test('aaa')
test(null)
test()
```

* x?: string | null 表示可能是 undefined 或者是 string 或者是 null



## 泛型

下面的几个类型声明显然有一定的相似性

```typescript
interface RefString {
  value: string
}

interface RefNumber {
  value: number
}

interface RefBoolean {
  value: boolean
}

const r1: RefString = { value: 'hello' }
const r2: RefNumber = { value: 123 }
const r3: RefBoolean = { value: true }
```

可以改进为

```typescript
interface Ref<T> {
  value: T
}

const r1: Ref<string> = { value: 'hello' }
const r2: Ref<number> = { value: 123 }
const r3: Ref<boolean> = { value: true }
```

* 泛型的要点就是 `<类型参数>`，把【类型】也当作一个变化的要素，像参数一样传递过来，这样就可以派生出结构相似的新类型



函数定义也支持泛型

```typescript
function ref<T>(n: T): Ref<T> {
  return { value: n }
}

const v1 = ref("hello"); 	// Ref<string>
const v2 = ref(123.3333);	// Ref<number>

v1.value.toLocaleLowerCase()
v2.value.toFixed(2)
```

<br>

# 意义

## 更好理解框架

现在越来越多的前端框架采用 typescript，如果懂 typescript 语法，可以更好地阅读框架代码

以 Map 为例

```typescript
const map = new Map<string, string>()
map
  .set("a", "b")
  .set("c", "d")

map.forEach((value,key,m)=>{
  console.log(value, key)
})
```

* 注意编译需要 `tsc --target es6 .\xxx.ts`

<br>

## 更好的提示

例如，从服务器返回的一段 json，如果不用 typescript，则编辑器也不能给出准确的提示

```typescript
interface User {
  name: string,
  age: number
}

const user: User = JSON.parse(`{ "name":"张三", "age":18 }`)
```

<br>

# 类

> 关于 TypeScript 与 JavaScript 中的类语法不是重点，class 相关语法只是起到辅助作用，更重要的是前面讲的 interface

## 基本语法

```typescript
class User {
    name: string;
    
    constructor(name: string) {
        this.name = name
    }
}

const u = new User('张三')
```

其实会被编译成这个样子（默认 --target=es3）

```js
var User = /** @class */ (function () {
    function User(name) {
        this.name = name;
    }
    return User;
}());
var u = new User('张三');
```

所以 js 中的 class，并不等价于 java 中的 class，它还是基于原型实现的。

<br>

## 只读属性

```typescript
class User {
  readonly name: string;
  
  constructor(name: string) {
      this.name = name
  }
}

const u = new User('张三')
u.name = '李四'				// 编译错误
```

* readonly 是 typescript 特有的，表示该属性只读

<br>

## 方法

```typescript
class User {
  readonly name: string;
  
  constructor(name: string) {
      this.name = name
  }

  study() {
    console.log(`[${this.name}]正在学习`)
  }
}

const u = new User('张三')
u.study()
```

<br>

## get，set

```typescript
class User {
  _name: string;

  constructor(name: string) {
    this._name = name
  }

  get name() {
    return this._name
  }

  set name(name: string) {
    this._name = name
  }
}

const u = new User('张三')
console.log(u.name)
u.name = '李四'
console.log(u.name)
```

* 注意，需要在编译时加上 `tsc --target es6 .\xxx.ts` 选项
* es6 等价于 es2015，再此之上还有 es2016 ... es2022

<br>

## 类与接口

```typescript
interface User {
  name: string
  study(course: string): void
}

class UserImpl implements User {
  name: string;
  constructor(name: string) {
    this.name = name
  }
  study(course: string) {
    console.log(`[${this.name}]正在学习[${course}]`)
  }
  foo() { }
}

const user: User = new UserImpl('张三')
user.study('Typescript')
user.foo() // 错误，必须是接口中定义的方法
```

<br>

## 继承与接口

```typescript
interface Flyable {
  fly(): void
}

class Animal {
  name: string;
  constructor(name: string) {
    this.name = name
  }
}

class Bird extends Animal implements Flyable {
  fly() {
    console.log(`${this.name}在飞翔`)
  }
}

const b: Flyable & Animal = new Bird("小花")
b.fly()
```

* Flyable & Animal 表示变量是 flyable 类型，同时也是 Animal 类型

<br>

## 方法重写

```typescript
class Father {
  study(): void {
    console.log(`father study`)
  }
}

class Son extends Father {  
  study(): void {
    super.study()
    console.log(`son study`)
  }
}

const f: Father = new Son()
f.study()
```

<br>