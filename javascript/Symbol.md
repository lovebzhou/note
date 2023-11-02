https://zh.javascript.info/symbol

```js
/**
 * https://zh.javascript.info/symbol
 */

let id1 = Symbol('id')
let id2 = Symbol('id')

console.debug('id1', id1)
console.debug('id1.toString()', id1.toString())
console.debug('id1.description', id1.description)
console.debug('id1 === id2', id1 === id2)

console.debug('\n// =>>> “隐藏”属性：不会被意外破坏原对象属性')
{
  let user = {
    // 属于另一个代码
    name: 'John',
  }

  let id = Symbol('id')

  user[id] = 1

  console.debug('user[id]', user[id]) // 我们可以使用 symbol 作为键来访问数据
  console.debug('user', user)
}

console.debug('\n// =>>> 对象字面量中的 symbol')
{
  let id = Symbol('id')

  let user = {
    name: 'John',
    [id]: 123, // 而不是 "id"：123
  }
  console.debug('user', user)
}

console.debug('\n// =>>> symbol 在 for…in 中会被跳过')
{
  let id = Symbol('id')
  let user = {
    name: 'John',
    age: 30,
    [id]: 123,
  }

  for (let key in user) console.debug('#in#', key, user[key]) // name, age（没有 symbol）

  console.debug('Object.keys(user):', Object.keys(user))
  // 使用 symbol 任务直接访问
  console.debug('#direct#', 'user[id]: ' + user[id]) // Direct: 123

  let clone = Object.assign({}, user)

  console.debug('#clone#', clone) // 123
}

console.debug('\n// =>>> 全局 symbol')
{
  // 从全局注册表中读取
  let id = Symbol.for('id') // 如果该 symbol 不存在，则创建它

  // 再次读取（可能是在代码中的另一个位置）
  let idAgain = Symbol.for('id')

  // 相同的 symbol
  console.debug('id === idAgain', id === idAgain) // true
}

console.debug('\n// =>>> Symbol.keyFor')
{
  // 通过 name 获取 symbol
  let sym = Symbol.for('name')
  let sym2 = Symbol.for('id')
  let localSymbol = Symbol('name')

  // 通过 symbol 获取 name
  console.debug('Symbol.keyFor(sym):', Symbol.keyFor(sym)) // name
  console.debug('Symbol.keyFor(sym2):', Symbol.keyFor(sym2)) // id
  console.debug('Symbol.keyFor(localSymbol):', Symbol.keyFor(localSymbol))
}
```