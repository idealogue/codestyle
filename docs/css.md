# CSS Styleguide
*Heavily influenced (pinched) from Nicolas Gallagher*

The following document outlines a reasonable style guide for CSS development. These guidelines strongly encourage the use of existing, common, sensible patterns. They should be adapted as needed to create your own style guide.

This is a living document and new ideas are always welcome. Please contribute.


## Table of contents

1. [General Rules](#general)
2. [Folder Structure](#folder)
3. [BEM__style--syntax](#bem)
4. [Molecules](#molecules)


<a name="general"></a>
## 1. General Rules

### Whitespace: 
Use 2 spaces. **Always**.

### Comments: 

Comment style should be simple and consistent within a single code base.

* Place comments on a new line above their subject.
* Keep line-length to a sensible maximum, e.g., 80 columns.
* Use "sentence case" comments and consistent text indentation.

```scss
// We pretty much just use sass inline comments.
// No need for this stuff to turn up in the final css file.
// You can use 'CMD + /' to comment multiple lines in sublime.
```

### Format

Use one selector per line & include a single space before the opening brace of a ruleset:

```scss
.selector-1,
.selector-2,
.selector-etc {
  // … 
}
```

Include one property per line & use one level of indentation for each declaration. Include a single space after the colon of a property and always close with a semi-colon, even on the last property in a block:

```scss
.selector {
  color: red; 
}
```

Use lowercase and long hex values: 

```scss
.selector {
  color: #aaaaaa; 
}
```

Use single or double quotes consistently. Preference is for double quotes:

```scss
.selector {
  content: "foo";
}
```

Use unitless values where possible:

```scss
.selector {
  line-height: 1.5;
}
```

Include a space after each comma in comma-separated property or function values:

```scss
.selector {
  background: linear-gradient(#ffffff, rgba(0, 0, 0, 0.8));
}
```

Separate each ruleset by a blank line:

```scss
.selector-a {
  // ...
}

.selector-b {
  // …
}
```

If declarations are to be consistently ordered, it should be in accordance with a single, simple principle. **Cluster related properties (e.g. positioning and box-model) together**:

```scss
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

### Exceptions and slight deviations

Long, comma-separated property values - such as collections of gradients or shadows - can be arranged across multiple lines in an effort to improve readability and produce more useful diffs. There are various formats that could be used; one example is shown below.

```css
.selector {
  background-image:
    linear-gradient(#ffffff, #cccccc),
    linear-gradient(#f3cccc, #4ecccc);
  box-shadow:
    1px 1px 1px #000000,
    2px 2px 1px 1px #cccccc inset;
}
```

### Preprocessors: additional format considerations

We use `.scss` syntax with libsass compilation - make sure you turn on sourcemaps in devtools (for your health).

Always place `@extend` statements on the first lines of a declaration block. Where possible, group `@include` statements at the top of a declaration block, after any `@extend` statements (breakpoints are obviously an exception).

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
## 2. Folder structure

The following shows the simple folder structure and core files we'll use for our projects. 

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
└── application.scss
```


<a name="bem"></a>
## 3. BEM__class--syntax

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
## 4. Molecules

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

