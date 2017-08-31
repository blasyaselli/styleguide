# Enterprise UI CSS/HTML Guidelines
## HTML Principles
1. Keep HTML Semantic as possible.
  * Use HTML5 Tags when applicable as opposed to divs `<sections>, <articles>, <header>, <footer>, <nav> etc.`
  * Respect heading hierarchy and nesting (SEO and Accessibility)
  `H1 > H2 > H3 > H4 > H5`
  * Use classes over IDs
  * Use less tags as possible

*Bad*
```
<div class="header">
  <div class="menu">
    <div class="primary-menu">
      <ul>
          <li><a href=""><span>item 1</span></a></li>
          <li><a href="">item 2</a></li>
        </ul>
    </div>
  </div>
</div>

```

*Good*
```
<header class="header">
  <nav class="menu">
      <ul class="menu--primary">
        <li><a href="">item 1</a></li>
        <li><a href="">item 2</a></li>
      </ul>
    </div>
  </nav>
</header>
```

2. BEM
Block Element Modifier is a methodology that helps you to create reusable components and code sharing in front-end development.
Additional resources: [BEM — Block Element Modifier](http://getbem.com/naming/)

```
<div class="block">...</div>
<div class="block__element">...</div>
<div class="block__element--modifier">...</div>
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



