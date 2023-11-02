https://zh.javascript.info/proxy
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy

Proxy 对象使您能够为另一个对象创建代理，它可以拦截和重新定义该对象的基本操作。


## 描述

```js
let proxy = new Proxy(target, handler)
```

- target：要代理的原始对象
- handler：一个对象，定义了哪些操作将被拦截，以及如何重新定义被拦截的操作。


## 可拦截操作

### get

`get(target, property, receiver)`：

-   `target` —— 是目标对象，该对象被作为第一个参数传递给 `new Proxy`，
-   `property` —— 目标属性名，
-   `receiver` —— 如果目标属性是一个 getter 访问器属性，则 `receiver` 就是本次读取属性所在的 `this` 对象。通常，这就是 `proxy` 对象本身（或者，如果我们从 proxy 继承，则是从该 proxy 继承的对象）。现在我们不需要此参数，因此稍后我们将对其进行详细介绍。

**示例**

```js
let numbers = [0, 1, 2]

numbers = new Proxy(numbers, {
  get(target, prop) {
    if (prop in target) {
      return target[prop]
    } else {
      return 0 // 默认值
    }
  },
})

console.debug(numbers[1]) // 1
console.debug(numbers[123]) // 0（没有这个数组项）
```

### set

`set(target, property, value, receiver)`：

-   `target` —— 是目标对象，该对象被作为第一个参数传递给 `new Proxy`，
-   `property` —— 目标属性名称，
-   `value` —— 目标属性的值，
-   `receiver` —— 与 `get` 捕捉器类似，仅与 setter 访问器属性相关。

**示例**

```js
let numbers = []

numbers = new Proxy(numbers, {
  set(target, prop, val) {
    if (typeof val === 'number') {
      target[prop] = val
      return true
    } else {
      return false
    }
  },
})

try {
  numbers.push(1) // 添加成功
  numbers.push(2) // 添加成功
  console.debug('Length is: ' + numbers.length) // 2

  numbers.push('test') // TypeError（proxy 的 'set' 返回 false）

  console.debug('This line is never reached (error in the line above)')
} catch (error) {
  console.debug(error.message)
}
```

### ownKeys

**示例**

```js
console.debug('\n=>>> 跳过以下划线 _ 开头的属性')
{
  let user = {
    name: 'John',
    age: 30,
    _password: '***',
  }

  user = new Proxy(user, {
    ownKeys(target) {
      return Object.keys(target).filter(key => !key.startsWith('_'))
    },
  })

  console.debug('user:', user)

  // "ownKeys" 过滤掉了 _password
  for (let key in user) console.debug('key:', key)

  // 对这些方法的效果相同：
  console.debug('Object.keys(user):', Object.keys(user))
  console.debug('Object.values(user):', Object.values(user))
}
```
但如果我们返回对象中不存在的键，Object.keys 并不会列出这些键，原因很简单：Object.keys 仅返回带有 enumerable 标志的属性。为了检查它，该方法会对每个属性调用内部方法 **GetOwnProperty**来获取 它的描述符（descriptor）。

```js
console.debug('\n=>>> 如果我们返回对象中不存在的键，Object.keys 并不会列出这些键')
{
  let user = {}

  user = new Proxy(user, {
    ownKeys(target) {
      return ['a', 'b', 'c']
    },
  })
  console.debug('user:', user)
  console.debug('Object.keys(user):', Object.keys(user))
}
```

### deleteProperty

`deleteProperty(target, property) `

**示例**

```js
et user = {
  name: 'John',
  _password: 'password',
  checkPassword(value) {
    //对象方法必须能读取 _password
    return value === this._password
  },
}

user = new Proxy(user, {
  // 拦截属性读取
  get(target, prop) {
    if (prop.startsWith('_')) {
      throw new Error('Access denied: read')
    }
    let value = target[prop]
    return typeof value === 'function' ? value.bind(target) : value // (*)
  },
  // 拦截属性写入
  set(target, prop, val) {
    if (prop.startsWith('_')) {
      throw new Error('Access denied: write')
    } else {
      target[prop] = val
      return true
    }
  },
  // 拦截属性删除
  deleteProperty(target, prop) {
    if (prop.startsWith('_')) {
      throw new Error('Access denied: delete')
    } else {
      delete target[prop]
      return true
    }
  },
  // 拦截读取属性列表
  ownKeys(target) {
    return Object.keys(target).filter(key => !key.startsWith('_'))
  },
})

// "get" 不允许读取 _password
try {
  console.debug(user._password) // Error: Access denied
} catch (e) {
  console.debug(e.message)
}

// "set" 不允许写入 _password
try {
  user._password = 'test' // Error: Access denied
} catch (e) {
  console.debug(e.message)
}

// "deleteProperty" 不允许删除 _password
try {
  delete user._password // Error: Access denied
} catch (e) {
  console.debug(e.message)
}

// "ownKeys" 将 _password 过滤出去
for (let key in user) console.debug(key) // name

console.debug('user.checkPassword("password"):', user.checkPassword('password'))
```

