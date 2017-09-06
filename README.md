# Enterprise UI CSS/HTML Guidelines
## HTML Principles
### Keep HTML Semantic as possible.
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

## CSS Rules
![alt Example](http://i.imgur.com/PyIwwrt.png)

### Semicolons

While the semicolon is technically a separator in CSS, always treat it as a terminator.

```css
/* bad */
div {
  color: red
}

/* good */
div {
  color: red;
}

### Specificity

Don't make values and selectors hard to override. Minimize the use of `id`'s
and avoid `!important`.

```css
/* bad */
.bar {
  color: green !important;
}
.foo {
  color: red;
}

/* good */
.foo.bar {
  color: green;
}
.foo {
  color: red;
}

### Box model

The box model should ideally be the same for the entire document. A global
`* { box-sizing: border-box; }` is fine, but don't change the default box model
on specific elements if you can avoid it.

```css
/* bad */
div {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
}

/* good */
div {
  padding: 10px;
}
```

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

### Spacing

1. Always have a space between selector and opening bracket, between property and value.
2. Indent properties
3. New line between selectors.

```scss
/* bad */
.login{
  float:left;
  widht:300px;
  input {
    color:red;
    background-color:yellow;
  }
  button {
    padding:20px;
  }
}

/* good */
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


### Element Push (with Margin)

It is the element's responsibility to push down on other elements. While this rule is not strickly enforced, please be aware of it.

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

header {
  margin: 20px;

  .menu
    margin-bottom: 10px;
  }

  .item-below-menu {
    margin-top: 10px;
    margin-bottom: 0px;
  }
}

```

### Animations

Favor transitions over animations. Avoid animating other properties than
`opacity` and `transform`.

```css
/* bad */
div:hover {
  animation: move 1s forwards;
}
@keyframes move {
  100% {
    margin-left: 100px;
  }
}

/* good */
div:hover {
  transition: 1s;
  transform: translateX(100px);
}
```

### CSS/SASS
1. Limit !importnat
  1. Important!’s should only be used to override vendor classes that already have important! or to override vendor plugins that have inline CSS. 
2. Media Queries should exist inside each class/element declaration and should not be combined with other elements. 
3. CSS Shorthand
  1. Use css shorthand as much as possible if you are declaring more than 2 values for margin, padding, border, animation.
  2. The only exception to not using shorthand is Background. 


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

Use the Sass ampersand `&` for nesting elements inside the original parent block.

```scss
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


For MODIFIERS, we stack the modifiers in the block element. **NOT on the element level.**

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

However, we can eliminate the use of multiple class names on the HTML by extending the parent block using `@extend`

>```scss
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

or, similarly to how `this` scopes in javascript, we can create a `$this` variable that serves as a reference to the parent block `.button`.

>```scss
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

One problem we run into with BEM is having to modify the child element of a modified parent block. In this example we want to modify the link color inside form when it is active. There are several solutions:

**Nesting by declaring classes (not ideal)**
```html

<div class="form">
  <input class="form__input" type="text">
  <div class="form__link"></div>
</div>

<div class="form--active">
  <input class="form__input" type="text">
  <div class="form__link"></div>
</div>
```
```scss
.form {
  &--active {
    @extend .form;
    .form__link { color: blue; //Override original Property}
  }
  &__input {..}
  &__link {color: red; }
}

// Output
.form__link { color: red}
.form--active .form__link { color: blue}

```

In the next example, we are going to output the same css but we will be using `#{$this}` to create an instance of the block inside the modifier. This is the best way to solve accessing a child element inside a parent block modifier.

**Nesting with SASS variables**

```scss
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




