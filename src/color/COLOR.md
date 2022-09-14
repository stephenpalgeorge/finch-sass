# Finch Color Module

[//]: # (@todo - write 'use' mixin documentation)

The 'color' module in Finch provides a set of useful functions for working with color in Sass. It aids in creating, editing and using a palette of predefined brand colors.

## Module configuration

There are 3 different variables that the user can configure the module with. These are declared as `!default` in the module and therefore can be overridden when the module is consumed with the `@use` or `@forward` rules.

1. **$colors:** Finch declares a map of colours that are used as a fallback, but obviously developers will want to override some/all the keys in this map, or extend it by adding their own. Users can declare their own map of colors, which will then be merged with Finch's defaults, with the user's colors taking priority when there are naming clashes.
2. **$utils:** The user can provide a map of key-value pairs in which the key is a property name and the value is a boolean. All properties are set as `false` by default. Any property that is declared as `true` in this map will have a set of utility classes generated for it.
3. **$responsive-utils:** This works exactly like the `$utils` map, except that in this instance, any property that is declared `true` will have additional utility classes for each breakpoint added.

## Module functions && mixins

The color module exposes a number of functions that can be used to work with color palettes in a Sass stylesheet. These are each detailed below:

### Use

The 'use' function is a consistent API for applying any color from your palette, and adjusting the shade of that color as required. It accepts 2 parameters: a color name, and an optional "shade" value, which must be an integer between 0 - 1000, and a multiple of 100 (300, 400, 500...etc.).

```scss
@use 'finch/color' as clr with (
  $colors: (red: '#ff0800', blue: '#0022ff'),
);

h1 {
  // h1 color will be #0022ff
  color: clr.use(blue);
}

p.highlighted {
  // .highlighted color will be #7500EB (the Finch default value for violet)
  color: clr.use(violet);
}

a {
  // `a` color will be a very light shade of blue
  color: clr.use(blue, 800);
}
```

The 'use' mixin is a wrapper for the use function that provides some additional behaviour. As well as setting the specified colour against the `$target` property, it also establishes some `hover` and `focus` styles - if the colour is considered 'dark', it is lightened by 10%, or conversely, if the colour is considered 'light', it is darkened by 10%.

### Complementary

The 'complementary' function generates a 2-color palette with a `base` color and a `complement`. The function accepts 2 arguments - a color name from which the other color in the palette will be calculated, and a boolean `$shades` argument, which determines if the returned map should just contain individual color values, or a map of shades for each color.

```scss
@use 'sass:map';
@use 'finch/color';

$brand-main: color.use(purple);

$palette: color.complementary($brand-main);
// `$palette` will be a map of 2 colors: base: #8213BE, complement: #4FBE13.

$more-colors: color.complementary($brand-main);
// `$more-colors` will be a map of 2 maps - one for each color - each containing shades from 0 - 1000. You could then access any of these colors like so:
$brand-accent: map.get($more-colors, complement, 700);
```

The following function definitions are a variation on the above, so I won't provide detailed examples for each.

### Analogous

The 'analogous' function generates a 3-color palette with a `base` color, a `secondary` and a `tertiary`. The function, like the `complementary` function, accepts a color name, a `$shades` boolean and then an optional 3rd argument: `$dir`, which defaults to `clockwise` - other acceptable values are `anti-clockwise` or `anticlockwise`. The `$dir` defines whether the secondary and tertiary colors should be calculated in positive 30deg hue increments, or negative 30deg hue increments.

### Triadic

The 'triadic' function generates a 3-color palette with a `base` color, a `secondary` and a `tertiary`. This function differs from `analogous` in that these 3 colors are evenly spaced around the color wheel, rather than being 30deg apart as they were in an `analogous` palette. This function, therefore, has no need for a `$dir` argument and just accepts the color name and `$shades` boolean.

### Tetradic

The 'tetradic' function generates a 4-color palette with `base` color, a `secondary`, `tertiary` and `quartenary`. The function accepts a color name and a `$shades` boolean.

## Utility Classes

If the module configuration (as specified above) sets up the module to generate utility classes, the following classes will be created for every prop that is flagged as true:

```
.<prop-name>:<prop-value>
.<prop-name>:<prop-value>!
```

For example `.color:violet` and `.background-color:blue!`. The class names that end with a `!` will have their value marked as `!important` in the CSS.

If a property is flagged for responsive utility classes in the module configuration, the following class names will be created:

```
.<prop-name>:<prop-value>@<breakpoint>
.<prop-name>:<prop-value>@<breakpoint>!
```

For example `.color:indigo@sm` and `.background-color:orange@lg!`. Again, the `!` signals `!important` styles.
