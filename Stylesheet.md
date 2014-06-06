# `.magnetis {}`

Magnetis Stylesheet code styleguide.

## Table of contents

* [Disclaimer](#disclaimer)
* [Introduction](#Introduction)
* [Colors](#colors)
* [Units](#units)
  * [Zero values](#zero-values)
  * [Float values](#float-values)
  * [`letter-spacing`](#letter-spacing)
  * [`line-height`](#line-height)
* [Inline images](#inline-images)
* [`!important`](#important)
* [Pseudo elements](#pseudo-elements)
* [Comments](#comments)
* [Quotes](#quotes)
  * [Attribute values](#attribute-values)
  * [Background URL](#background-url)
  * [`@import`](#import)
* [Organization](#organization)
* [Specificity and nesting](#specificity-and-nesting)
* [Sass syntax](#sass-syntax)
* [Variable definition](#variable-definition)
  * [Local variables](#local-variables)
  * [Immutables (aka constants)](#immutables-aka-constants)
* [Naming conventions](#naming-conventions)
  * [Naming classes](#naming-classes)
  * [Naming modifiers](#naming-modifiers)
  * [Naming states](#naming-states)
* [Whitespace](#whitespace)
  * [Tabs](#tabs)
  * [EOF](#eof)
  * [Inline blocks](#inline-blocks)
  * [Property names and values](#property-names-and-values)
  * [Declaration blocks](#declaration-blocks)
  * [Multiple selectors](#multiple-selectors)
  * [Selector structure](#selector-structure)
  * [Multiple rules](#multiple-rules)
  * [Multiple values](#multiple-values)
* [Links](#links)
  * [Articles and blog posts](#articles-and-blog-posts)
  * [Tools](#tools)

## Disclaimer

This document is heavily inspired by [Mark Otto's Code Guide](http://mdo.github.io/code-guide/#css) and [Idiomatic CSS](https://github.com/necolas/idiomatic-css).

## Introduction

* To write our stylesheets we use methodologies and tools that you need to spend some time and get familiar with:
	1. We follow some rules specified by [SMACSS](http://smacss.com) that enforces a more modular approach of writing components.
	2. We use [Sass](http://sass-lang.com) as a pre-processor of our CSS code.
	3. We use [Compass](http://compass-style.org) to help with things like vendor prefixes and spritesheet generation.

**[⬆ back to top](#table-of-contents)**

## Colors

* Write color hexadecimal values always in lower case. Use shorthand notation when possible.

```scss
// Bad
$disabled-color: #CCCCCC;

// Good
$disabled-color: #ccc;

// Bad
$alert-color: red;

// Good
$alert-color: #ff0066;

// Bad
$wood-color: #EAEAE2;

// Good
$wood-color: #eaeae2;
```
**[⬆ back to top](#table-of-contents)**

## Units

### Zero values

* Avoid specifying units for zero values.

```scss
// Bad
.square {
  border: 0px;
}

// Good
.square {
  border: 0;
}
```

### Float values

* Don't prefix float values or color parameters with a leading zero.

```scss
// Bad
.heading {
  margin-left: -0.75px;
}

// Good
.heading {
  margin-left: -.75px;
}
```

### `letter-spacing`

* Use `pt` units to declare `letter-spacing` values. We found it to be easier to match Photoshop visual specifications.

```scss
// Bad
.text {
  letter-spacing: -.75px;
}

// Good
.text {
  letter-spacing: -.75pt;
}
```

### `line-height`

* Do not add units for `line-height` values.

```scss
// Bad
p {
  line-height: 1.55px;
}

// Good
p {
  // Equivalent to 150% of the font size
  line-height: 1.5;
}
```

**[⬆ back to top](#table-of-contents)**

## Inline images

* Inline images are only allowed if they weight less than **1KB** and are present only once in the code.

**[⬆ back to top](#table-of-contents)**

## `!important`

* If you can't explain why an `!important` is important, it's probably not important at all :sunglasses:

**[⬆ back to top](#table-of-contents)**

## Pseudo elements

* Pseudo elements should be accessed by using a single colon `:`. Do not use double colons `::`.

```scss
// Bad
.button::before {
  outline: 1px solid;
}

// Good
.button:after {
  background: fuchsia;
}
```

**[⬆ back to top](#table-of-contents)**

## Comments

* The comment structure should look like the following:

```scss
/* ==========================================================================
   Component name
   ========================================================================== */

/**
 * Some description about my component.
 * Always try to be very concise and straightforward!
 */

.component {
  // ...
}

/* Sub component name
   ========================================================================== */

/**
 * Some description about my sub component.
 */

.component__sub-component {
}

/**
 * Modifier: Description of component modifier.
 */

.component--modifier {
  // ...
}

/**
 * Function: Description of component function.
 */
 
@function component-function() {
  // ...
}

/**
 * Placeholder: Description of component placeholder.
 */
 
%component-placeholder {
  // ...
}

/**
 * Mixin: Description of component mixin.
 */
 
@mixin component-mixin {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

## Quotes

* Always use double quotes `"`.

```scss
// Bad
.selector:after {
  content: 'Foo Bar';
}

// Good
.selector:before {
  content: "Lorem Ipsum";
}
```

### Attribute values

* Quote attribute values in selectors.

```scss
// Bad
input[type=radio] {
  display: none;
}

// Good
input[type="number"] {
  opacity: 1;
}
```

### Background URL

* Quote background urls.

```scss
// Bad
.selector {
  background: url(path/to/image.png) no-repeat;
}

// Good
.selector {
  background: url("path/to/image.png") no-repeat;
}
```

### `@import`

* Always wrap imported modules with double quotes `"`.

```scss
// Bad
@import helpers/foo;

// Good
@import "helpers/bar";
```

**[⬆ back to top](#table-of-contents)**

## Organization

* The order of declaration should look like:
	1. `@extend` 
	2. `@import`
	3. Variable definitions
	4. Functions, placeholders and mixins
	5. Other rules

```scss
// Bad
.button {
  border: 3px solid seagreen;
  @include border-radius(5px);
  background: fuchsia;
  @extend %button-base;
}

// Good
.button {
  @extend %button-base;
  @include border-radius(5px);
  
  border: 3px solid seagreen;
  background: fuchsia;
}

// Bad
.square {
  @include border-radius(3px);
  @extend %shape-base;
  @include box-sizing(border-box);
  @extend %square-base;
  // Other rules
}

// Good
.square {
  @extend %shape-base;
  @extend %square-base;
  @include border-radius(3px);
  @include box-sizing(border-box);
  
  // Other rules
}

// Bad
.selector {
  border: 0;
  @include font-smoothing;
  @mixin inner-mixin {
    // ...
  }
  @extend centered-content;
  %inner-placeholder {
    // ...
  }
  $color: red;
}

// Good
.selector {
  @extend centered-content;
  @include font-smoothing;
  
  $color: red;
  
  %inner-placeholder {
    // ...
  }
  
  @mixin inner-mixin {
    // ...
  }
  
  border: 1px solid gray;
}
```

**[⬆ back to top](#table-of-contents)**

## Specificity and nesting

* Avoid writing overly-specific selectors and large numbers of nested rules. Break them up when readability starts to be affected.
* Maximum of **3** levels of nesting.

```scss
// Bad
.menu {
  background: orange;
  .menu-item {
    font-family: sans-serif;
    .link {
      &:hover {
       color: red;
      }
      > .link-label {
        font-weight: bold;
      }
    }
  }
}

// Good
.menu {
  background: orange;
}

.menu-item {
  font-family: sans-serif;
}

.link:hover {
  color: red;
}

.link-label {
  font-weight: bold;
}
```

**[⬆ back to top](#table-of-contents)**

## Sass syntax

* You should follow Sass's **SCSS** syntax. Do not write code that follows the <s>SASS</s> syntax.

```sass
// Bad
.menu
  &.is-open
    opacity: 1
```

```scss
// Good
.menu {
  &.is-open {
    opacity: 1;
  }
}
```

**[⬆ back to top](#table-of-contents)**

## Variable definition

### Local variables

* Local variables should be named in lower case separating spaces with an hyphen `-`.

```scss
// Bad
$SquareSize: 20px;

// Good
$square-size: 50px;

// Bad
.selector {
  $BgColor: green;
}

// Good
.selector {
  $bg-color: green;
}
```

### Immutables (aka constants)

* Variables that act as constants should be named in `SNAKE_CASE_ALL_CAPS` followed by `!default`.

```scss
// Bad
$defaultBackground: fuchsia;

// Bad
$default-background: fuchsia !default;

// Good
$DEFAULT_BACKGROUND: fuchsia !default;
```

**[⬆ back to top](#table-of-contents)**

## Naming conventions

### Naming classes

* Classes should be named in lower case separating spaces with an hyphen `-`.

```scss
// Bad
.fooBar {
  border: none;
}

// Good
.foo-bar {
  outline: 1px solid red;
}

// Bad
.LoremIpsumDolor {
  text-align: center;
}

// Good
.lorem-ipsum-dolor {
  vertical-align: middle;
}
```

### Naming modifiers

* Modifiers should have the base module as a prefix and the name of the modifier, separated by double hyphens `--`.

```scss
.logo {
  background: url("./logo.png") no-repeat;
}

// Bad
.logoBig {
  transform: scale(1.5);
}

// Good
.logo--big {
  transform: scale(1.25);
}

// Bad
.logo-with-small-size {
  transform: scale(.25);
}

// Good
.logo--small {
  transform: scale(.5);
}
```

### Naming states

* States should indicate a state, as the name suggests. Always start states with "is", "has", "should" or "was".

```scsss
// Bad
.logo.logoHidden {
  opacity: 0;
}

// Good
.logo.is-hidden {
  opacity: 0;
}

// Bad
.logo.logo-with-border {
  border: 1px solid;
}

// Good
.logo.has-border {
  border: 1px solid fuchsia;
}
```

**[⬆ back to top](#table-of-contents)**

## Whitespace

### Tabs

* Use soft tabs set to 2 spaces and never mix spaces with tabs.

```scss
// Bad
.foo {
∙∙∙∙border: 1px solid red;
}

// Bad
.bar {
∙display: block;
}

// Bad
.baz {
⇥width: 100%;
}

// Good
.lorem {
∙∙box-sizing(border-box);
}
```

### EOF

* Always add an empty line at the end of your file.

```scss
// Bad
.main {
  background: red;
}

// Good
.main {
  background: blue;
}
↵
```

### Inline blocks

* Only selectors with a single property are allowed to be declared inline, add line breaks if there are more than a single one.

```scss
// Bad
.section { cursor: pointer; text-align: center; }

// Good
.section { cursor: default; }

// Good
.section {
  text-align: left;
  vertical-align: middle;
}
```

### Property names and values

* Always add a whitespace between property names and values.

```scss
// Bad
.selector {
  top:0;
  left:0;
}

// Good
.selector {
  top: 10px;
  left: 15px;
}
```

### Declaration blocks

* Include one space before the opening brace of declaration blocks.

```scss
// Bad
.selector{
  vertical-align: baseline;
}

// Good
.selector {
  vertical-align: middle;
}
```

### Multiple selectors

* When targeting multiple selectors break each one in a new line.

```scss
// Bad
.footer, .header, .main {
  display: block;
}

// Good
.footer,
.header,
.main {
  margin: 0 auto;
}
```

### Selector structure

* Add a line break after `@extend`/`@include`, variables, functions/placeholders/mixins and other rules.

```scss
// Bad
.selector {
  @extend centered-content;
  @include font-smoothing;
  $color: red;
  %inner-placeholder {
    // ...
  }
  @mixin inner-mixin {
    // ...
  }
  border: 1px solid gray;
}

// Good
.selector {
  @extend centered-content;
  @include font-smoothing;
  
  $color: red;
  
  %inner-placeholder {
    // ...
  }
  
  @mixin inner-mixin {
    // ...
  }
  
  border: 1px solid gray;
}
```

### Multiple rules

* Keep multiple rules in a single line but add a white space after each comma.

```scss
// Bad
.box {
  box-shadow: 0 1px 1px #eee,
  inset 0 1px 0 #f00;
}

// Good
.box {
  box-shadow: 0 1px 1px #eee, inset 0 1px 0 #f00;
}
```

### Multiple values

* Add a white space after each comma on multiple values.

```scss
// Bad
.selector {
  background: rgba(0,0,0,.5);
}

// Good
.selector {
  background: rgba(255, 255, 255, .75);
}
```

**[⬆ back to top](#table-of-contents)**

## Links

### Articles and blog posts

* [Improving Code Readability With CSS Styleguides](http://www.smashingmagazine.com/2008/05/02/improving-code-readability-with-css-styleguides/)
* [Line-Height Units](http://tzi.fr/css/text/line-height-units)

### Tools

* [CSSLint](https://github.com/CSSLint)
* [SublimeLinter-csslint](https://github.com/SublimeLinter/SublimeLinter-csslint)
* [csslint.vim](http://www.vim.org/scripts/script.php?script_id=3823)
* [st-snippets](https://github.com/rafaelrinaldi/st-snippets/tree/master/comments)

**[⬅ back to main](../../)**&nbsp;&nbsp;&nbsp;&nbsp;**[⬆ back to top](#table-of-contents)**