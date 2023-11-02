## 1. 概述
### 参考资料
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator
- https://zh.javascript.info/generators

## 2. 快速入门

### 2.1. 生成器函数
`Generator` 的实例必须从[生成器函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function*)返回

```js
  function* generator() {
    yield 1;
    yield 2;
    yield 3;
  }
```

### 2.2. 实例方法

#### 2.2.1. next()

```js
  let gen = generator();
  
  console.log(gen.next());    // {value: 1, done: false}
  console.log(gen.next());    // {value: 2, done: false}
  console.log(gen.next());    // {value: 3, done: false}
  console.log(gen.next());    // {value: undefined, done: true}
  console.log(gen.next());    // {value: undefined, done: true}
```

#### 2.2.2. return()
  
```js
  gen = generator();

  console.log(gen.next());    // {value: 1, done: false}
  console.log(gen.return(9)); // {value: 9, done: true}
  console.log(gen.next());    // {value: undefined, done: true}
  console.log(gen.next());    // {value: undefined, done: true}
```
  
#### 2.2.3. throw()

##### 内部try-catch

```js
function* gen() {
  try {
    let result = yield '2 + 2 = ?' // (1)
    console.debug('The execution does not reach here, because the exception is thrown above')
  } catch (e) {
    console.debug('#generator#error#', e.message)
  }
}

let generator = gen()
let question = generator.next().value
console.debug(question)
generator.throw(new Error('The answer is not found in my database'))
```

**输出**

```sh
2 + 2 = ?
#generator#error# The answer is not found in my database
```

##### 外部try-catch

```js
 function* generate() {
  let result = yield '2 + 2 = ?'
}

let generator = generate()
let question = generator.next().value
console.debug(question)
try {
  generator.throw(new Error('The answer is not found in my database'))
} catch (e) {
  console.debug('#block#error#', e.message)
}
```

**输出**

```sh
2 + 2 = ?
#block#error# The answer is not found in my database
```

### 2.3. yield*

yield* 指令将执行 委托 给另一个 generator

```js
function* generateSequence(start, end) {
  for (let i = start; i <= end; i++) yield i
}

// 使用 yield* 这个特殊的语法来将一个 generator “嵌入”（组合）到另一个 generator 中
function* generatePasswordCodes() {
  // 0..9
  yield* generateSequence(48, 57)

  // A..Z
  yield* generateSequence(65, 90)

  // a..z
  yield* generateSequence(97, 122)
}

let str = ''

for (let code of generatePasswordCodes()) {
  str += String.fromCharCode(code)
}
console.debug(str)
```

**输出**

```sh
0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
```