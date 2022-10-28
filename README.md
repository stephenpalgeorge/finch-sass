# Finch Sass

Finch is a minimal, but opinionated framework for writing design-systems that can scale. Finch is 2 things:

1. A tool-belt of Sass mixins, functions and variables that create a consistent API for using colors, fonts and layout primitives.
2. An intelligent, configurable utility class generator

It works out of the box, but _everything_ is customisable and configurable to suit your design system and tokens. Finch is broken up into a series of modules that extend the core Sass functionality:

1. finch/color
2. finch/layout
3. finch/typography
4. finch/utils

There is also a 'configuration' module that houses the core definitions that Finch needs to work, and that are consumed across the other modules. Each module's directory contains a more detailed markdown file that gives a full specification of what's included in the module and how it all works. Here follows a more terse overview.

## Getting started

Install Finch as a dev-dependency. It's also recommended (strongly!) that you add the path to the root `finch-sass` dir in your `node_modules` to whatever sass compilation tool you're using. This will differ from tool to tool, but we'll show the Sass CLI and gulp-sass versions as examples.

```bash
npm i -D finch-sass
```

If you're compiling your Sass with the Sass CLI directly, then make sure your compilation script includes the `--load-path` option, pointing to `finch-sass`, e.g.

```json
{
  "build": "sass src:build --load-path=node_modules/finch-sass"
}
```

Adding this path will mean that Sass will check that directory when looking for stylesheets to load, meaning within your `.scss` documents you can simply write this:

```scss
@use 'finch/color';
// no need for the ugliness of stuff like this anymore:
// @use '../../../node_modules/finch-sass/finch/color';
```

If you're using [gulp-sass](https://www.npmjs.com/package/gulp-sass), then include this path in the `includePaths` option, e.g.:

```javascript
function buildStyles() {
  return gulp.src('./sass/**/*.scss')
    .pipe(sass({
        includePaths: ['node_modules/finch-sass']
    }).on('error', sass.logError))
    .pipe(gulp.dest('./css'));
};
```

## Configuration

One of the core concepts behind Finch is configurable modules. It's recommended you have one root stylesheet where you bring in and configure your modules, so they can then be used across the rest of your source code.

```scss
// styles.scss

@use 'finch/color' as clr with (
  $colors: (brand-main: #f00, brand-secondary: #ff0),
  $utils: (background-color)
);

@use 'finch/typography' as typ with (
  $font-families: ($serif: "Georgia, serif")
);
```

Review each module's README for full details of exactly what can be configured and how your custom values are used. In this case, we configure the color module by supplying 2 of our own brand colors, which will be merged into a map with all the default Finch colors. We also declare that we want utility classes for the `background-color` property - this means, in our markup, we'll be able to do something like this:

```html
<div class="bg:brand-main:base">
    This div has a red background!
</div>
```

We also configured the typography module to use 'Georgia' as our 'serif' font. There is, on top of this, a `config` module, which should be used before any of the other modules. The `config` module contains the core definitions for things like the `modular-scale` and `breakpoints` and `prop-labels` that are used by the other modules. The config module's own README file details all the variables that you can pass in to set things up the way you like.

## finch/color

Finch's color module provides a collection of functions and mixins for a consistent API when using colors in your stylesheets. Finch also has some opinions about how you use color and builds out some variants for you. Full details can be found in `finch/color/README.md`, but at the core of the module are the `use` function and mixin.

### The `use` function

The color `use` function provides a consistent way of accessing any variant of any color from the design system. All of the below examples are valid in Finch, and work out-of-the-box:

```scss
@use 'finch/color' as clr;

h1 {
  color: clr.use(blue);
}

h2 {
  color: clr.use(blue, vivid);
}

a {
  color: clr.use(red, 400);
  &:hover {
    color: clr.use(red, 200);
  }
}
```

### The `use` mixin

The color `use` mixin consumes the `use` function and applies the value to specified properties, as well as setting hover and focus states by default.

```scss
@use 'finch/color' as clr;

a {
  @include clr.use(red);
}
```

In the above example, Finch reads the value of `red` and determines if it's a 'light' or 'dark' color. If it's light, then it will automatically apply the 'dark' variant of the color to the hover and focus states of the `a` elements, or vice versa. It's also possible to target properties other than `color`, e.g.

```scss
@use 'finch/color' as clr;

a {
  @include clr.use(red, dark, $target: background-color);
}

section.frame {
  border: 2px solid;
  @include clr.use(dark, $target: (color, border-color));
}
```

The color module also exposes a `theme` mixin and a number of other useful functions and mixins for working with color.

## finch/layout

The Finch layout module provides a collection of functions and mixins for working with sizing and arranging elements on a page. There is a `use` function, as with every Finch module, and a number of other 'layout primitive' mixins.

### The `use` function

The layout module's `use` function applies a size from one of two modular scales that are at the core of Finch. See `finch/config/README.md` for full details about the modular scales, but the following examples show typical use-cases for the function.

```scss
@use 'finch/layout' as lyt;

h1 + * {
  margin-top: lyt.use(900);
}

:is(h2, h3) + * {
  margin-top: lyt.use(700);
}

input, button {
  border-width: lyt.use(400, $detail: true);
}
```

## finch/typography

The Finch typography module provides a collection of functions and mixins for working with fonts and all font-related properties. As with all of Finch's modules, there is a `use` function, which applies one of a pre-defined set of fonts. N.B. that the `font` shorthand property is used, so a font declaration represents font-family, weight, size, line-height etc. As well as Finch's default fonts, which you can override, you can also add your own, either by passing a valid string for the `font` CSS property, or by using Finch's own `gen-font` function, which provides some defaults.

```scss
@use 'finch/typography' as typ;

h1 {
  font: typ.use(titles);
}

h2, h3, h4 {
  font: typ.use(subtitles);
}

p {
  font: typ.use(body);
}
```

## finch/utils
@todo

## Utility classes

Finch is also a utility-class generator that gives you granular control over which classes you want it to create. Every module allows you to pass in a list of `$utils` and `$responsive-utils`, which are property names appropriate to that module, for which utility classes will be generated. For example, using the color module as such:

```scss
@use 'finch/color' with ($utils: (color, border-color));
```

Would make available in your markup utility classes such as `color:violet`, `color:blue:vivid`, `color:purple:700`, `border-color:dark:transparent` etc. etc.
