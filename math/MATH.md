# Finch Math Module

The math module in finch exposes some useful functions that extend the native mathematical capabilities of Sass.

## Module functions

### To Radix

The `to-radix` function is used to convert a decimal number into another base. This is used internally within Finch to aid in the conversion of colors from rgb, hsl, to hex. It is exposed here in case you also have a use for it.

```scss
@use 'finch/math' as mth;

$hexFull: mth.to-radix(255, $target: 16); // ff
```

### Mod

The `mod` function is an implementation of the mathematical 'modulo' operator (normally written with the `%` sign). The native, built-in, modulo operator should almost always be preferred. This implementation, however, was written to serve a particular use case in the Finch's internals as it returns a map containing both the quotient (the whole integer part of the division) and the remainder (the number usually returned from a modulo operation).

```scss
@use 'finch/math' as mth;

$output: mth.mod(20, 8); // (quotient: 2, remainder: 4)
```
