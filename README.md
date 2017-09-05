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

### BEM
Block Element Modifier is a methodology that helps you to create reusable components and code sharing in front-end development.

Additional resources: [BEM — Block Element Modifier](http://getbem.com/naming/)

![alt text](https://i.imgur.com/JpnCkq5.png)

HTML
```html
<div class="block">...</div>
<div class="block__element">...</div>
<div class="block__element--modifier">...</div>
```

Basic Example of BEM with HTML/CSS
```
<style>
// Main Block
.block {
  width: 200px;
  height: 100px;
  background-color: red;
}
.block--modifier { // Main Block with modifier that overrides background from red to blue
  background-color: blue;
}

.block__element { // Element inside Main Block
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

Typically in BEM, we stack the modifiers on the block element. If we have a button that contains a modifier for color and rounded corners, the html will be `<div class="button button--blue button--rounded"></div>`. However, this can create a lot of redudent harder to read HTML. To solve this problem, we will use the `@extend` feature in SASS that will allow us to extend the original block.

```sass
.button {
  padding: 10px;
  background-color: red;
  &--blue {
    @extend .button;
    background-color: blue;
  }
  &--rounded {
    @extend .button;
    border-radius: 10px;
  }
}
```

or, to match the way we scope in javascript, we can create `$this` variable that serves as `.button`.
```scss
.button {
  $this: &;
  padding: 10px;
  background-color: red;
  &--blue {
    @extend $this;
    background-color: blue;
  }
  &--rounded {
    @extend $this;
    border-radius: 10px;
  }
}
```



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



