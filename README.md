#Sprinklr Style Guide

##Naming Conventions and Methodologies
BEM stands for “Block”, “Element”, “Modifier”. It is a front-end methodology: a new way of thinking when developing Web interfaces.

Read more about it here: https://en.bem.info/


#####Block
A block is an independent entity, a “building block” of an application. A block can be either simple or compound (containing other blocks).

Block names must be unique within a project to unequivocally designate which block is being described. Only instances of the same block can have the same names.

A block name follows the block-name scheme and defines a namespace for elements and modifiers.

*Example*
```
menu
lang-switcher
```

*HTML*
```html
<div class="menu">...</div>
```

*CSS*

```css
.menu { color: red; }
```

#####Element
An element is a part of a block that performs a certain function. Elements are context-dependent: they only make sense in the context of the block that they belong to.

An element name is delimited by a double underscore (__).

Element names must be unique within the scope of a block. An element can be repeated several times.

*Example*

```
menu__item
lang-switcher__lang-icon
```

*HTML*

```html
<div class="menu"> ... <span class="menu__item"></span> </div>
```

*CSS*

```css
.menu__item { color: red; }
```

#####Modifier
A modifier defines a different state or version of a block or an element.

A modifier name is delimited by double hyphens (--).

*Example*

```
menu--hidden
```

*HTML*

```html
<div class="menu menu--hidden">...</div>
```

>Incorrect notation

>```html
><div class="menu--hidden">...</div>
>```
>Here the notation is missing the block that is affected by the modifier.

*CSS*

```css
.menu--hidden { 
    @include hidden; /* Using Bootstrap */  
}
```

The naming convention follows this pattern:

```css
.block {}
.block__element {}
.block--modifier {}
```

.block represents the higher level of an abstraction or component.
.block__element represents a descendent of .block that helps form .block as a whole.
.block--modifier represents a different state or version of .block.

The reason for double rather than single hyphens and underscores is so that your block itself can be hyphen delimited, for example:

```css
.site-search {} /* Block */
.site-search__field {} /* Element */
.site-search--full {} /* Modifier */
```

By reading some HTML with some classes in, you can see how – if at all – the chunks are related; something might just be a component, something might be a child, or element, of that component, and something might be a variation or modifier of that component. To use an analogy/model, think how the following things and elements are related:

```css
.person {}
.person__hand {}
.person--female {}
.person--female__hand {}
.person__hand--left {}
```

Taking the previous .site-search example again, with ‘regular’ naming:

```html
<form class="site-search full">
    <input type="text" class="field">
    <input type="Submit" value ="Search" class="button">
</form>
```

These classes are fairly loose, and don’t tell us much. Even though we can work it out, they’re very inexplicit. With BEM notation we would now have:

```html
<form class="site-search site-search--full">
    <input type="text" class="site-search__field">
    <input type="Submit" value ="Search" class="site-search__button">
</form>
```

You can abbreviate class names while you are using it in subsequent elements.

*Example*

```html
<div class="topbar">
    <div class="topbar__context">
        <div class="topbar__ctx__container">
            <div class="topbar__ctx__ctnr">
                ...
            </div>
        </div>
    </div>
</div>
```

here ```context``` is abbreviated to ```ctx``` and ```container``` is abbreviated to ```ctnr```. The class should be abbreviated only in its subsequent use.

Please do not abbreviate words like ```icon``` to ```icn```. Use abbreviation only for lengthy words.

>Important Note

>While using BEM in a component, don't abbreviate the component's class. For example ```topbar``` in the above case. A list of acronyms to be used in the CSS have to be specified in STYLE_ACRONYMS.md file


##CSS Formatting

* Use soft tabs (4 spaces) for indentation
* Use dashes instead of camelCasing in class names.
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations
* Write your CSS in alphabetical order

*Bad*

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
```

*Good*

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

##CSS Comments

* Prefer comments on their own line. Avoid end-of-line comments.

*Example*

```css
/* ==========================================================================
   Section comment block
   ========================================================================== */

/* Sub-section comment block
   ========================================================================== */

/**
 * Short description using Doxygen-style comment format
 *
 * The first sentence of the long description starts here and continues on this
 * line for a while finally concluding here at the end of this paragraph.
 *
 * The long description is ideal for more detailed explanations and
 * documentation. It can include example HTML, URLs, or any other information
 * that is deemed necessary or useful.
 *
 * @tag This is a tag named 'tag'
 *
 * TODO: This is a todo statement that describes an atomic task to be completed
 *   at a later date. It wraps after 80 characters and following lines are
 *   indented by 2 spaces.
 */

/* Basic comment */
```

## Sass

##### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your `@extend`, regular CSS and `@include` declarations logically (see below)

##### Ordering of property declarations

1. `@extend` declarations

    Just as in other OOP languages, it's helpful to know right away that this “class” inherits from another.

    ```css
    .btn-green {
        @extend %btn;
        // ...
    }
    ```

2. Property declarations

    Now list all standard property declarations, anything that isn't an `@extend`, `@include`, or a nested selector.

    ```css
    .btn-green {
        @extend %btn;
        background: green;
        font-weight: bold;
        // ...
    }
    ```

3. `@include` declarations

    Grouping `@include`s at the end makes it easier to read the entire selector, and it also visually separates them from `@extend`s.

    ```css
    .btn-green {
        @extend %btn;
        background: green;
        font-weight: bold;
        @include transition(background 0.5s ease);
        // ...
    }
    ```

4. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```css
    .btn {
        @extend %btn;
        background: green;
        font-weight: bold;
        @include transition(background 0.5s ease);

        .btn__icon {
            margin-right: 10px;
        }
    }
    ```

    >Do not nest selectors more than three levels deep!

    >```css
    >.page-container {
    >    .content {
    >        .profile {
    >          /* STOP! */
    >        }
    >    }
    >}
    >```

    >When selectors become this long, you're likely writing CSS that is:

    >* Strongly coupled to the HTML (fragile) *—OR—*
    >* Overly specific (powerful) *—OR—*
    >* Not reusable



    > Never nest ID selectors!

    > If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should *never* need to do this.


##### Mixins

Mixins, defined via `@mixin` and called with `@include`, should be used sparingly and only when function arguments are necessary. A mixin without function arguments (i.e. `@mixin hidden { display: none; }`) is better accomplished using a placeholder selector (see below) in order to prevent code duplication.

##### Placeholders

Placeholders in Sass, defined via `%selector` and used with `@extend`, are a way of defining rule declarations that aren't automatically output in your compiled stylesheet. Instead, other selectors “inherit” from the placeholder, and the relevant selectors are copied to the point in the stylesheet where the placeholder is defined. This is best illustrated with the example below.

Placeholders are powerful but easy to abuse, especially when combined with nested selectors. **As a rule of thumb, avoid creating placeholders with nested rule declarations, or calling `@extend` inside nested selectors.** Placeholders are great for simple inheritance, but can easily result in the accidental creation of additional selectors without paying close attention to how and where they are used.

*Sass*

```sass
/* Unless we call `@extend %icon` these properties won't be compiled! */
%icon {
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
}

.icon-error {
    @extend %icon;
    color: red;
}

.icon-success {
    @extend %icon;
    color: green;
}
```

*CSS*

```css
.icon-error,
.icon-success {
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
}

.icon-error {
    color: red;
}

.icon-success {
    color: green;
}
```
