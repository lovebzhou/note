https://mochajs.org

## INTERFACES
### BDD

The **BDD** interface provides `describe()`, `context()`, `it()`, `specify()`, `before()`, `after()`, `beforeEach()`, and `afterEach()`.

`context()` is just an alias for `describe()`, and behaves the same way; it provides a way to keep tests easier to read and organized. Similarly, `specify()` is an alias for `it()`.
```js
describe('Array', function () {
  before(function () {
    // ...
  });

  describe('#indexOf()', function () {
    context('when not present', function () {
      it('should not throw an error', function () {
        (function () {
          [1, 2, 3].indexOf(4);
        }.should.not.throw());
      });
      it('should return -1', function () {
        [1, 2, 3].indexOf(4).should.equal(-1);
      });
    });
    context('when present', function () {
      it('should return the index where the element first appears in the array', function () {
        [1, 2, 3].indexOf(3).should.equal(2);
      });
    });
  });
});
```

### TDD

The **TDD** interface provides `suite()`, `test()`, `suiteSetup()`, `suiteTeardown()`, `setup()`, and `teardown()`:
```js
suite('Array', function () {
  setup(function () {
    // ...
  });

  suite('#indexOf()', function () {
    test('should return -1 when not present', function () {
      assert.equal(-1, [1, 2, 3].indexOf(4));
    });
  });
});
```

### EXPORTS
The **Exports** interface is much like Mocha’s predecessor [expresso](https://github.com/tj/expresso). The keys `before`, `after`, `beforeEach`, and `afterEach` are special-cased, object values are suites, and function values are test-cases:
```js
module.exports = {
  before: function () {
    // ...
  },

  Array: {
    '#indexOf()': {
      'should return -1 when not present': function () {
        [1, 2, 3].indexOf(4).should.equal(-1);
      }
    }
  }
};
```
### QUNIT

The [QUnit](https://qunitjs.com/)-inspired interface matches the “flat” look of QUnit, where the test suite title is defined _before_ the test-cases. Like TDD, it uses `suite()` and `test()`, but resembling BDD, it also contains `before()`, `after()`, `beforeEach()`, and `afterEach()`.
```js
function ok(expr, msg) {
  if (!expr) throw new Error(msg);
}

suite('Array');

test('#length', function () {
  var arr = [1, 2, 3];
  ok(arr.length == 3);
});

test('#indexOf()', function () {
  var arr = [1, 2, 3];
  ok(arr.indexOf(1) == 0);
  ok(arr.indexOf(2) == 1);
  ok(arr.indexOf(3) == 2);
});

suite('String');

test('#length', function () {
  ok('foo'.length == 3);
});
```
### REQUIRE

The `require` interface allows you to require the `describe` and friend words directly using `require` and call them whatever you want. This interface is also useful if you want to avoid global variables in your tests.

_Note_: The `require` interface cannot be run via the `node` executable, and must be run via `mocha`.
```js
var testCase = require('mocha').describe;
var pre = require('mocha').before;
var assertions = require('mocha').it;
var assert = require('chai').assert;

testCase('Array', function () {
  pre(function () {
    // ...
  });

  testCase('#indexOf()', function () {
    assertions('should return -1 when not present', function () {
      assert.equal([1, 2, 3].indexOf(4), -1);
    });
  });
});
```

## EXAMPLES

Real live example code:

-   [Mocha examples](https://github.com/mochajs/mocha-examples)
-   [Express](https://github.com/visionmedia/express/tree/master/test)
-   [Connect](https://github.com/senchalabs/connect/tree/master/test)
-   [SuperAgent](https://github.com/visionmedia/superagent/tree/master/test/node)
-   [WebSocket.io](https://github.com/LearnBoost/websocket.io/tree/master/test)
-   [Mocha tests](https://github.com/mochajs/mocha/tree/master/test)