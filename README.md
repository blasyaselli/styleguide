# Enterprise UI CSS/HTML Guidelines

High level guidelines:

* Be consistent.
* Don't rewrite existing code to follow this guide.
* Don't violate a guideline without a good reason.
* A reason is good when you can convince a teammate.

A note on the language:

* "Avoid" means don't do it unless you have good reason.
* "Don't" means there's never a good reason.
* "Prefer" indicates a better option and its alternative to watch out for.
* "Use" is a positive instruction.


- [HTML Principles](#html-principles)
    - [Keep HTML Semantic as possible.](#keep-html-semantic-as-possible)
- [CSS Principles](#css-principles)
    - [Formatting](#formatting)
    - [Selectors](#selectors)
    - [Order](#order)
    - [!important Read this!!](#important-read-this)
    - [Positioning](#positioning)
- [CSS Philosophies](#css-philosophies)
    - [Pushing Elements (with Margins)](#pushing-elements-with-margins)
    - [Centering](#centering)
    - [Box model](#box-model)
    - [Animations](#animations)
- [BEM - Basic](#bem---basic)
    - [BEM & SASS](#bem--sass)
    - [BEM & SASS - Modefiers and child elements](#bem--sass---modefiers-and-child-elements)



## HTML Principles
### Keep HTML Semantic as possible.
  * Prefer double quotes for attributes.
  * Use HTML5 Tags when applicable as opposed to divs `<sections>, <articles>, <header>, <footer>, <nav> etc.`
  * Respect heading hierarchy and nesting (SEO and Accessibility)
  `H1 > H2 > H3 > H4 > H5`
  * Use classes over IDs
  * Use less tags as possible

> ```html
> <!-- bad -->
> <div class="header">
>   <div class="menu">
>     <div class="primary-menu">
>       <ul>
>           <li><a href=""><span>item 1</span></a></li>
>           <li><a href="">item 2</a></li>
>         </ul>
>     </div>
>   </div>
> </div>
> 
> <!-- good -->
> <header class="header">
>   <nav class="menu">
>       <ul class="menu--primary">
>         <li><a href="">item 1</a></li>
>         <li><a href="">item 2</a></li>
>       </ul>
>     </div>
>   </nav>
> </header>
> ```


## CSS Principles
![alt Example](http://i.imgur.com/PyIwwrt.png)

### Formatting

* Use the SCSS syntax.
* Follow BEM methedology (see Bem)
* Indent properties (2 spaces)
* Use hyphens when naming mixins, extends, classes, functions & variables: `span-columns` not `span_columns` or `spanColumns`.
* Use one space between property and value: `width: 20px` not `width:20px`.
* Use semicolon at the end of a declaration: `width: 20px;` not `width: 20px `.
* Use a blank line above a selector that has styles.
* Prefer kite variables and hex color codes `#fff` or `#FFF` not `white`.
* Avoid using shorthand properties for only one value: `background-color: #ff0000;`, not `background: #ff0000;`.
* Use shorthand properties to combine values when 3 or more are declared.
> ```scss
> // Bad
> .post {
>   margin-top: 10px;
>   margin-bottom: 10px;
>   margin-right: 0px;
>   margin-left: 0px;
> }
> 
> // Better
> .post {
>   margin: 10px 0px 10px 0px;
> }
> 
> // Best
> .post {
>   margin: 10px 0px;
> }
> ```
* Use one space between selector and `{`.
* Use only lowercase, except for hex or string values.
* Use a leading zero in decimal numbers: `0.5` not `.5`.
* Use space around operands: `$variable * 1.5`, not `$variable*1.5`.

```scss
// Bad
.login{
  float:left;
  width:300px;
  input {
    color:red;
    background-color:yellow;
  }
  button {
    padding:20px;
  }
}

// Good
.login {
  float: left;
  width: 300px;

  input {
    color: red;
    background-color: yellow;
  }

  button {
    padding: 20px;
  }
}
```

### Selectors

* Don't use ID's for style.
* Avoid over-qualified selectors: `h1.page-title`, `div > .page-title`.
* Avoid nesting more than 3 selectors deep.
* Avoid nesting within a media query.


### Order
* Place `@extends` and `@includes` at the top of your declaration list. **EXCEPT** when `@include` is a media query mixin.
* Place media queries directly after the declaration list without a new line.
* If 5 or more delecration, order them as follows:
  1. Layout Properties (`position`, `float`, `clear`, `display`)
  2. Box Model Properties (`width`, `height`, `margin`, `padding`)
  3. Visual Properties (`color`, `background`, `border`, `box-shadow`)
  4. Typography Properties (`font-size`, `font-family`, `text-align`, `text-transform`)
  5. Misc Properties (`cursor`, `overflow`, `z-index`)å
* Place pseudo-states and pseudo-elements before BEM elements.
* Place BEM Modifiers before BEM elements (see BEM).
* Avoid empty lines between closing brackets `}`.

Example:
```scss
.post {
  &__heading {
    font-size: 10px;
    padding: 10px;
    &:hover {
      color: blue;
    }
  }

  &__seperator {
    width: 100%;
    float: left;
    margin: 14px 0px;
    @include media-breakpoint-down(sm) {
      display: none;
    }
  }

  &__content {
    @extend .content;
    position: realtive;
    float: left;
    width: 100%;
    padding: 10px;
    background-color: red;
    border: 1px solid black;
    text-align: left;
    overflow: hidden;
    z-index: 0;
    @include media-breakpoint-down(xl) {
      width: 300px;
      float: none;
      text-align: center;
    }

    p {
      line-height: 1em;
    }
  }
}
```

### !important Read this!!
* Limit the use of `!important` - They should only be used to override vendor classes that already have important! or to override vendor plugins that have inline CSS. Or if Blas says you can.


### Positioning

There are many ways to position elements in CSS but try to restrict yourself to the
properties/values below. By order of preference:

```
display: block;
position: relative;
display: flex;
position: sticky;
position: absolute;
position: fixed;
```

**Important:** Keep in mind to also prioritize `float: left`  and `float:right` for fluid/responsive design before implementing flex.


## CSS Philosophies 

### Pushing Elements (with Margins)

* Avoid pushing an element with paddings, use margins.
* Typical, push down and right. It is the element's responsibility to push down on other elements. While this rule is not strickly enforced, please be aware of it.

```scss
/* Not ideal */
header {
  margin: 0;

  .menu
    margin-top: 20px;
  }

  .item-below-menu {
    margin-top: 10px;
    margin-bottom: 0px;
  }
}

/* Ideal */
header {
  margin-bottom: 20px;

  .menu
    margin-bottom: 10px;
  }

  .item-below-menu {
    margin-top: 10px;
    margin-bottom: 0px;
  }
}

```

### Centering

For horizontal positioning use `margin:  0 auto` before resorting flex.

```scss
/* Horizontal Centering without Flex */
.container-to-center {
  margin: 0 auto;
  width: 500px;
}
```

More information on Flex: https://css-tricks.com/snippets/css/a-guide-to-flexbox/

### Box model

The box model should ideally be the same for the entire document. A global
`* { box-sizing: border-box; }` is fine, but don't change the default box model
on specific elements if you can avoid it.

```css
// Bad
div {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
}

// Good
div {
  padding: 10px;
}
```

### Animations

Favor transitions over animations. Avoid animating other properties than
`opacity` and `transform`.

```css
/* bad */
div {
  &:hover {
    animation: move 1s forwards;
  }
}
@keyframes move {
  100% {
    margin-left: 100px;
  }
}

/* good */
div {
  &:hover {
    transition: 1s;
    transform: translateX(100px);
  }
}
```



## BEM - Basic
Block Element Modifier is a methodology that helps you to create reusable components and code sharing in front-end development. 

Overview and Additional Notes: [BEM — Block Element Modifier](http://getbem.com/naming/)

![alt Example](https://i.imgur.com/JpnCkq5.png)

Basic Example of BEM with HTML/CSS
```html
<style>
.block {
  width: 200px;
  height: 100px;
  background-color: red;
}

.block--modifier {
  background-color: blue;
}

.block__element {
  border: 1px solid red;
}
</style>

<div class="block">
  <div class="block__element">...</div>
</div>

<!-- modified block -->
<div class="block block--modifier">
  <div class="block__element">...</div>
</div>

```
### BEM & SASS

SASS and BEM are a match made in heaven. SASS has a lot of powerful features that are used to make much readable BEM code.

Use the Sass ampersand `&` for nesting elements inside the original parent BLOCK.

```sass
// SASS File
.block {
  &__element {
    color: blue;
  }
  &__second-element {
    float: left;
  }
}

// The above will output
.block__element {
  color: blue;
}
.block__second-element {
  float: left;
}
```


For MODIFIERS, we stack the modifiers in the BLOCK level. **NOT on the ELEMENT level.**

>Bad
>```html
><div class="button">
>  <div class="button__icon--blue"></div>
></div>
>```
>Good
>```html
><div class="button button--blue ">
>  <div class="button__icon"></div>
></div>
>```

However, we can eliminate the use of multiple class names on the HTML by extending the parent BLOCK using `@extend`

>```sass
>.button {
>  padding: 10px;
>  background-color: red;
>  &--rounded {
>    @extend .button;
>    border-radius: 10px;
>  }
>}
>```
>```html
><div class="button--rounded">
>  ...
></div>
>```

or, similarly to how `this` scopes in javascript, we can create a `$this` variable that serves as a reference to the parent BLOCK `.button`.

>```sass
>.button {
>  $this: &;
>  padding: 10px;
>  background-color: red;
>  &--blue {
>    @extend $this;
>    background-color: blue;
>  }
>  &--rounded {
>    @extend $this;
>    border-radius: 10px;
>  }
>}
>```

### BEM & SASS - Modefiers and child elements

One problem BEM users run into is having to modify a child ELEMENT of a modified parent BLOCK. In this example we want to modify the link color inside `.form` when it is active, `.form--active`. 

**Nesting by declaring classes inside Modified BLOCK**
>```html
>// HTML without form active
><div class="form">
>  <input class="form__input" type="text">
>  <div class="form__link"></div>
></div>
>//HTML with active form
><div class="form--active">
>  <input class="form__input" type="text">
>  <div class="form__link"></div>
></div>
>```
>```sass
>.form {
>  &--active {
>    @extend .form;
>    .form__link { color: blue; //Override original Property}
>  }
>  &__input {..}
>  &__link {color: red; }
>}
>
>// Output
>.form__link { color: red}
>.form--active .form__link { color: blue}
>```

In the next example, we are going to output the same css but we will be using `#{$this}` to create an instance of the block inside the modifier. This is the best way to solve accessing a child element inside a parent block modifier.

**Nesting with SASS variables**

```sass
.block {
  $this: &; // Asign .block to variable $this
  background-color: red;

  &--modifier {
    @extend $this;
    background-color: blue;
    #{$this} {     // modifies element inside modifier
      &__element {
        border: 1px solid blue;
      }
    }
  }

  &__element {
    text-align: center;
    border: 1px solid red;
  }
}
```






