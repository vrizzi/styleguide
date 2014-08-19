# `magnetis();`

Magnetis JavaScript code styleguide.

## Table of contents

* [Disclaimer](#disclaimer)
* [Semicolons](#semicolons)
* [Strict mode](#strict-mode)
* [Context and closures](#context-and-closures)
* [Variables](#variables)
  * [Single `var` definition](#single-var-definition)
  * [Order](#order)
  * [Style](#style)
* [Objects](#objects)
  * [Object literal syntax](#object-literal-syntax)
  * [Reserved words](#reserved-words)
  * [Object keys](#object-keys)
* [Arrays](#arrays)
  * [Array literal syntax](#array-literal-syntax)
  * [Adding items](#adding-items)
  * [Cleaning up](#cleaning-up)
  * [Array conversion](#array-conversion)
* [Strings](#strings)
  * [Quotes](#quotes)
  * [String concatenation](#string-concatenation)
  * [Long strings](#long-strings)
  * [String conversion](#string-conversion)
* [Functions](#functions)
  * [Function arguments](#function-arguments)
* [Naming conventions](#naming-conventions)
  * [Naming functions](#naming-functions)
  * [Naming constructors](#naming-constructors)
  * [Naming private properties](#naming-private-properties)
  * [Referencing `this`](#referencing-this)
  * [Naming booleans](#naming-booleans)
  * [Naming acessors](#naming-acessors)
  * [Naming event handlers](#naming-event-handlers)
* [Properties](#properties)
* [Conditional Expressions and equality](#conditional-expressions-and-equality)
* [Blocks](#blocks)
* [Comments](#comments)
* [Whitespace](#whitespace)
  * [Tabs](#tabs)
  * [EOF](#eof)
  * [Condition and loop declarations](#condition-and-loop-declarations)
  * [Operators](#operators)
  * [Loop steps](#loop-steps)
  * [Object declarations](#object-declarations)
  * [Chaining](#chaining)
* [Type checking](#type-checking)
  * [String](#string)
  * [Number](#number)
  * [Boolean](#boolean)
  * [Object](#object)
  * [Array](#array)
  * [Node](#node)
  * [`null`](#null)
  * [`null` or `undefined`](#null-or-undefined)
  * [`undefined`](#undefined)
    * [Global variables](#global-variables)
    * [Local variables](#local-variables)
    * [Property](#property)
* [Best practices](#best-practices)
  * [jQuery](#jquery)
* [Links](#links)
  * [Styleguides](#styleguides)
  * [Articles and blog posts](#articles-and-blog-posts)
  * [Tools](#tools)

## Disclaimer

This document is inspired by [jQuery Style Guide](http://contribute.jquery.org/style-guide/js), [idiomatic.js](https://github.com/rwaldron/idiomatic.js) and [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript).

**[⬆ back to top](#table-of-contents)**

## Semicolons

* Yes, please! [ASI](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Automatic_semicolon_insertion) is very complicated, don't rely on it.

```js
// Bad
(function() {
  var name = 'John'
  return name
})()

// Good
(function() {
  var name = 'John';
  return name;
})();
```

**[⬆ back to top](#table-of-contents)**

## Strict mode

* You should use the strict mode pragma. Just be aware not to use it globally!

```js
// Bad
'use strict';

function doSomething() {
}

function doSomethingElse() {
}

// Good
function doSomething() {
  'use strict';
  // stuff
}

// Good
(function() {
  'use strict';

  function doSomething() {
  }

  function doSomethingElse() {
  }
})();
```

**[⬆ back to top](#table-of-contents)**

## Context and closures

* Always wrap your modules in closures creating a unique context of execution. Throwing things at the global scope can lead to all kinds of weird behaviors.

```js
// Bad
function doAwesomeStuff() {
  $('.js-item').fadeIn();
}

function doEvenCoolerStuff() {
  $('.js-item').animate({top: 100}, 'fast');
}

// Good
(function($) {
  function doAwesomeStuff() {
    $('.js-item').fadeIn();
  }

  function doEvenCoolerStuff() {
    $('.js-item').animate({top: 100}, 'fast');
  }
})(jQuery);

// Good
var Module = (function() {
  function Module() {
    // magic happens here
  }

  return Module;
})();
```

**[⬆ back to top](#table-of-contents)**

## Variables

### Single `var` definition

* Use single `var` definition approach when defining variables.

```js
// Bad
var foo = 'Foo';
var bar = 'Bar';
var baz = 'Baz';

// Good
var foo = 'Foo',
  bar = 'Bar',
  baz = 'Baz';
```

### Order

* Variables with no startup value should come first.

```js
// Bad
var name = 'John',
  surname = 'Doe',
  country,
  age = 42,
  email;

// Good
var country,
  email,
  name = 'John',
  surname = 'Doe',
  age = 42;
```

### Style

* Do not follow "comma first" style!

```js
// Bad (OMG, really bad!)
var once
  , upon
  , aTime;

// Good
var once,
  upon,
  aTime;

// Bad
var user = {
  name: 'John'
  , surname: 'Doe'
  , age: 42
};

// Good
var user = {
  name: 'John',
  surname: 'Doe',
  age: 42
};
```

**[⬆ back to top](#table-of-contents)**

## Objects

### Object literal syntax

* Use the literal syntax for object creation.

```js
// Bad
var foo = new Object();

// Good
var bar = {};
```

### Reserved words

* Do not use [reserved words](http://es5.github.io/#x7.6.1) as keys. They can cause problems in older IE versions.

```js
// Bad
var foo = {
  class: 'Foo'
};

// Good
var bar = {
  klass: 'Bar'
};

// Good
var baz = {
  type: 'Baz'
};
```

### Object keys

* Use :camel: camelCase when naming object keys when possible.

```js
// Bad
var user = {
  'name': 'John',
  'has-children': false,
  'is-underage': false,
  'age': 42
};

// Good
var user = {
  name: 'John',
  hasChildren: false,
  isUnderage: false,
  age: 42
};
```

* If you need to use quotes to wrap your keys for any reason, be consistent about it:

```js
// Bad
var config = {
  development: true,
  'markdown-plugin': true,
  shouldIgnoreManifest: false,
  PersistData: true,
  enforcenamespace: true
};

// Good
var config = {
  'development': true,
  'markdown-plugin': true,
  'should-ignore-manifest': false,
  'persist-data': true,
  'enforce-namespace': true
};

// Good
var config = {
  development: true,
  markdownPlugin: true,
  shouldIgnoreManifest: false,
  persistData: true,
  enforceNamespace: true
};
```

**[⬆ back to top](#table-of-contents)**

## Arrays

### Array literal syntax

* Use the literal syntax for array creation.

```js
// Bad
var foo = new Array();

// Good
var bar = [1, 2, 3];
```

### Adding items

* If you don't know the array length use `Array.push`.

```js
var groceries = [];

// Bad
groceries[groceries.length] = 'tomato';

// Good
groceries.push('oreo');
```

### Cleaning up

* To cleanup an array set its length to zero.

```js
// Bad
var foo = [1, 3, 5];
foo = null;

// Good
var bar = [2, 4, 6];
bar.length = 0;
```

### Array conversion

* To convert an array-like object to an array, use `Array.slice`.
* PS: you might want to do this because array-like objects (e.g {'0': 'hello', '1': 'there'} ) don't have methods like '.join()', for more information on this suject read [this article]('http://nfriedly.com/techblog/2009/06/advanced-javascript-objects-arrays-and-array-like-objects/')

```js
function argsToArray() {
  var args = Array.prototype.slice.call(arguments);
}
```

**[⬆ back to top](#table-of-contents)**

## Strings

### Quotes

* Use single quotes `''` for strings.

```js
// Bad
var phil = "Philip Sampaio";

// Good
var luciano = 'Luciano Tavares';
```

### String concatenation

* Concat strings using the plus `+` operator.

```js
var name = 'Rafael';

// Bad
name.concat('Rinaldi');

// Good
name += ' Rinaldi';
```

### Long strings

* Prefer string concatenation for long strings, always adding line breaks after an operator. Long strings can impact performance big time ([benchmark](http://jsperf.com/ya-string-concat) and [discussion](https://github.com/airbnb/javascript/issues/40)).

```js
// Bad
var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// Bad
var errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// Good
var errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';
```

### String conversion

* To convert objects to string, use the `toString()` method.

* The conversion with `toString()` does not work with `null` and `undefined` values.
To convert any value to string, concats its value to an empty string. This is useful when you don't care about the value of the conversion (ex: in a logging or tracking system).
In case you convert a `null` value, it will transform into `"null"`. The same rule is applied to `undefined` values.

```js
var value = 42;

// Good
value += '';

var empty;

empty += ''; // "undefined"
```

**[⬆ back to top](#table-of-contents)**

## Functions

### Function arguments

* Whenever you have more than 3 arguments being passed to a function it's preferable to use an object instead.

```js
// Bad

// Too many arguments
function setUser(firstName, lastName, age, gender, occupation) {
  // do stuff
}

setUser('Jon', 'Snow', 25, 'Male', 'Nights-watcher');

// Good

// Use a config/options object instead
function setUser(user) {
  // do stuff
}

setUser({
  firstName: 'Jon',
  lastName: 'Snow',
  age: 25,
  gender: 'Male',
  occupation: 'Nights-watcher'
});
```

## Naming conventions

* Use camelCase when naming objects, functions and instances.

```js
// bad
var foo_bar = 'I am messed up';

// good
var fooBar = 'I am alright!';
```

* Use PascalCase when naming constructors and "classes".

```js
// Bad
function crewMember(name, role) {
  this.name = name;
  this.role = role;
}

var thales = new crewMember('Thales', 'Desenvolvedor');

// Good
function CrewMember(name, role) {
  this.name = name;
  this.role = role;
}

var vinicius = new CrewMember('Vinicius', 'Designer');
```

### Naming functions

* Avoid single letter names. Be descriptive and clear.

```js
// Bad
function f() {
  // What the hell is this?
}

// Good
function bootstrapFoo() {
  // Ah, ok!
}

// Bad
function stuff() {
  // "stuff" is too generic
}

// Good
function doAnimationStuff() {
  // Now we're talking
}
```

### Naming constructors

* Always wrap constructor invocations with brackets. It's going to be easier to pass new constructor values if needed in the future.

```js
// Bad
var foo = new FooBar;

// Good
var bar = new FooBar();
var baz = new FooBar(1, 'lorem');
```

### Naming private properties

* Always use a leading underscore `_` when naming private properties and methods.

```js
// Bad
var $name = 'Foo';
var __name = 'Bar';

// Good
var _name = 'Baz';
```

### Referencing `this`

* When making a reference to `this` name it as `self`.

```js
// Bad
function() {
  var that = this;

  return function() {
    console.log(that);
  }
}

// Bad
function() {
  var _this = this;

  return function() {
    console.log(this);
  }
}

// Good
function() {
  var self = this;

  return function() {
    console.log(self);
  }
}
```

### Naming booleans

* When naming a Boolean variable, it should start with "is", "has", "should" or "was".

```js
// Bad
var ready = true,
  animate = true,
  started = false,
  animation = true;

// Good
var isReady = true,
  shouldAnimate = true,
  wasStarted = true,
  hasAnimation = true;
```

* When naming a Boolean function, it should start with "is".

```js
// Bad
function ready() {
  return false;
}

// Good
function isReady() {
  return true;
}
```

### Naming acessors

* When naming an acessor, try to start with "get" or "set". Also explicitly name `value` as getters parameter.

```js
var currentStatus;

// Bad
function status(val) {
  if (val) {
    currentStatus = val;
  }

  return currentStatus;
}

// Good
function setStatus(value) {
  currentStatus = value;
}

function getStatus() {
  return currentStatus;
}
```

### Naming event handlers

* When naming an event handler, combine its action with the event type.

```js
// Bad
$('.button').click(click);

function click() {
  console.log('meh');
}

// Good
$('.button').click(toggleColorOnClick);

function toggleColorOnClick() {
  console.log('should toggle color on click!');
}
```

### Naming element selectors

* When naming class selectors of DOM elements that will have any kind of behavior, add `js` prefix to the name of your component separating words using an hyphen `-`.

```html
<ul class="js-navigation-menu">
  <li>Foo</li>
  <li>Bar</li>
</ul>
```

```js
var menu = $('.js-navigation-menu');
```

**[⬆ back to top](#table-of-contents)**

## Properties

* Use dot notation to access properties.

```js
var user = {
  name: 'John',
  age: 42
};

// Bad
var userName = user['name'];

// Good
var userAge = user.age;
```

* Use subscript notation `[]` to access dynamic properties.

```js
var user = {
  name: 'John',
  age: 42
};

function getUserInfo(info) {
  return user[info];
}

var userName = getUserInfo('name');
```

**[⬆ back to top](#table-of-contents)**

## Conditional Expressions and equality

* Use `===` and `!==` over `==` and `!=`.

**[⬆ back to top](#table-of-contents)**

## Blocks

* Always wrap blocks within braces and embrace new lines.

```js
// Bad
if (false)
  return;

// Bad
if (false) { return false; }

// Good
if (true) {
  return true;
}

// Good
if (true) {
  return true;
} else {
  return false;
}
```

**[⬆ back to top](#table-of-contents)**

## Comments

* Don't make trivial and obvious comments, be concise about it. If you caught yourself writing too much documentation for your code, consider a pair programming because you're likely over engineering it.

* Feel free to use markdown syntax on code comments.

* No need to use JSDoc syntax since we don't automatically generate documentation.

* Using `FIXME`, `TODO` and `NOTE` tags can help other developers understand and maintain your code. You can use it as you want.

* Use documentation block syntax followed by a newline for multiline comments. There are [tools](#tools) available to make it really easy.

```js
// Bad

// Used to match `RegExp` special characters.
// See this [article on `RegExp` characters](http://www.regular-expressions.info/characters.html#special)
// for more details.

var matchSpecialChars = /[.*+?^${}()|[\]\/\\]/g;

// Bad

/*
Used to match `RegExp` special characters.
See this [article on `RegExp` characters](http://www.regular-expressions.info/characters.html#special)
for more details.
*/

var matchSpecialChars = /[.*+?^${}()|[\]\/\\]/g;

// Good

/**
* Used to match `RegExp` special characters.
* See this [article on `RegExp` characters](http://www.regular-expressions.info/characters.html#special)
* for more details.
*/

var matchSpecialChars = /[.*+?^${}()|[\]\/\\]/g;

```

* Use `//` for single line comments. Place single line comments on a newline above the subject of the comment and add an empty line before the comment.

```js
// Bad
var active = true;  // is current tab

// Good
// is current tab
var active = true;

// Bad
function getType() {
  console.log('fetching type...');
  // set the default type to 'no type'
  var type = this._type || 'no type';

  return type;
}

// Good
function getType() {
  console.log('fetching type...');

  // set the default type to 'no type'
  var type = this._type || 'no type';

  return type;
}
```

**[⬆ back to top](#table-of-contents)**

## Whitespace

### Tabs

* Use soft tabs set to 2 spaces and never mix spaces with tabs.

```js
// Bad
function() {
∙∙∙∙var name;
}

// Bad
function() {
∙var name;
}

// Bad
function() {
⇥var name;
}

// Good
function() {
∙∙var name;
}
```

### EOF

* Always add an empty line at the end of your file.

```js
// Bad
(function() {
  document.write('Hello World!');
})();

// Good
(function() {
  document.write('Hello World!');
})();
↵
```

### Condition and loop declarations

* Place 1 space before and after a condition or loop declarations.

```js
// Bad
if(false){return false;}

// Good
if (true) {
  return true;
}

// Bad
while(false){console.log('whatever');}

// Good
while (false) {
  console.log('alright!');
}
```

### Operators

* Set off operators with spaces.

```js
// Bad
var x=y+5;

// Good
var x = y + 5;
```

### Loop steps

* Place 1 space after loop steps and function arguments.

```js
// Bad
for(var i=0;i<10;++i) {
  console.log(i);
}

// Good
for (var i = 0; i < 42; ++i) {
  console.log(i);
}

// Bad
function setUser(name,surname,age){
  // sets user
}

// Good
function setUser(name, surname, age) {
  // sets user
}
```

### Object declarations

* In objects that have more than a single property you should break all properties into new lines.

```js
// Bad
var user = {name: 'John', surname: 'Doe', age: 42};

// Good
var user = {
  name: 'John',
  surname: 'Doe',
  age: 42
};
```

* Add inner spaces when declaring inline objects.

```js
// Bad
var app = {locale: 'pt-br'};

// Good
var app = { locale: 'pt-br' };
```

### Chaining

* Use indentation when making long method chains.

```js
// Bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// Good
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount();
```

**[⬆ back to top](#table-of-contents)**

## Type checking

### String

```js
typeof variable === 'string';
```

### Number

```js
typeof variable === 'number';
```

### Boolean

```js
typeof variable === 'boolean';
```

### Object

```js
typeof variable === 'object';
```

### Array

```js
Array.isArray(arrayLikeObject);
```

### Node

```js
elem.nodeType === 1;
```

### `null`

```js
variable === null;
```

### `null` or `undefined`

```js
variable == null;
```

### `undefined`

#### Global variables

```js
typeof variable === 'undefined';
```

#### Local variables

```js
variable === undefined;
```

### Property

```js
object.prop === undefined;
object.hasOwnProperty(prop);
'prop' in object;
```

**[⬆ back to top](#table-of-contents)**

## Best practices

* Do not start closures with semi-colons.

* We encourage you to log important app events and changes in development mode only. Follow the format of `Module/ClassName :: method() :: message`.

```js
function App() {
  console.log('App :: App instance created');
}

App.prototype.bootstrap = function() {
  console.log('App :: bootstrap() :: App was initialized');
};
```

### jQuery

* Cache jQuery lookups always when possible.
* Prefer `remove()` over `empty()`.
* Always favor functional and utility functions of jQuery over Underscore.

**[⬆ back to top](#table-of-contents)**

## Links

### Articles and blog posts

* [It’s time to start using JavaScript strict mode](http://www.nczonline.net/blog/2012/03/13/its-time-to-start-using-javascript-strict-mode)
* [Why is it recommended to have empty line in the end of file?](http://stackoverflow.com/questions/2287967/why-is-it-recommended-to-have-empty-line-in-the-end-of-file)
* [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript)
* [Naming methods, properties and objects](http://blog.millermedeiros.com/naming-methods-properties-and-objects)

### Tools

* [DocBlockr](https://sublime.wbond.net/packages/DocBlockr)
* [tcomment_vim](https://github.com/tomtom/tcomment_vim)
* [JSHint](http://www.jshint.com)
* [JSHint Plugins](http://www.jshint.com/install)
* [SublimeLinter](http://www.sublimelinter.com)
* [SublimeLinter-jshint](https://github.com/SublimeLinter/SublimeLinter-jshint)
* [How To Use SublimeLinter 3 (video)](http://youtu.be/u6fvJRao-E4)

**[⬅ back to main](../../)**&nbsp;&nbsp;&nbsp;&nbsp;**[⬆ back to top](#table-of-contents)**
