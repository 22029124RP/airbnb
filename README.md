# ~~Airbnb~~ ascribe JavaScript Style Guide() {

## Introduction
At ascribe we write a lot of JavaScript and value quality code.
Since all of us liked [Airbnb's JavaScript Style Guide](https://github.com/airbnb/javascript), we figured that we can just fork it and change it to our needs.

## Approach
As we want everyone in the team to be able to take part in the creation of this style guide, we decided to have it reviewed and transformed to our specific needs.
This is why we're going to remove all the contents of this document, to add them bit-by-bit again through a process of pull-requests and reviews.
Depending on how fast the review will be, every other day there will be a new pull request for everyone to read and comment on.

As for adjusting the linters, we still need to figure out a way on how to change them efficiently. So if you have suggestions, come forward.


Other Style Guides
 - [ES5](es5/)
 - [React](react/)

## Table of Contents

  1. [Types](#types)
  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Arrow Functions](#arrow-functions)
  1. [Constructors](#constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators-and-generators)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting) #TBD
  1. [Comparison Operators & Equality](#comparison-operators--equality) #TBD
  1. [Blocks](#blocks) #TBD
  1. [Comments](#comments) #TBD
  1. [Whitespace](#whitespace) #TBD
  1. [Commas](#commas) #TBD
  1. [Semicolons](#semicolons) #TBD
  1. [Type Casting & Coercion](#type-casting--coercion) #TBD
  1. [Naming Conventions](#naming-conventions) #TBD
  1. [Accessors](#accessors) #TBD
  1. [Events](#events) #TBD
  1. [jQuery](#jquery) #TBD
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility) #TBD #TBD
  1. [ECMAScript 6 Styles](#ecmascript-6-styles) #TBD
  1. [Testing](#testing) #TBD
  1. [Performance](#performance) #TBD
  1. [Resources](#resources) #TBD
  1. [In the Wild](#in-the-wild) #TBD
  1. [Translation](#translation) #TBD
  1. [The JavaScript Style Guide Guide](#the-javascript-style-guide-guide) #TBD
  1. [Chat With Us About JavaScript](#chat-with-us-about-javascript) #TBD
  1. [Contributors](#contributors) #TBD
  1. [License](#license) #TBD

## Types

  - [1.1](#1.1) <a name='1.1'></a> **Primitives**: When you access a primitive type you work directly on its value.

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - [1.2](#1.2) <a name='1.2'></a> **Complex**: When you access a complex type you work on a reference to its value.

    + `object`
    + `array`
    + `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#table-of-contents)**

## References

  - [2.1](#2.1) <a name='2.1'></a> Use `const` for all of your references; avoid using `var`.

  > Why? This ensures that you can't reassign your references (mutation), which can lead to bugs and difficult to comprehend code.

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  - [2.2](#2.2) <a name='2.2'></a> If you must mutate references, use `let` instead of `var`.

  > Why? `let` is block-scoped rather than function-scoped like `var`.

    ```javascript
    // bad
    var count = 1;
    if (true) {
        count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
        count += 1;
    }
    ```

  - [2.3](#2.3) <a name='2.3'></a> Note that both `let` and `const` are block-scoped.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    {
        let a = 1;
        const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ back to top](#table-of-contents)**

## Objects

  - [3.1](#3.1) <a name='3.1'></a> Use the literal syntax for object creation.

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  - [3.2](#3.2) <a name='3.2'></a> If your code will be executed in browsers in script context, don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8. [More info](https://github.com/airbnb/javascript/issues/61). It’s OK to use them in ES6 modules and server-side code.

    ```javascript
    // bad
    const superman = {
        default: { clark: 'kent' },
        private: true,
    };

    // good
    const superman = {
        defaults: { clark: 'kent' },
        hidden: true,
    };
    ```

  - [3.3](#3.3) <a name='3.3'></a> Use readable synonyms in place of reserved words.

    ```javascript
    // bad
    const superman = {
        class: 'alien',
    };

    // bad
    const superman = {
        klass: 'alien',
    };

    // good
    const superman = {
        type: 'alien',
    };
    ```

  <a name="es6-computed-properties"></a>
  - [3.4](#3.4) <a name='3.4'></a> Use computed property names when creating objects with dynamic property names.

  > Why? They allow you to define all the properties of an object in one place.

    ```javascript

    function getKey(k) {
        return `a key named ${k}`;
    }

    // bad
    const obj = {
        id: 5,
        name: 'Berlin',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
        id: 5,
        name: 'Berlin',
        [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a>
  - [3.5](#3.5) <a name='3.5'></a> Use object method shorthand.

    ```javascript
    // bad
    const atom = {
        value: 1,

        addValue: function (value) {
            return atom.value + value;
        },
    };

    // good
    const atom = {
        value: 1,

        addValue(value) {
            return atom.value + value;
        },
    };
    ```

  <a name="es6-object-concise"></a>
  - [3.6](#3.6) <a name='3.6'></a> Use property value shorthand.

  > Why? It is shorter to write and descriptive.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
        lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
        lukeSkywalker,
    };
    ```

  - [3.7](#3.7) <a name='3.7'></a> Group your shorthand properties at the beginning of your object declaration.

  > Why? It's easier to tell which properties are using the shorthand.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
        episodeOne: 1,
        twoJediWalkIntoACantina: 2,
        lukeSkywalker,
        episodeThree: 3,
        mayTheFourth: 4,
        anakinSkywalker,
    };

    // good
    const obj = {
        lukeSkywalker,
        anakinSkywalker,
        episodeOne: 1,
        twoJediWalkIntoACantina: 2,
        episodeThree: 3,
        mayTheFourth: 4,
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  - [4.1](#4.1) <a name='4.1'></a> Use the literal syntax for array creation.

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  - [4.2](#4.2) <a name='4.2'></a> Use Array#push instead of direct assignment to add items to an array.

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a>
  - [4.3](#4.3) <a name='4.3'></a> Use array spreads `...` to copy arrays.

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
        itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```
  - [4.4](#4.4) <a name='4.4'></a> To convert an array-like object to an array, use Array#from.

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

**[⬆ back to top](#table-of-contents)**

## Destructuring

  - [5.1](#5.1) <a name='5.1'></a> Use object destructuring when accessing and using multiple properties of an object.

  > Why? Destructuring saves you from creating temporary references for those properties.

    ```javascript
    // bad
    function getFullName(user) {
        const firstName = user.firstName;
        const lastName = user.lastName;

        return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(obj) {
        const { firstName, lastName } = obj;
        return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
        return `${firstName} ${lastName}`;
    }
    ```

  - [5.2](#5.2) <a name='5.2'></a> Use array destructuring.

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  - [5.3](#5.3) <a name='5.3'></a> Use object destructuring for multiple return values, not array destructuring.

  > Why? You can add new properties over time or change the order of things without breaking call sites.

    ```javascript
    // bad
    function processInput(input) {
        // then a miracle occurs
        return [left, right, top, bottom];
    }

    // the caller needs to think about the order of return data
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
        // then a miracle occurs
        return { left, right, top, bottom };
    }

    // the caller selects only the data they need
    const { left, right } = processInput(input);
    ```


**[⬆ back to top](#table-of-contents)**

## Strings

  - [6.1](#6.1) <a name='6.1'></a> Use single quotes `''` for strings.

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // good
    const name = 'Capt. Janeway';
    ```

  - [6.2](#6.2) <a name='6.2'></a> When using (single- or double) quotes in a string, use the other literal (`''` or `""`).

    ```javascript
    // bad
    const name = "What a \"nice\" day!";

    // bad
    const name = 'Let\'s go to Rosi\'s!';

    // good
    const name = 'What a "nice" day!';

    // good
    const name = "Let's go to Rosi's!";
    ```

  - [6.3](#6.3) <a name='6.3'></a> Strings longer than 100 characters should be written across multiple lines using string concatenation.

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because ' +
        'of Batman. When you stop to think about how Batman had anything to do ' +
        'with this, you would get nowhere fast.';
    ```

  - [6.4](#6.4) <a name='6.4'></a> Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

  <a name="es6-template-literals"></a>
  - [6.5](#6.5) <a name='6.5'></a> When programmatically building up strings, use template strings instead of concatenation.

  > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

    ```javascript
    // bad
    function sayHi(name) {
        return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
        return ['How are you, ', name, '?'].join();
    }

    // good
    function sayHi(name) {
        return `How are you, ${name}?`;
    }
    ```

  - [6.6](#6.6) <a name='6.6'></a> **NEVER** use eval() on a string, it opens too many vulnerabilities.

**[⬆ back to top](#table-of-contents)**

## Functions

  - [7.1](#7.1) <a name='7.1'></a> Use function declarations instead of function expressions.

  > Why? Function declarations are named, so they're easier to identify in call stacks. Also, the whole body of a function declaration is hoisted, whereas only the reference of a function expression is hoisted. This rule makes it possible to always use [Arrow Functions](#arrow-functions) in place of function expressions.

    ```javascript
    // bad
    const foo = function () {
    };

    // good
    function foo() {
    }
    ```

  - [7.2](#7.2) <a name='7.2'></a> Immediately-invoked function expressions should use arrow functions as opposed to traditional functions:

    ```javascript
    // immediately-invoked function expression (IIFE)
    (() => {
        console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - [7.3](#7.3) <a name='7.3'></a> **NEVER** declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.
  - [7.4](#7.4) <a name='7.4'></a> **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    if (currentUser) {
        function test() {
            console.log('Nope.');
        }
    }

    // good
    let test;
    if (currentUser) {
        test = () => {
            console.log('Yup.');
        };
    }
    ```

  - [7.5](#7.5) <a name='7.5'></a> **NEVER** name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    function nope(name, options, arguments) {
        // ...stuff...
    }

    // good
    function yup(name, options, args) {
        // ...stuff...
    }
    ```

  <a name="es6-rest"></a>
  - [7.6](#7.6) <a name='7.6'></a> **NEVER** use `arguments`, opt to use rest syntax `...` instead.

  > Why? `...` is explicit about which arguments you want pulled. Plus rest arguments are a real Array and not Array-like like `arguments`.

    ```javascript
    // bad
    function concatenateAll() {
        const args = Array.prototype.slice.call(arguments);
        return args.join('');
    }

    // good
    function concatenateAll(...args) {
        return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a>
  - [7.7](#7.7) <a name='7.7'></a> Use default parameter syntax rather than mutating function arguments.

    ```javascript
    // really bad
    function handleThings(opts) {
        // No! We shouldn't mutate function arguments.
        // Double bad: if opts is falsy it'll be set to an object which may
        // be what you want but it can introduce subtle bugs.
        opts = opts || {};
        // ...
    }

    // still bad
    function handleThings(opts) {
        if (opts === void 0) {
            opts = {};
        }
        // ...
    }

    // good
    function handleThings(opts = {}) {
        // ...
    }
    ```

  - [7.8](#7.8) <a name='7.8'></a> Avoid side effects with default parameters.

  > Why? They are confusing to reason about.

  ```javascript
  var b = 1;
  // bad
  function count(a = b++) {
    console.log(a);
  }
  count();  // 1
  count();  // 2
  count(3); // 3
  count();  // 3
  ```

  - [7.9](#7.9) <a name='7.9'></a> Always put default parameters last.

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
        // ...
    }

    // good
    function handleThings(name, opts = {}) {
        // ...
    }
    ```

- [7.10](#7.10) <a name='7.10'></a> **NEVER** use the Function constructor to create a new function.

  > Why? Creating a function in this way evaluates a string similarly to eval(), which opens vulnerabilities.

  ```javascript
  // bad
  var add = new Function('a', 'b', 'return a + b');

  // still bad
  var subtract = Function('a', 'b', 'return a - b');
  ```

**[⬆ back to top](#table-of-contents)**

## Arrow Functions

  - [8.1](#8.1) <a name='8.1'></a> When you must use function expressions (as when passing an anonymous function), use arrow function notation.

  > Why? It creates a version of the function that executes in the context of `this`, which is usually what you want, and is a more concise syntax.

  > Why not? If you have a fairly complicated function, you might move that logic out into its own function declaration.

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
        const y = x + 1;
        return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
        const y = x + 1;
        return x * y;
    });
    ```

  - [8.2](#8.2) <a name='8.2'></a> If the function body consists of a single expression, feel free to omit the braces and use the implicit return. Otherwise use a `return` statement.

  > Why? Syntactic sugar. It reads well when multiple functions are chained together.

  > Why not? If you plan on returning an object.

    ```javascript
    // good
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // bad
    [1, 2, 3].map(number => {
        const nextNumber = number + 1;
        `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map(number => {
        const nextNumber = number + 1;
        return `A string containing the ${nextNumber}.`;
    });
    ```

  - [8.3](#8.3) <a name='8.3'></a> In case the expression spans over multiple lines, wrap it in parentheses for better readability.

  > Why? It shows clearly where the function starts and ends.

    ```javascript
    // bad
    [1, 2, 3].map(number => 'As time went by, the string containing the ' +
        `${number} became much longer. So we needed to break it over multiple ` +
        'lines.'
    );

    // good
    [1, 2, 3].map(number => (
        `As time went by, the string containing the ${number} became much ` +
        'longer. So we needed to break it over multiple lines.'
    ));
    ```


  - [8.4](#8.4) <a name='8.4'></a> If your function only takes a single argument, feel free to omit the parentheses.

  > Why? Less visual clutter.

    ```javascript
    // good
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].reduce((y, x) => x + y);
    ```

**[⬆ back to top](#table-of-contents)**

## Constructors

  - [9.1](#9.1) <a name='9.1'></a> Always use `class`. Avoid manipulating `prototype` directly.

  > Why? `class` syntax is more concise and easier to reason about.

    ```javascript
    // bad
    function Queue(contents = []) {
        this._queue = [...contents];
    }
    Queue.prototype.pop = function() {
        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;
    }


    // good
    class Queue {
        constructor(contents = []) {
            this._queue = [...contents];
        }
        pop() {
            const value = this._queue[0];
            this._queue.splice(0, 1);
            return value;
        }
    }
    ```

  - [9.2](#9.2) <a name='9.2'></a> Use `extends` for inheritance.

  > Why? It is a built-in way to inherit prototype functionality without breaking `instanceof`.

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
        Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function() {
        return this._queue[0];
    }

    // good
    class PeekableQueue extends Queue {
        peek() {
            return this._queue[0];
        }
    }
    ```

  - [9.3](#9.3) <a name='9.3'></a> Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = function() {
        this.jumping = true;
        return true;
    };

    Jedi.prototype.setHeight = function(height) {
        this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    class Jedi {
        jump() {
            this.jumping = true;
            return this;
        }

        setHeight(height) {
            this.height = height;
            return this;
        }
    }

    const luke = new Jedi();

    luke.jump()
        .setHeight(20);
    ```


  - [9.4](#9.4) <a name='9.4'></a> It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    class Jedi {
        constructor({ name = 'no name' } = {}) {
            this.name = name;
        }

        getName() {
            return this.name;
        }

        toString() {
            return `Jedi - ${this.getName()}`;
        }
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Modules

  - [10.1](#10.1) <a name='10.1'></a> Always use modules (`import`/`export`) over a non-standard module system. You can always transpile to your preferred module system.

  > Why? Modules are the future, let's start using the future now.

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - [10.2](#10.2) <a name='10.2'></a> Do not use wildcard imports.

  > Why? This makes sure you have a single default export.

    ```javascript
    // bad
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // good
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  - [10.3](#10.3) <a name='10.3'></a>And do not export directly from an import.

  > Why? Although the one-liner is concise, having one clear way to import and one clear way to export makes things consistent.

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './airbnbStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

**[⬆ back to top](#table-of-contents)**

## Iterators and Generators

  - [11.1](#11.1) <a name='11.1'></a> Prefer [JavaScript's higher-order functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) like `map()` and `reduce()` instead of loops like `for-of` unless there is a substantial performance disadvantage by doing so.

  > Why? This enforces our immutable rule. Dealing with pure functions that return values is easier to reason about than side-effects.

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {
        sum += num;
    }

    sum === 15;

    // good
    let sum = 0;
    numbers.forEach((num) => sum += num);
    sum === 15;

    // best (use the functional force, Luke)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;
    ```

  - [11.2](#11.2) <a name='11.2'></a> Only use `for-in` if you know exactly what you're doing. If unsure, prefer the options given in [11.1](#11.1).

  - [11.3](#11.3) <a name='11.3'></a> Don't use generators for now.

  > Why? They don't transpile well to ES5.

**[⬆ back to top](#table-of-contents)**


## Properties

  - [12.1](#12.1) <a name='12.1'></a> Use dot notation when accessing properties.

    ```javascript
    const luke = {
        jedi: true,
        age: 28,
    };

    // bad
    const isJedi = luke['jedi'];

    // good
    const isJedi = luke.jedi;
    ```

  - [12.2](#12.2) <a name='12.2'></a> Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    const luke = {
        jedi: true,
        age: 28,
    };

    function getProp(prop) {
        return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

**[⬆ back to top](#table-of-contents)**


## Variables

  - [13.1](#13.1) <a name='13.1'></a> Always use `const` or `let` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  - [13.2](#13.2) <a name='13.2'></a> Use one `const` or `let` declaration per variable.

    > Why? It's easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs.

    ```javascript
    // bad
    const items = getItems(),
          goSportsTeam = true,
          dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
          goSportsTeam = true;
          dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  - [13.3](#13.3) <a name='13.3'></a> Group all your `const`s and then group all your `let`s.

  > Why? This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  - [13.4](#13.4) <a name='13.4'></a> Assign variables where you need them, but place them in a reasonable place.

  > Why? `let` and `const` are block scoped and not function scoped.

    ```javascript
    // good
    function() {
        test();
        console.log('doing stuff..');

        //..other stuff..

        const name = getName();

        if (name === 'test') {
            return false;
        }

        return name;
    }

    // bad - unnecessary function call
    function(hasName) {
        const name = getName();

        if (!hasName) {
            return false;
        }

        this.setFirstName(name);

        return true;
    }

    // good
    function(hasName) {
        if (!hasName) {
            return false;
        }

        const name = getName();
        this.setFirstName(name);

        return true;
    }
    ```

  > Note that referencing a variable declared by `let` or `const` before they are set results in a reference error, including typeof (see [Why `typeof` is no longer "safe"](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15))

    ```javascript
    if (condition) {
        console.log(typeof value);     // ReferenceError!
        let value = "blue";
    }
    ```

**[⬆ back to top](#table-of-contents)**

# }
