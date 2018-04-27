title: JavaScript - The not so weird parts
output: index.html
theme: theme
controls: false

-- intro

# JavaScript - The not so weird (anymore) parts

## By [@daffl](https://twitter.com/daffl)
## [daffl.github.io/not-so-weird-js](https://daffl.github.io/not-so-weird-js)

---

## The weird and not so weird

- ECMAScript
- JavaScript tooling
- Truthy and falsyness
- Operators
- `var`, `let`, `const`, globals
- Object declarations
- Destructuring
- Functions and `this`
- Inheritance

---

## ECMAScript (ES)

Standard for the JavaScript language

- 1995 - JavaScript (Mocha, LiveScript) prototype
- 1997 - 1st edition
- 1998 - 2nd edition
- 1999 - 3rd edition
- 2009 - 5th edition (ES5)
- 2015 - 6th edition (ES6, ES2015)
- 2016 - ES2016
- 2017 - ES2017

--- standardization

---

## JavaScript tooling

- Code quality tools ([ESLint](https://eslint.org/), [prettier](https://github.com/prettier/prettier))
- Transpilers ([Babel](https://babeljs.io/), [Traceur](https://github.com/google/traceur-compiler), [Rollup](https://github.com/rollup/rollup))
    - Turn not weird JavaScript (ES2015+) into weird (old) JavaScript
    - Allow support for non-standard and experimental features
    - ES modules (`import`, `export`)
    - JSX
- Compile-to JavaScript languages
    - [TypeScript](https://www.typescriptlang.org/)
    - [ClojureScript](https://github.com/clojure/clojurescript)

---

## Weird-ish: Truthy- and falsyness

__Truthyness__

```javascript
const person = { name: 'David' }

// Do something if property is truthy
if(person.name) {}
// Do something if property exists at all
if(typeof person.name !== 'undefined') {}
```

__Falsy values__

```javascript
false
null
undefined
0
''
person.undefinedProperty
```

---

## The weirdest: `==`, `!=`

`==` and `!=` compare the value

```javascript
1 == 1 // -> true
1 == '1' // -> true
1 == 2 // -> false

// Now things get weird
'' == false // -> true
[] == false // -> true
null == undefined // -> true
```

---

## Not weird: `===`, `!==`

`===` and `!==` compare value _and_ type

```javascript
1 === 1 // -> true
1 === '1' // -> false
1 === parseInt('1', 10) // -> true
[] === false // -> false
null === undefined // -> false
```

#### ALWAYS USE `===` and `!==`

---

## Weird: `var`

`var` declares a variable that can be reassigned and redeclared and is scoped to the function.

```javascript
for(var i = 0; i < 10; i++) {}

console.log(i); // 10

if(true) {
    var i = 20;

    var i = 30;
}

console.log(i); // 30
```

---

## Very weird: globals

Variables declared without anything [will automatically become global](https://github.com/DmitryBaranovskiy/raphael/issues/934)

```js
function test() {
    var local = 42;
    global = 'global';
}
```

Attach to the global (`window` in browser) object explicitly:

```javascript
function test() {
    var local = 42;
    window.global = 'global';
}
```

---

## Not so weird: `let`

`let` declares a variable that can be reassigned but not redeclared and is scoped to the block.

```javascript
for(let i = 0; i < 10; i++) {}

console.log(i); // ReferenceError: i is not defined

if(true) {
    let i = 20;
    i = 10;
    console.log(i); // 10

    let i = 2; // SyntaxError: Identifier 'i' has already been declared
}
```

---

## Not so weird: `const`

`const` declares a variable that can be reassigned but not redeclared and is scoped to the block.

```javascript
if(true) {
    const i = 20;
    i = 10; // TypeError: Assignment to constant variable

    const i = 2; // SyntaxError: Identifier 'i' has already been declared
}

console.log(i); // ReferenceError: i is not defined
```

---

## A little weird: Object declarations

```javascript
var key = 'type';
var firstName = 'David';

var user = {
    firstName: firstName,
    lastName: 'Luecke',
    fullName: function() {
        return this.firstName + ' ' + this.lastName;
    }
}

user[key] = 'user';
```

---

## Not so weird: Object shorthands

```javascript
const firstName = 'David';
const user = {
    [key]: 'user',
    firstName,
    lastName: 'Luecke',
    fullName() {
        return `${this.firstName} ${this.lastName}`;
    }
}
```

---

## Awesome: Destructuring

```javascript
const user = {
    name: 'David',
    likes: [ 'ice cream', 'rock climbing' ],
    address: {
        city: 'Vancouver'
    }
}

const { name } = user; // name = 'David'
// Nested
const { address: { city } } = user; // city = 'Vancouver'
// From arrays
const { likes: [, secondLike ] } = user; // secondLike = 'rock climbing'
// Aliased
const { name: username } = user; // username = 'David'
// Defaults
const { address: { country = 'Canada' } } = user; // country = 'Canada'
```

---

## Slightly weird: Functions and `arguments`

Functions are treated just like any other variable:

```js
var myFunction = function(arg) {}
// the same as
function myFunction(arg) {}

const person = {}

person.sayHi = function() {}

// Magic arguments
function sum() {
    var result = 0;
    for(var i = 0; i < arguments.length; i++) {
        result += arguments[i];
    }
    return result;
}
```

---

## Not weird: Fancy function arguments

```js
// Default arguments
function add(a = 1, b = 2) {}

// Destructuring arguments
function parseAddress({ address: { city, country = 'Canada' } }) {}

// Spread operator (varargs)
function sum(...numbers) {
    let result = 0;
    for(let number of numbers) {
        result += number;
    }
    return result;
}
```

---

## Weird: The trinity of `this`

`this` refers to the _owner_ of the function you are calling.

1) __The object__, when the function is called on an object

```js
person.sayHi()
```

2) A new object if the function is called with the `new` operator

```js
new Dog('Goofy');
```

3) The owner has been changed with [call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call), [apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) or [bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

```js
sayHi.call(owner, arg1, arg2);
sayHi.apply(owner, argsArray);

var boundSayHi = sayHi.bind(owner);
```

Otherwise `this` will be the global (`window`) object (or `undefined` in strict mode).

---

## Less weird: Arrow functions

Arrow functions `this` is always the same as the `this` of its surrounding context. It can __not__ be changed. Arrow functions do not have magic `arguments`.

```javascript
// Single argument, single line
const square = x => x * x;

// Multiple arguments, single line
const sum = (a, b) => a + b;

// Multi line
const sum = (...numbers) => {
    let result = 0;
    for(let number of numbers) {
        result += number;
    }
    return result;
}
```

---

## Kind of weird: JavaScript inheritance

JavaScript only knows objects. Inheritance is done by pointing an objects prototype to another object.

```js
const Elephant = function() {
    this.legs = 4;
    this.hasTrunk = true;
}

Elephant.prototype.getType = function() {
    return 'Mammal';
}

const dumbo = new Elephant();
dumbo.legs // 4
dumbo.getType(); // Mammal
```

--- inheritance

---

## Kind of weird: JavaScript inheritance

```js
const Dogephant = function() {
    this.hasFur = true;
}

Dogephant.prototype = new Elephant();

const Wienephant = function() {
    this.isTiny = true;
}

Wienephant.prototype = new Dogephant();
```

---


## Less weird: ECMAScript 5 inheritance

Uses only objects and inherits with `Object.create(prototypeObject)`

```js
const Elephant = {
    legs: 4,
    hasTrunk: true,
    getType() {
        return 'Mammal';
    }
}

const Dogephant = Object.create(Elephant);

Dogephant.hasFur = true;

const Wienephant = Object.create(Dogephant);

Wienephant.isTiny = true;
```

---

## Maybe not weird: ES2015 (ES6) classes

Uses `class`, `extends` and `constructor`

```js
class Elephant {
    constructor() {
        this.legs = 4;
        this.hasTrunk = true;
    }

    getType() {
        return 'Mammal';
    }
}
```

---

## Maybe not weird: ES2015 (ES6) classes

```javascript
class Dogephant extends Elephant {
    constructor() {
        super();
        this.hasFur = true;
    }
}

class Wienephant extends Dogephant {
    constructor() {
        super();
        this.isTiny = true;
    }
}
```