请注意在 (\*) 行中 get 捕捉器的重要细节：我们在 (\*) 行中将对象方法的上下文绑定到原始对象 target，以便可以访问_password。

### has

`has(target, property)`

-   `target` —— 是目标对象，被作为第一个参数传递给 `new Proxy`，
-   `property` —— 属性名称。

**示例**

```js
let range = {
  start: 1,
  end: 10,
}

range = new Proxy(range, {
  has(target, prop) {
    return prop >= target.start && prop <= target.end
  },
})

console.debug(5 in range) // true
console.debug(50 in range) // false
```

### apply

`apply(target, thisArg, args)`

- target 是目标对象（在 JavaScript 中，函数就是一个对象），
- thisArg 是 this 的值。
- args 是参数列表。

**示例**

```js
/**
 * 基于函数的实现：
 *
 * 但是包装函数不会转发属性读取/写入操作或者任何其他操作。进行包装后，
 * 就失去了对原始函数属性的访问，例如 name，length 和其他属性。
 */
{
  function delay(f, ms) {
    // 返回一个包装器（wrapper），该包装器将在时间到了的时候将调用转发给函数 f
    return function () {
      // (*)
      setTimeout(() => f.apply(this, arguments), ms)
    }
  }

  function sayHi(user) {
    console.debug(`Hello, ${user}!`)
  }

  // 在进行这个包装后，sayHi 函数会被延迟 3 秒后被调用
  sayHi = delay(sayHi, 3000)
  console.debug(sayHi.length) // 0（在包装器声明中，参数个数为 0)

  sayHi('John') // Hello, John! (after 3 seconds)
}

// 使用 Proxy 来替换掉包装函数
{
  function delay(f, ms) {
    return new Proxy(f, {
      apply(target, thisArg, args) {
        setTimeout(() => target.apply(thisArg, args), ms)
      },
    })
  }

  function sayHi(user) {
    console.debug(`Hello, ${user}!`)
  }

  sayHi = delay(sayHi, 3000)
  console.debug(sayHi.length) // 1 (*) proxy 将“获取 length”的操作转发给目标对象

  sayHi('John') // Hello, John!（3 秒后）
}
```

## private属性拦截

```js
console.debug('\n // =>>> No private property forwarding')
{
  class Secret {
    #secret
    constructor(secret) {
      this.#secret = secret
    }
    get secret() {
      return this.#secret.replace(/\d+/, '[REDACTED]')
    }
  }

  const aSecret = new Secret('123456')
  console.log(aSecret.secret) // [REDACTED]
  {
    // Looks like a no-op forwarding...
    try {
      const proxy = new Proxy(aSecret, {})
      console.log(proxy.secret) // TypeError: Cannot read private member #secret from an object whose class did not declare it
    } catch (error) {
      console.debug('[error]proxy.secret', error.message)
    }
  }

  {
    const proxy = new Proxy(aSecret, {
      get(target, prop, receiver) {
        // By default, it looks like Reflect.get(target, prop, receiver)
        // which has a different value of `this`
        return target[prop]
      },
    })
    console.log('proxy.secret', proxy.secret)
  }

  // 对于一些方法，这意味着您还必须将方法的 this 值重定向到原始对象：
  {
    class Secret {
      #x = 1
      x() {
        return this.#x
      }
    }

    const aSecret = new Secret()
    const proxy = new Proxy(aSecret, {
      get(target, prop, receiver) {
        const value = target[prop]
        if (value instanceof Function) {
          return function (...args) {
            return value.apply(this === receiver ? target : this, args)
          }
        }
        return value
      },
    })
    console.log('proxy.x()', proxy.x())
  }
}
```