pato.js
=======

Functional pattern matching for JavaScript

## Example ##
```javascript
var pato = require('pato');
var match = pato.match;
var int = pato.types.int;

// match by value
var r = match(2)
    .equal(1).then(100)
    .equal(2).then(200)
    .equal('foo').then('bar')
.done();
assert(200 == r);

// match by type 
var r = match('world')
    .type('number').then(function(x) { return 2 * x; })
    .type('array').then([1, 2])
    .type('string').then(function(x) { return 'hello ' + x; })
.done();
assert('hello world' == r);

// match by condition
var r = match(31)
    .cond(int(0, 9)).then('less than 10')
    .cond(int(10, 50)).then('between 10 and 50')
.done();
assert('between 10 and 50' == r);

var r = match({ age : 31, male : false, name : 'todd'})
    .cond({ age : int(10, 35), male : true }).then('young man')
    .cond({ age : int(10, 35), male : false }).then('young girl')
    .cond({ age : int(0, 10), male : true }).then('little boy')
    .cond({ age : int(0, 10), male : false}).then('little girl')
    .any().then('N/A')
.done();
assert('young man' == r);

// match by predicator 
var r = match({ age : 65, male : false, name : 'todd'})
    .cond({ age : int(10, 35), male : true }).then('young man')
    .cond({ age : int(10, 35), male : false }).then('young girl')
    .cond({ age : int(0, 10), male : true }).then('little boy')
    .cond({ age : int(0, 10), male : false}).then('little girl')
    .cond(function(cond) { return cond.age > 60 && cond.male; }).then('old man')
    .any().then('N/A')
.done();
assert('old man' == r);

// match by constructor
function Car() {};
var car = new Car();
var r = match(car)
    .instance_of(Car).then('This is a car')
    .type('array').then([1, 2])
    .type('string').then(function(x) { return 'hello ' + x; })
.done();
assert('This is a car' == r);
```
