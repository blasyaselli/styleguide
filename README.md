# Enterprise UI CSS/HTML Guidelines
## HTML Principles
### Keep HTML Semantic as possible.
  * Use HTML5 Tags when applicable as opposed to divs `<sections>, <articles>, <header>, <footer>, <nav> etc.`
  * Respect heading hierarchy and nesting (SEO and Accessibility)
  `H1 > H2 > H3 > H4 > H5`
  * Use classes over IDs
  * Use less tags as possible

> *Bad*
> ```html
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
> ```
> 
> *Good*
> ```html
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

### BEM - Basic
Block Element Modifier is a methodology that helps you to create reusable components and code sharing in front-end development.

Additional resources: [BEM — Block Element Modifier](http://getbem.com/naming/)

![alt text](https://i.imgur.com/JpnCkq5.png)

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
<div class="block block__element--modifier">
  <div class="block__element">...</div>
</div>

```
### BEM with SASS

Using the Sass ampersand `&`, we can nest elements inside the original block.

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


With BEM, we stack the modifiers in the block element. 
```html
<div class="button button--blue button--rounded">
  ...
</div>
<div class="button button--blue button--rounded button--active">
  ...
</div>
```
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

### BEM - Modefiers and element blocks

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

```sass
.block {
  $this: &; // Asign .block to variable $this
  width: 200px;
  height: 100px;
  background-color: red;
  &--modifier {
    background-color: blue;
    // Modifying an element with a parent modifier
    #{$this} {
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


## CSS Rules

### Positioning Elements
1. Prioritize floating left and floating right (over absolute positioning) as this will allow more responsive functionality. 
2. Flexbox for layouts when applicable
  1. Exception: horizontal positioning use `margin:  0 auto`
  2. Exception: stacking rows use: `float: left; width: 100%`
3. Absolute Position and Translate as a last resort, or only when necessary (fixed headers, loaders, popups). 
4. Element Responsibility to push down on other elements. 

### CSS/SASS
1. Limit !importnat
  1. Important!’s should only be used to override vendor classes that already have important! or to override vendor plugins that have inline CSS. 
2. Media Queries should exist inside each class/element declaration and should not be combined with other elements. 
3. CSS Shorthand
  1. Use css shorthand as much as possible if you are declaring more than 2 values for margin, padding, border, animation.
  2. The only exception to not using shorthand is Background. 



