- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Iterator
- https://zh.javascript.info/iterable

可以应用 `for..of` 的对象被称为 **可迭代的**。

-   技术上来说，可迭代对象必须实现 `Symbol.iterator` 方法。
    -   `obj[Symbol.iterator]()` 的结果被称为 **迭代器（iterator）**。由它处理进一步的迭代过程。
    -   一个迭代器必须有 `next()` 方法，它返回一个 `{done: Boolean, value: any}` 对象，这里 `done:true` 表明迭代结束，否则 `value` 就是下一个值。
-   `Symbol.iterator` 方法会被 `for..of` 自动调用，但我们也可以直接调用它。
-   内建的可迭代对象例如字符串和数组，都实现了 `Symbol.iterator`。
-   字符串迭代器能够识别代理对（surrogate pair）。（译注：代理对也就是 UTF-16 扩展字符。）

有索引属性和 `length` 属性的对象被称为 **类数组对象**。这种对象可能还具有其他属性和方法，但是没有数组的内建方法。

`Array.from(obj[, mapFn, thisArg])` 将可迭代对象或类数组对象 `obj` 转化为真正的数组 `Array`，然后我们就可以对它应用数组的方法。可选参数 `mapFn` 和 `thisArg` 允许我们将函数应用到每个元素。

```js
// =>>> Symbol.iterator

console.debug('// =>>> function makeRangeV1(from, to)')

/**
 * 分离Symbol.iterator
 */
function makeRangeV1(from, to) {
  const range = {
    from,
    to,
  }

  // 1. for..of 调用首先会调用这个：
  range[Symbol.iterator] = function () {
    // ……它返回迭代器对象（iterator object）：
    // 2. 接下来，for..of 仅与下面的迭代器对象一起工作，要求它提供下一个值
    return {
      current: this.from,
      last: this.to,

      // 3. next() 在 for..of 的每一轮循环迭代中被调用
      next() {
        // 4. 它将会返回 {done:.., value :...} 格式的对象
        if (this.current <= this.last) {
          return {done: false, value: this.current++}
        } else {
          return {done: true}
        }
      },
    }
  }

  return range
}

for (let i of makeRangeV1(1, 5)) {
  console.debug(i)
}

console.debug('// =>>> function makeRangeV2(from, to)')

/**
 * 嵌入Symbol.iterator
 * 
 * 缺点：现在不可能同时在对象上运行两个 for..of 循环了：它们将共享迭代状态，
 * 因为只有一个迭代器，即对象本身。
 */
function makeRangeV2(from, to) {
  return {
    from,
    to,

    [Symbol.iterator]() {
      this.current = from
      return this
    },

    next() {
      if (this.current <= this.to) {
        return {done: false, value: this.current++}
      } else {
        return {done: true}
      }
    },
  }
}

for (let i of makeRangeV2(1, 5)) {
  console.debug(i)
}

console.debug('// =>>> function makeRangeV3(from, to)')
/**
 * 嵌入Symbol.iterator
 * 
 * 使用Closure变量
 */
function makeRangeV3(from, to) {
  let current = from
  return {
    from,
    to,

    [Symbol.iterator]() {
      return this
    },

    next() {
      if (current <= to) {
        return {done: false, value: current++}
      } else {
        return {done: true}
      }
    },
  }
}

for (let i of makeRangeV3(1, 5)) {
  console.debug(i)
}

console.debug('// =>>> function makeRangeV4(from, to)')

/**
 * 嵌入Symbol.iterator
 * 
 * generator实现
 * 
 * https://zh.javascript.info/generators
 */
function makeRangeV4(from, to) {
  return range = {
    from,
    to,

    *[Symbol.iterator]() {
      // [Symbol.iterator]: function*() 的简写形式
      for (let value = from; value <= to; value++) {
        yield value
      }
    },
  }
}

for (let i of makeRangeV4(1, 5)) {
  console.debug(i)
}

// =>>> 字符串迭代 - for..of

const str = 'Hello'

console.debug('// =>>> for..of字符串迭代')

for (let c of str) {
  console.debug(c)
}

// =>>> 显式调用迭代器

console.debug('=>>> 显式调用迭代器')

// 和 for..of 做相同的事
// for (let char of str) alert(char);

let iterator = str[Symbol.iterator]()

while (true) {
  let result = iterator.next()
  if (result.done) break
  console.debug(result.value) // 一个接一个地输出字符
}

// =>>> Array.from: 将可迭代对象转换为数组
console.debug('// =>>> Array.from: 将可迭代对象转换为数组')
let arrayLike = {
  0: 'Hello',
  1: 'World',
  length: 2,
}

let arr = Array.from(arrayLike) // (*)
for (let o of arr) {
  console.debug(o)
}

arr = Array.from(makeRangeV4(1, 5))
for (let o of arr) {
  console.debug(o)
}
```