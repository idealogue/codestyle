
# ESH-E–ESH–ESH
Strategies for styling html like an eshay using sasshay.

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


