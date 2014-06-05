# Stylesheet code styleguide

## Table of contents

## Introduction

* We follow some rules specified by SMACSS that enforces a more modular approach of writing components. You should take some time to understand it.
* We use Sass as a pre-processor of our CSS code. You should take some time to understand it as well.
* We use Compass as a Sass framework to help with things like vendor prefixes and spritesheet generation. Please, take some time to read its documentation.

## Inline images

* Inline images are only allowed when they're less than 1KB and are present only once on the code.

## `!important`

* If you can't explain why an `!important` is important, it's probably not important.

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

## Spacing

* Always add a whitespace after property names.

```scss
// Bad
```


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


## Quotes

* Always use double quotes.

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

### `@import`

* Always wrap imported modules with double quotes `"`.

```scss
// Bad
@import helpers/foo;

// Good
@import "helpers/bar";
```

## Nesting

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
      > p {
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

.link > p {
  font-weight: bold;
}

.link:hover {
  color: red;
}
```
## Specificity

* Avoid being too specific when naming a class. Besides of being difficult to read it will likely turn into a dirty mess.

```scss
// Bad
.menu .menu-item > .link h1 {
  font-face: Comic Sans;
}

// Good
.link h1 {
  font-face: Helvetica;
}

// Good
.link-title {
  font-face: serif;
}
```

## Sass and Compass

### Sass syntax

* You should follow Sass's **SCSS** syntax. Do not write code that follows the <s>SASS</s> syntax.

```sass
// Bad
.menu
	.item
		&.is-open
			opacity: 1
```

```scss
// Good
.menu {
	.item {
		&.is-open {
			opacity: 1;
		}
	}
}
```

### Order

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

### Variable definition

* Local variables should follow the same rules of class definition.
* Variables that act as constants (they values do not change) should be named in `SNAKE_CASE_ALL_CAPS` followed by `!default`.

```scss
// Bad
$SquareSize: 20px;

// Good
$square-size: 50px;

// Bad
.selector {
  $Color: green;
}

// Good
.selector {
  $color: green;
}

// Bad
$defaultBackground: fuchsia;

// Good
$DEFAULT_BACKGROUND: fuchsia !default;
```

### Sass best practices



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


## Best practices

