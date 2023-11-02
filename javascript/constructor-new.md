https://zh.javascript.info/constructor-new

### 构造函数

构造函数在技术上是常规函数。不过有两个约定：

1.  它们的命名以大写字母开头。
2.  它们只能由 `"new"` 操作符来执行。

```js
function User(name) {
  // this = {};（隐式创建）

  // 添加属性到 this
  this.name = name;
  this.isAdmin = false;

  // return this;（隐式返回）
}

let user = new User("Jack");

```

这是构造器的主要目的 —— 实现可重用的对象创建代码。这比每次都使用字面量创建要短得多，而且更易于阅读。

### new function() { … }

如果我们有许多行用于创建单个复杂对象的代码，我们可以将它们封装在一个立即调用的构造函数中，像这样：

```js
// 创建一个函数并立即使用 new 调用它
let user = new function() {
  this.name = "John";
  this.isAdmin = false;

  // ……用于用户创建的其他代码
  // 也许是复杂的逻辑和语句
  // 局部变量等
};
```

这个构造函数不能被再次调用，因为它不保存在任何地方，只是被创建和调用。因此，这个技巧旨在封装构建单个对象的代码，而无需将来重用。

### 构造器模式测试

在一个函数内部，我们可以使用 `new.target` 属性来检查它是否被使用 `new` 进行调用了。

```js
  function User() {
    console.debug('new.target:', new.target)
  }

  // 不带 "new"：
  User() // undefined

  // 带 "new"：
  new User() // function User { ... }
  ```

### 构造器return

-   如果 `return` 返回的是一个对象，则返回这个对象，而不是 `this`。
-   如果 `return` 返回的是一个原始类型，则忽略。

```js
function BigUser(name) {
  this.name = 'John'
  this.age = 18

  return {name, gender: 'male'} // <-- 返回这个对象
}
```

### 总结

-   构造函数，或简称构造器，就是常规函数，但大家对于构造器有个共同的约定，就是其命名首字母要大写。
-   构造函数只能使用 `new` 来调用。这样的调用意味着在开始时创建了空的 `this`，并在最后返回填充了值的 `this`。