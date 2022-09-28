# Finch Typography Module

The 'typography' module in Finch is designed to give you the tools needed to rapidly work with fonts in your design system. It aids in creating, editing and using a pre-defined suite of fonts.

## Module configuration

There are four variables that are declared as `!default` and can therefore be overridden in any stylesheet that consumes the module with the `@use` or `@forward` rules.

1. **$font-families:** Finch declares a map of font-families, with simple, descriptive names ('sans-serif', 'cursive') etc. This map can be extended, or overridden by supplying your own 'font-families' map. Any keys that clash with Finch's defaults will prefer your custom value.
2. **$fonts:** Finch declares a map of fonts. This is different to font families as it now uses the full 'font' shorthand syntax, declaring the family, size, line height, style, weight and variant.
3. **$utils:** The user can provide a map of key-value pairs in which the key is a property name and the value is a boolean. All properties are set as `false` by default. Any property that is declared as `true` in this map will have a set of utility classes generated for it.
4. **$responsive-utils:** This works exactly like the `$utils` map, except that in this instance, any property that is declared `true` will have additional utility classes for each breakpoint added.

## Module functions and mixins

The typography module exposes a number of functions and mixins that can be used to work with fonts in a Sass stylesheet. These are detailed below:

### Use

The 'use' function is a consistent API for applying a particular font declaration.

```scss
@use 'finch/typography' as typ;

h1, h2, h3 {
  font: typ.use(titles);
}

p,
input,
textarea,
select {
  font: typ.use(body);
}

a,
button,
input[type="submit"],
*[role="button"] {
  font: typ.use(links);
}
```
