

### [事件循环：微任务和宏任务](https://zh.javascript.info/event-loop)

```js
/**
 * https://zh.javascript.info/event-loop
 */

console.debug('// =>>> 用例 1：拆分 CPU 过载任务')
if (0) {
  {
    let i = 0

    function count() {
      console.time('count')

      // 做一个繁重的任务
      for (let j = 0; j < 1e9; j++) {
        i++
      }

      console.timeEnd('count')
    }

    count()
  }

  {
    let i = 0

    console.time('count-v2')

    function count() {
      // 将调度（scheduling）移动到开头
      if (i < 1e9 - 1e6) {
        setTimeout(count) // 安排（schedule）新的调用
      }

      do {
        i++
      } while (i % 1e6 != 0)

      if (i == 1e9) {
        console.timeEnd('count-v2')
      }
    }

    count()
  }
}

// =>>>
{
  /**
   * - code 首先显示，因为它是常规的同步调用。
   * - promise 第二个出现，因为 then 会通过微任务队列，并在当前代码之后执行。
   * - timeout 最后显示，因为它是一个宏任务。
   */
  setTimeout(() => console.debug('timeout'))

  setImmediate(() => {
    console.debug('setImmediate')
  })

  queueMicrotask(() => {
    console.debug('queueMicrotask')
  })

  process.nextTick(() => {
    console.debug('process.nextTick')
  })

  Promise.resolve().then(() => console.debug('promise'))

  console.debug('code')
}
```

### 自定义事件

