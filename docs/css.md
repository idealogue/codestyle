# CSS Styleguide
*Heavily influenced (pinched) from Nicolas Gallagher*

The following document outlines a reasonable style guide for CSS development.
These guidelines strongly encourage the use of existing, common, sensible
patterns. They should be adapted as needed to create your own style guide.

This is a living document and new ideas are always welcome. Please
contribute.


## Table of contents

1. [General Principles](#general-principles)
2. [Whitespace](#whitespace)
3. [Comments](#comments)
4. [Format](#format)
5. [Folder Structure](#folder)
6. [BEM__style--syntax](#bem)
7. [Molecules](#molecules)

[License](#license)


<a name="general-principles"></a>
## 1. General principles

> "Part of being a good steward to a successful project is realizing that
> writing code for yourself is a Bad Idea™. If thousands of people are using
> your code, then write your code for maximum clarity, not your personal
> preference of how to get clever within the spec." - Idan Gazit

* Don't try to prematurely optimize your code; keep it readable and
  understandable.
* All code in any code-base should look like a single person typed it, even
  when many people are contributing to it.
* Strictly enforce the agreed-upon style.
* If in doubt when deciding upon a style use existing, common patterns.


<a name="whitespace"></a>
## 2. Whitespace

**Use: 2 spaces**

Only one style should exist across the entire source of your code-base. Always
be consistent in your use of whitespace. Use whitespace to improve
readability.

* _Never_ mix spaces and tabs for indentation.
* Choose between soft indents (spaces) or real tabs. Stick to your choice
  without fail.
* If using spaces, choose the number of characters used per indentation level.

Tip: configure your editor to "show invisibles" or to automatically remove
end-of-line whitespace.

Tip: use an [EditorConfig](http://editorconfig.org/) file (or equivalent) to
help maintain the basic whitespace conventions that have been agreed for your
code-base.


<a name="comments"></a>
## 3. Comments

Well commented code is extremely important. Take time to describe components,
how they work, their limitations, and the way they are constructed. Don't leave
others in the team guessing as to the purpose of uncommon or non-obvious
code.

Comment style should be simple and consistent within a single code base.

* Place comments on a new line above their subject.
* Keep line-length to a sensible maximum, e.g., 80 columns.
* Make liberal use of comments to break CSS code into discrete sections.
* Use "sentence case" comments and consistent text indentation.

Tip: configure your editor to provide you with shortcuts to output agreed-upon
comment patterns.

Example:

```css
// We pretty much just use sass inline comments.
// No need for this stuff to turn up in the final css file.
// You can use 'CMD+/' to comment multiple lines in sublime.
```


<a name="format"></a>
## 4. Format

The chosen code format must ensure that code is: easy to read; easy to clearly
comment; minimizes the chance of accidentally introducing errors; and results
in useful diffs and blames.

* Use one discrete selector per line in multi-selector rulesets.
* Include a single space before the opening brace of a ruleset.
* Include one declaration per line in a declaration block.
* Use one level of indentation for each declaration.
* Include a single space after the colon of a declaration.
* Use lowercase and shorthand hex values, e.g., `#aaa`.
* Use single or double quotes consistently. Preference is for double quotes,
  e.g., `content: ""`.
* Quote attribute values in selectors, e.g., `input[type="checkbox"]`.
* _Where allowed_, avoid specifying units for zero-values, e.g., `margin: 0`.
* Include a space after each comma in comma-separated property or function
  values.
* Include a semi-colon at the end of the last declaration in a declaration
  block.
* Place the closing brace of a ruleset in the same column as the first
  character of the ruleset.
* Separate each ruleset by a blank line.

```css
.selector-1,
.selector-2,
.selector-3[type="text"] {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  display: block;
  font-family: helvetica, arial, sans-serif;
  color: #333;
  background: #fff;
  background: linear-gradient(#fff, rgba(0, 0, 0, 0.8));
}

.selector-a,
.selector-b {
  padding: 10px;
}
```

#### Declaration order

If declarations are to be consistently ordered, it should be in accordance with
a single, simple principle.

**We prefer to cluster related properties (e.g. positioning and
box-model) together.**

```css
.selector {
  /* Positioning */
  position: absolute;
  z-index: 10;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;

  /* Display & Box Model */
  display: inline-block;
  overflow: hidden;
  box-sizing: border-box;
  width: 100px;
  height: 100px;
  padding: 10px;
  border: 10px solid #333;
  margin: 10px;

  /* Other */
  background: #000;
  color: #fff;
  font-family: sans-serif;
  font-size: 16px;
  text-align: right;
}
```

#### Exceptions and slight deviations

Long, comma-separated property values - such as collections of gradients or
shadows - can be arranged across multiple lines in an effort to improve
readability and produce more useful diffs. There are various formats that could
be used; one example is shown below.

```css
.selector {
  background-image:
    linear-gradient(#fff, #ccc),
    linear-gradient(#f3c, #4ec);
  box-shadow:
    1px 1px 1px #000,
    2px 2px 1px 1px #ccc inset;
}
```

### Preprocessors: additional format considerations

We use .scss sass syntax.

* Limit nesting to 1 level deep. Reassess any nesting more than 2 levels deep.
  This prevents overly-specific CSS selectors.

**TODO:** Establish a system for breakpoint / `&:after` nesting etc.

* Always place `@extend` statements on the first lines of a declaration
  block.
* Where possible, group `@include` statements at the top of a declaration
  block, after any `@extend` statements (breakpoints are obviously an exception).

**TODO:** Consider mixin prefix convention for Idealogue mixins...

```scss
.selector-1 {
  @extend .other-rule;
  @include clearfix();
  @include box-sizing(border-box);
  width: grid-unit(1);
  // other declarations
}
```
<a name="folder"></a>
## Folder structure

The following shows the simple folder structure and core files we'll use for our projects:

```
.
├── _global
│   ├── mixins
│   │   ├── _breakpoints.scss
│   │   ├── _compass.scss
│   │   ├── _icons.scss
│   │   └── _snippets.scss
│   │   
│   ├── reset
│   │   ├── _formalize.scss
│   │   └── _normalize.scss
│   │   
│   ├── _base.scss
│   ├── _fonts.scss
│   ├── _javascript.scss
│   └── _variables.scss
│   
│   ├── molecule_folders
│   
├── _cbf.scss
└── styles.scss
```
<a name="bem"></a>
## BEM__class--syntax

Basic strucure:

```css
.class {}
.class__sub-class {}
.class__sub-class--modifier {}
```

When you do a thing with css aka, build a widget, you're going to call it something. This is known as the parent class, or module, or widget name - whatever you want really.

```css
.list { }
```

When building out a class you're going to need to tie the children to the parent somehow, but we don't want to _physically_ nest all our classes so we create sub-classes like so:

```css
.list__heading {}
.list__item {}
```

If we want to modify something slightly from it's base class we can use a modifier, this keeps it tightly coupled to the original class/subclass, but allows for variation.

```css
.list__heading--large {
  @extend .list__heading;
  font-size: 30px;
}
```

If you need to nest more than one level deep of subclasses you're probably looking to either create a new class or use private classes (discussed below).

```css
// don't do this…
.class__sub-class__sub-sub-class__sub-class-of-a-subclass {}
```
<a name="molecules"></a>
## Molecules

What _are_ molecules? We just don't know. But here we'll try and explain as best we can the how we think about our css in relation to our html in relation to that thing on the screen someone's gonna click.

First of all, this is awful:

```html
<!-- don't do this -->
<div class="some-class layout_stuff float-right padding-module all-the-etcs"></div>
```

The main reason being the cognative overload we face when trying to track down multiple classes driving different style across our css file/folder structure.

For the same reasons, try not to do a combination of the following (especially when all the other classes are in different files).

```html
<div class="awesome_class"></div>
```

```css
.awesome_class {
  @extend .some-class;
  @extend .layout_stuff; 
  @extend .float-right; 
  @extend .padding-module; 
  @extend .all-the-etcs;
}
```

**Our objective should be to look at a block of html, view code, whatever (probably in the devtools inspector) and know exactly where to go and modify those styles.**

With that in mind we propose the following guidelines:

- Restrict your child classes to a single level deep.
- Create as many "molecule" classes as you like, go nuts.
- Refactor in to re-usable patterns _after the fact_
- When you need to go deeper use nested "private" classes:

```css
// private class example
.list {}
.list__item {
  ._icon {}
  ._text {}
}
.list__heading {}
.list__heading--large {
  @extend .list__heading;
  font-size: 30px;
}
```

Private classes allow us to re-use common names like icon, text, title etc without namespacing them on the actual class name - and sass nesting makes it easy to manage. Simples.




<a name="license"></a>
## License

_Principles of writing consistent, idiomatic CSS_ by Nicolas Gallagher is
licensed under the [Creative Commons Attribution 3.0 Unported
License](http://creativecommons.org/licenses/by/3.0/). This applies to all
documents and translations in this repository.

Based on a work at
[github.com/necolas/idiomatic-css](https://github.com/necolas/idiomatic-css).
