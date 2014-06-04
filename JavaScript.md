# JavaScript code styleguide

## Table of contents

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

## Semicolons

* Yes, please!

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
	name: 'John',
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

* Use camelCase when naming object keys

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

* To convert any value to string, concats its value to an empty string.

```js
// Bad
var value = 42;

_.toString(value);

// Bad
value.toString();

// Good
value += '';
```

**[⬆ back to top](#table-of-contents)**

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

* When saving a reference to `this` use `self`.

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
  if(val) {
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
if(false)
  return;
	
// Bad	
if(false) { return false; }

// Good
if(true) {
  return true;
}

// Good
if(true) {
  return true;
} else {
  return false;
}
```

**[⬆ back to top](#table-of-contents)**

## Comments

* Use `/** ... **/` for multiline comments.

```js
// Bad

// this method parses a string
// based on the given name
function parse(name) {
  return '';
}

// Good

/**
 * This method parses a string
 * based on the given name
 */

function parseString(name) {
  return '';
}
```

* Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

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

* Using `FIXME`, `TODO` and `NOTE` can help other developers understand and maintain your code. It's encouraged that you make use of them.

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

* In objects that have more than a single property you should break all properties in new lines.

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

* We encourage you to log important app states always when possible. For that you should follow the format of module/class, then method, then the message or value that you want to log.

```js
function FooBar() {
}

// Bad
FooBar.prototype.doStuff(value) {
  console.log('do stuff' + value);
}

// Good
FooBar.prototype.doStuff(value) {
  console.log('FooBar :: doStuff() ::', value);
}
```

### jQuery

	* Cache jQuery lookups always when possible.
	* Prefer `remove()` over `empty()`.
	* Always favor functional and utility functions of jQuery over Underscore.

**[⬆ back to top](#table-of-contents)**

## Links

### Styleguides

* [jQuery](http://contribute.jquery.org/style-guide/js)
* [idiomatic.js](https://github.com/rwaldron/idiomatic.js)
* [Airbnb](https://github.com/airbnb/javascript)

### Articles and blog posts

* [It’s time to start using JavaScript strict mode](http://www.nczonline.net/blog/2012/03/13/its-time-to-start-using-javascript-strict-mode)

### Tools

* [JSHint](http://www.jshint.com)
* [JSHint Plugins](http://www.jshint.com/install)
* [esformatter](https://github.com/millermedeiros/esformatter)
* [esformatter Plugins](https://github.com/piuccio/sublime-esformatter)

**[⬆ back to top](#table-of-contents)**