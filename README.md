#![logo](logo.png)

## Introduction :point_up:

This repository holds the guidelines we follow to write the front-end code of Magnetis.
The idea is to have a common place to look for and to enforce the same code standards throughout team members.

## Quotes :ear:

>All code in any code-base should look like a single person typed it, no matter how many people contributed
><small>– _Rick Waldon_</small>

&nbsp;

>Arguments over style are pointless. There should be a style guide, and you should follow it
><small>– _Rebecca Murphey_</small>

&nbsp;

>It’s harder to read code than to write it.
><small>– _Joel Spolsky_</small>

&nbsp;

>The only thing worse than other people’s code, is your own code 6 months later.
><small>– _Zeh Fernando_</small>

&nbsp;

>Part of being a good steward to a successful project is realizing that writing code for yourself is a Bad Idea™. If thousands of people are using your code, then write your code for maximum clarity, not your personal preference of how to get clever within the spec.
><small>– _Idan Gazit_</small>

## Styleguides :open_file_folder:

* <big>**[JavaScript](/JavaScript.md)**</big>
* <big>**[Stylesheet](/Stylesheet.md)**</big>
* <big>**[Markup](/Markup.md)**</big>

## Linters

Under `/linters` you will see configuration files we use to enforce the rules described on the styleguide via code linters.
ATM we use [JSHint](http://www.jshint.com) for Javascript and [SCSS lint](https://github.com/causes/scss-lint) for Sass with SCSS syntax.

## EditorConfig

Inside Magnetis we have people using Vim, Sublime Text and TextMate. We make use of the awesome project [EditorConfig](http://editorconfig.org) to help us maintain consistent coding styles between these different editors. The rules are specified on `editorconfig` file.

## Discussion :busts_in_silhouette:

:deciduous_tree: **This is a living project** :sunny: so you don't need to agree with everything presented by the documents.
Feel free to [open an issue](../../issues) to discuss changes and whatnot.
