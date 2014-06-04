# Stylesheet code styleguide

## Table of contents

## Introduction

* We follow some rules specified by SMACSS that enforces a more modular approach of writing components. You should take some time to understand it.
* We use Sass as a pre-processor of our CSS code. You should take some time to understand it as well.
* We use Compass as a Sass framework to help with things like vendor prefixes and spritesheet generation. Please, take some time to read its documentation.

## Spacing

## Comments


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

* Avoid being too specific when naming a class. Besides of being difficult to read it will likely turn into a mess.

```scss
// Bad
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

* Always place `@extend` and `@include` should stay at the top of the class definition. Also, you should add a line break before the last `@extend` or `@include`.

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
``` 

* `@extend` should come before `@include`.

```scss
// Bad
.square {
  @include border-radius(3px);
  @extend %shape-base;
  @include box-sizing(border-box);
  @extend %square-base;
}

// Good
.square {
  @extend %shape-base;
  @extend %square-base;
  @include border-radius(3px);
  @include box-sizing(border-box);
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

## Best practices

