# Sass - A language extension for css

Provides features that are missing in css

___

## Sass

variables start with a $ followed by the variable name

- Example-
Using in css files

```css
// declaring vars
$main-fonts: Arial, sans-serif;
$headings-color: green;

// using them
h1 {
  font-family: $main-fonts;
  color: $headings-color;
}
```

___

## Nesting css rules for better structure

- Instead of selecting and styling parents and children separately, Sass supports nesting to write specific and shared rules easily
  In large projects nesting can help organize your code by placing child style rules within the respective parent elements

```css
nav {
  background-color: red; // style rule specific to nav

  ul {
    list-style: none; // style rule for nav and ul

    li {
      display: inline-block; // style for li and it's parents
    }
  }
}
```

____

## Mixins

Mixins are like functions in css
Reuse a group of css rule declarations, by converting them to a mixin function i.e. declare mixin
> to reuse later, mixins can be called with @include directive, passing the desired values along with parameters i.e. call mixin

- Convert syles you want to reuse into mixins by using the @mixin directive, custom function name and passing the variables needed inside parenthesis

```diff
+ @mixin box-shadow($x, $y, $blur, $c){
+  -webkit-box-shadow: $x $y $blur $c;
+  -moz-box-shadow: $x $y $blur $c;
+  -ms-box-shadow: $x $y $blur $c;
+  box-shadow: $x $y $blur $c;
+ }

div {
-  -webkit-box-shadow: 0px 0px 4px #fff;
-  -moz-box-shadow: 0px 0px 4px #fff;
-  -ms-box-shadow: 0px 0px 4px #fff;
-  box-shadow: 0px 0px 4px #fff;
+  @include box-shadow(0px, 0px, 4px, #fff);
}
```

> include the _vendor prefixes_[^vp] and a _general style_ inside mixin**

____

## Conditions

`@if` `@else` `@else if` directives are available for checking conditions

Example

```css
@mixin text-effect($val) {
  @if $val == danger {
    color: red;
  }
  @else if $val == success {
    color: green;
  }
  @else {
    color: black;
  }
}
```

____

## for loop

2 ways to write in Sass

1. start through end - include iteration for last number

    ```css
    @for $i from 1 through 12 {
      .col-#{$i} { width: 100%/12 * $i; }
    }
    ```

    the #{} syntax is an example of string interpolation. takes the value of variable and combines with the rest of the text, forming a string

    above snippet converted to css will look like

    ```css
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

2. start to end - exclude iteration for last number

____

## each to loop over list or map

loop over a list with @each directive

```css
@each $color in blue, red, green {
  .#{$color}-text {color: $color;}
}
```

to loop over a map, use

```css
$colors: (color1: blue, color2: red, color3: green);

@each $key, $color in $colors {
  .#{$color}-text {color: $color;}
}
```

> $key variable is needed to work correctly

the output looks like

```css
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

___

## while loop

create css rules untill a condition is met with `@while` directive

```css
$x: 1;
@while $x < 13 {
  .col-#{$x} { width: 100%/12 * $x;}
  $x: $x + 1;
}
```

> __Do not forget the exit condition to avoid infinite loop__

___

## split code into modules for reuse

create __partial file__ with `_` character prefix and `.scss` extension
> this indicates to sass that
> 1. this file is a segment of css,
> 2. not to convert it to css

use `@import` to use the code in __partial file__
`@import 'mixins'`
> just use file name without `_` and `.scss`, Sass understands this is a partial

___

## extend styles in one rule and add upon them to create a new rule with less repitition

use `@extend` keyword along with the rule

```css
.panel{
  background-color: red;
  height: 70px;
  border: 2px solid green;
}

.big-panel{
  @extend .panel; // styles from '.panel' rule reused
  width: 150px;
  font-size: 2em;
}
```

`.big-panel` has properties of `.panel` and more

[^vp]: Newer CSS features take time before they are fully adopted and ready to use in all browsers. As features are added to browsers, CSS rules for new features may need vendor prefixes. Consider box-shadow(feature) shown in above snippet
