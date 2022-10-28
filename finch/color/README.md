# Color

The color module is designed to make it as easy as possible to work with colors in your design system. The module generates color variations for you, provides tools like contrast-checking and colorspace conversion, and gives you a consistent, intuitive DX for consuming different colors in your project code.

## Defaults

By default, the color module comprises the following colors *(note that for the shades of grey, both 'grey' and 'gray' spellings will work)*:

- **Red:** #B1021C rgb(177, 2, 28)
- **Blue:** #3CABEC rgb(60, 171, 236)
- **Green:** #425E21 rgb(66, 94, 33)
- **Yellow:** #F7C326 rgb(247, 195, 38)
- **Pink:** #E071AC rgb(224, 113, 172)
- **Purple:** #8213BE rgb(130, 19, 190)
- **Orange:** #FF670F rgb(255, 103, 15)
- **Indigo:** #8794DE rgb(135, 148, 222)
- **Violet:** #7500EB rgb(117, 0, 235)
- **Light:** #FBF7FC rgb(251, 247, 252)
- **Light grey:** #EFEFEF rgb(239, 239, 239)
- **Dark grey:** #BEBEBE rgb(190, 190, 190)
- **Dark:** #212121 rgb(33, 33, 33)

The above swatches represent the collection of "base" colors. Finch also defines some "contextual" colors by giving these base shades descriptive names. Of course, all of this is configurable.

- **Error:** see 'Red'
- **Invalid:** see 'Red'
- **Warning:** see 'Yellow'
- **Info:** see 'Blue'
- **Alert:** see 'Orange'
- **Success:** see 'Green'
- **Valid:** see 'Green'
- **Disabled:** see 'Dark grey'

## Variants and shades

Finch will produce a number of variations to its base colors (and any custom colors that you pass in). These fall into one of two categories: 'shades' and 'variants'.

### Shades

Shades are variations of the base color that adjust the color's 'lightness' in both the positive and negative directions. These shades are represented by integer values from 0 - 1000, in increments of 100 (e.g. 0, 100, 200...900, 1000). The base color itself is placed on this scale based on its 'lightness' value. For example, a color with lightness of '44%' would be placed at key 400 in the shades map, but would also be available at the key 'base'.

### Variants

Variants adjust the base color in a variety of other ways (saturation, alpha etc.). A full list of variants and their respective adjustments is as follows:

- **dark:** lightness adjusted by -20%
- **light:** lightness adjusted by +30%
- **transparent:** alpha adjusted by -80%
- **translucent:** alpha adjusted by -25%
- **grayscale:** a shade of grey with the same lightness value as the base color
- **dull:** saturation adjusted by -30%
- **vivid:** saturation adjusted by +20%