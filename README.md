# Sass - A language extension for css

Provides features that are missing in css

## Sass, variables start with a $ followed by the variable name

- Example-
Using in css files

// declaring vars
$main-fonts: Arial, sans-serif;
$headings-color: green;

// using them
h1 {
  font-family: $main-fonts;
  color: $headings-color;
}

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

[^vp]: Newer CSS features take time before they are fully adopted and ready to use in all browsers. As features are added to browsers, CSS rules for new features may need vendor prefixes. Consider box-shadow(feature) shown in above snippet
