# `<magnetis />`

Magnetis markup code styleguide.

## Table of contents

* [Disclaimer](#disclaimer)
* [Doctype](#doctype)
* [`lang`](#lang)
* [IE compatibility mode](#ie-compatibility-mode)
* [Encoding](#encoding)
* [CSS and JavaScript includes](#css-and-javascript-includes)
* [Naming](#naming)
* [Attribute order](#attribute-order)
* [Boolean attributes](#boolean-attributes)
* [Quotes](#quotes)
* [Self closing tags](#self-closing-tabs)
* [Optional closing tags](#optional-closing-tags)
* [Inline JavaScript](#inline-javascript)
* [Semantics](#semantics)
* [Whitespace](#whitespace)
  * [Tabs](#tabs)
  * [EOF](#eof)
  * [Multiple attributes](#multiple-attributes)
  * [Closing self closing tags](#closing-self-closing-tags)
* [Templates](#templates)
* [`x-template`](#x-template)
* [`id` over `class`](#id-over-class)
* [Links](#links)
  * [Websites](#websites)

## Disclaimer

This document is inspired by [Mark Otto's Code Guide](http://mdo.github.io/code-guide/#html), [Harry Roberts's coding style](http://csswizardry.com/2012/04/my-html-css-coding-style) and [Idiomatic HTML](https://github.com/necolas/idiomatic-html).

**[⬆ back to top](#table-of-contents)**

## Doctype

* Follow HTML5 doctype to enforce standards and a more consistent rendering throughout browsers.

```html
<!doctype html>
<html>
  <head></head>
  <body></body>
</html>
```

**[⬆ back to top](#table-of-contents)**

## `lang`

* You should specify the language of the documents. We currently only support `pt-br`.

```html
<html lang="pt-br">
</html>
```

**[⬆ back to top](#table-of-contents)**

## IE compatibility mode

Internet Explorer supports the use of a document compatibility `<meta>` tag to specify what version of IE the page should be rendered. You should force edge mode.

```html
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
```

**[⬆ back to top](#table-of-contents)**

## Encoding

* Use `UTF-8` as de default encoding of HTML documents.

```html
<head>
  <meta charset="utf-8">
</head>
```

**[⬆ back to top](#table-of-contents)**

## CSS and JavaScript includes

* There's no need to specify the `type` attribute of include tags.

```html
<!-- External CSS -->
<link rel="stylesheet" href="path/to/external.css">

<!-- Inline CSS -->
<style>
  /* ... */
</style>

<!-- JavaScript -->
<script src="path/to/script.js"></script>
```

**[⬆ back to top](#table-of-contents)**

## Naming

* Always use lowercase tag and attribute names.

```html
<!-- Bad -->
<div id="advertisingModal" class="ModalBase dropShadowFX"></div>

<!-- Good -->
<div id="advertising-modal" class="modal-base drop-shadow-fx"></div>
```

**[⬆ back to top](#table-of-contents)**

## Attribute order

* The order of attributes should look like:
	1. Element `id` and/or `name`
	2. JavaScript class name `js-selector`
	3. Generic classes (clearfix, grid stuff, etc)
	4. Other classes
	5. States
	6. Data attributes `data-*`
	7. Other attributes
	8. Boolean attributes

```html
<!-- Bad -->
<div class="clearfix is-opaque js-button button grid-size-2" id="btn" hidden data-fx="fade"></div>

<!-- Good -->
<div id="btn" class="js-button clearfix grid-size-2 button is-opaque" data-fx="fade" hidden></div>
```

**[⬆ back to top](#table-of-contents)**

## Boolean attributes

* Do not add values to boolean attributes.

```html
<!-- Bad -->
<input type="checkbox" value="Bar" checked="checked">Foo</input>

<!-- Good -->
<input type="checkbox" value="Bar" checked>Foo</input>

<!-- Bad -->
<input type="submit" disabled="disabled">Send email</input>

<!-- Good -->
<input type="submit" disabled>Send email</input>
```

**[⬆ back to top](#table-of-contents)**

## Quotes

* Always use double quotes `"` to wrap attribute values.

```html
<!-- Bad -->
<button id=my-button data-fx='fade'>Fade!</button>

<!-- Good -->
<button id="my-button" data-fx="fade">Fade!</button>
```

**[⬆ back to top](#table-of-contents)**

## Self closing tags

* Always close self closing tags with a slash `/`, no matter what.

```html
<!-- Bad -->
<p>Lorem ipsum<br>dolor sit</p>

<!-- Good -->
<p>Lorem ipsum<br />dolor sit</p>

<!-- Bad -->
<img src="path/to/image.png">

<!-- Good -->
<img src="path/to/image.png" />
```

**[⬆ back to top](#table-of-contents)**

## Optional closing tags

* Always close optional closing tags

```html
<!-- Bad -->
<ul>
  <li>Once
  <li>Upon
  <li>A Time
</ul>

<!-- Good -->
<ul>
  <li>Once</li>
  <li>Upon</li>
  <li>A Time</li>
</ul>
```

**[⬆ back to top](#table-of-contents)**

## Inline JavaScript

* Avoid writing JavaScript at all in HTML documents as they're hard to spot.

**[⬆ back to top](#table-of-contents)**

## Semantics

* Strive to maintain HTML standards and semantics, but not at the expense of practicality. Use the least amount of markup with whenever possible.

**[⬆ back to top](#table-of-contents)**

## Whitespace

### Tabs

* Use soft tabs set to 2 spaces and never mix spaces with tabs.

### EOF

* Always add an empty line at the end of your file.

### Multiple attributes

* It's cool to break multiple attributes to enforce code readability. Just keep the following format:

```html
<div
  class="modal"
  id="main-modal"
  style="display: none;"	
  hidden
>
</div>
```

### Closing self closing tags

* When closing self closing tags, always add a single space before `/>`.

```html
<!-- Bad -->
<img src="doge.png"/>

<!-- Good -->
<img src="doge.png" />

```

## Templates

### `x-template`

* Template elements should have their `type` value set to `x-template`.

```html
<!-- Bad -->
<script type="text-template" id="box-template">
  <div class="box"></div>
</script>

<!-- Good -->
<script type="x-template" id="box-template">
  <div class="box"></div>
</script>
```

### `id` over `class`

* Template elements should be identified through `id` instead of `class`. Add `template` suffix as well.

```html
<!-- Bad -->
<script type="x-template" class="js-component">
  <p>Component</p>
</script>

<!-- Good -->
<script type="x-template" id="component-template">
  <p>Component</p>
</script>
```

**[⬆ back to top](#table-of-contents)**

## Links

### Websites

* [HTML5 Doctor](http://html5doctor.com)

**[⬅ back to main](../../)**&nbsp;&nbsp;&nbsp;&nbsp;**[⬆ back to top](#table-of-contents)**
