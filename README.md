![npm](https://badge.fury.io/js/mini-mock.svg)

# mini-mock
An ultra-lightweight mocking framework for Node

## Why?
I got tired of the complexity of using mocking frameworks for JS. I wanted something really simple, that does
just what I want it to do.

## General
- For Javascript version: ECMAScript 6
- Written with the 'single object under test' approach in mind
- Object under test should ideally use constructor injection (all dependencies injected via the constructor)
- Records call count on every stubbed function
- Stubs asynchronous and synchronous functions
- After mock instances are returned via the `create()` function, Mocker resets its internal references, so it can immediately be reused

## Usage examples

### Asynchronous mocks (ie: functions using callbacks)

#### Without callback args

```javascript

// set up mock
var Mocker = require('mini-mock');
var mocker = new Mocker();

var foo = mocker.mock(Foo.prototype)
    // async function stubs
    .withAsyncStub('forward', null) // 1st arg: function name; 2nd arg: callback arguments (null in this case)
    .withAsyncStub('reverse', null)
    .create();

// object under test
var objTotest = new Bar(foo);
objToTest.go();

// assertions
expect(foo.recorder['forward'].calls).to.equal(1);
expect(foo.recorder['reverse'].calls).to.equal(1);

```

#### With callback args

```javascript

// set up mock
var Mocker = require('mini-mock');
var mocker = new Mocker();

var foo = mocker.mock(Foo.prototype)
    // async function stubs
    .withAsyncStub('forward', [null, {key1: value1}]) // 1st arg: function name; 2nd arg: callback arguments (null error; object result)
    .withAsyncStub('reverse', [null, {key2: value2}])
    .create();

// object under test
var objTotest = new Bar(foo);
objToTest.go();

// assertions
expect(foo.recorder['forward'].calls).to.equal(1);
expect(foo.recorder['reverse'].calls).to.equal(1);

```

### Synchronous mocks (ie: immediate return)

```javascript

// set up mock
var Mocker = require('mini-mock');
var mocker = new Mocker();

var bar = mocker.mock(Bar.prototype)
    // sync function stubs
    .withSyncStub('go', foo) // 1st arg: function name; 2nd arg: expected result
    .create();

// object under test
var objTotest = new Robot(bar);
objToTest.walk();

// assertions
expect(bar.recorder['go'].calls).to.equal(1);

```

See also the tests in `/test`.