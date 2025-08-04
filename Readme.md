Subject:

Sass: Syntactically Awesome Stylesheets
is a CSS preprocessor that adds features like variables, nesting, mixins, functions, 
and loops to make CSS more efficient and maintainable


One feature of Sass that's different than CSS is it uses variables. 
They are declared and set to store data, similar to JavaScript.

In JavaScript, variables are defined using the let and const keywords. 
In **Sass**, variables start with a $ followed by the variable name.

Example Code:
```
    $main-fonts: Arial, sans-serif;
    $headings-color: green;
```
And to use the variables:
Example Code:
```
    h1 {
        font-family: $main-fonts;
        color: $headings-color;
    }
```
One example where variables are useful is when a number of elements need to be the same color. 
If that color is changed, the only place to edit the code is the variable value.


Sass allows **nesting** of CSS rules, which is a useful way of organizing a style sheet.
Normally, each element is targeted on a different line to style it, like so:
Example Code:
```
    article {
        height: 200px;
    }

    article p {
        color: white;
    }

    article ul {
        color: blue;
    }
```
For a large project, the CSS file will have many lines and rules. 
This is where nesting can help organize your code by placing child style rules within the respective parent elements:
Example Code:
```
    article {
        height: 200px;

        p {
            color: white;
        }

        ul {
            color: blue;
        }
    }
```


In Sass, a **mixin** is a group of CSS declarations that can be reused throughout the style sheet. 
The definition starts with the **@mixin** at-rule, followed by a custom name. 
You apply the mixin using the **@include** at-rule.
Example Code:
```
    @mixin reset-list {
        margin: 0;
        padding: 0;
        list-style: none;
    }

    nav ul {
        @include reset-list;
    }
```
Compiles to:
Example Code:
```
    nav ul {
        margin: 0;
        padding: 0;
        list-style: none;
    }
```

Your mixins can also take arguments, which allows their behavior to be customized. The arguments are required when using the mixin.
Example Code:
```
    @mixin prose($font-size, $spacing) {
        font-size: $font-size;
        margin: 0;
        margin-block-end: $spacing;
    }

    p {
        @include prose(1.25rem, 1rem);
    }

    h2 {
        @include prose(2.4rem, 1.5rem);
    }
```
You can make arguments optional by giving the parameters default values.
Example Code:
```
    @mixin text-color($color: black) {
        color: $color;
    }

    p {
        @include text-color(); /* color: black */
    }

    nav a {
        @include text-color(orange);
    }
```


The **@if** directive in Sass is useful to test for a specific case - 
it works just like the if statement in JavaScript.
Example Code:
```
    @mixin make-bold($bool) {
        @if $bool == true {
            font-weight: bold;
        }
    }
```
And just like in JavaScript, the **@else if** and **@else** directives test for more conditions:
Example Code:
```
    @mixin text-effect($val) {
        @if $val == danger {
            color: red;
        }
        @else if $val == alert {
            color: yellow;
        }
        @else if $val == success {
            color: green;
        }
        @else {
            color: black;
        }
    }
```


The **@for** directive adds styles in a loop, very similar to a for loop in JavaScript.

@for is used in two ways: "start through end" or "start to end". 
The main difference is that the **"start to end"** excludes the end number as part of the count, 
and **"start through end"** includes the end number as part of the count.

Here's a start through end example:
Example Code:
```
    @for $i from 1 through 12 {
        .col-#{$i} { width: 100%/12 * $i; }
    }
```
The #{$i} part is the syntax to combine a variable (i) with text to make a string. When the Sass file is converted to CSS, it looks like this:
Example Code:
```
    .col-1 {
        width: 8.33333%;
    }

    .col-2 {
        width: 16.66667%;
    }

    ...

    .col-12 {
        width: 100%;
    }
```


Sass also offers the **@each** directive which loops over each item in a **list** or **map**. 
On each iteration, the variable gets assigned to the current value from the list
Example Code:
```
    @each $color in blue, red, green {
        .#{$color}-text {color: $color;}
    }
```

A **map** has slightly different syntax. Here's an example:
Example Code:
```
    $colors: (color1: blue, color2: red, color3: green);

        @each $key, $color in $colors {
            .#{$color}-text {color: $color;}
        }
```
Note that the $key variable is needed to reference the keys in the map. 
Otherwise, the compiled CSS would have color1, color2... in it. 

Both of the above code examples are converted into the following CSS:
Example Code:
```
    .blue-text {
        color: blue;
    }

    .red-text {
        color: red;
    }

    .green-text {
        color: green;
    }
```


The **@while** directive is an option with similar functionality to the JavaScript while loop. 
It creates CSS rules until a condition is met.

The @for challenge gave an example to create a simple grid system. This can also work with @while.
Example Code:
```
    $x: 1;
    @while $x < 13 {
        .col-#{$x} { width: 100%/12 * $x;}
        $x: $x + 1;
    }
```
First, define a variable $x and set it to 1. 
Next, use the @while directive to create the grid system while $x is less than 13.
After setting the CSS rule for width, $x is incremented by 1 to avoid an infinite loop.


**Partials** in Sass are separate files that hold segments of CSS code. 
These are imported and used in other Sass files. 
This is a great way to group similar code into a module to keep it organized.

Names for partials start with the underscore (_) character, which tells Sass it is a small segment of CSS and not to convert it into a CSS file. 
Also, Sass files end with the .scss file extension. 
To bring the code in the partial into another Sass file, use the @import directive.

For example, if all your mixins are saved in a partial named "_mixins.scss", and they are needed in the "main.scss" file, this is how to use them in the main file:
Example Code:
```
    @import 'mixins'
```
Note that the underscore and file extension are not needed in the import statement - Sass understands it is a partial. 
Once a partial is imported into a file, all variables, mixins, and other code are available to use.


Sass has a feature called **extend** that makes it easy to borrow the CSS rules from one element and build upon them in another.

For example, the below block of CSS rules style a .panel class. 
It has a background-color, height and border.
Example Code:
```
    .panel{
        background-color: red;
        height: 70px;
        border: 2px solid green;
    }
```
Now you want another panel called .big-panel. 
It has the same base properties as .panel, but also needs a width and font-size. 
It's possible to copy and paste the initial CSS rules from .panel, but the code becomes repetitive as you add more types of panels. 
The extend directive is a simple way to reuse the rules written for one element, then add more for another:
Example Code:
```
    .big-panel{
        @extend .panel;
        width: 150px;
        font-size: 2em;
    }
```
The .big-panel will have the same properties as .panel in addition to the new styles.


