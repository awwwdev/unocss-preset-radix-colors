# unocss-preset-radix

## Installation

```
pnpm add -D unocss-preset-radix
```

## Usage

```ts
import { defineConfig, presetUno } from "unocss";
import { presetRadix } from "unocss-preset-radix";

export default defineConfig({
  presets: [
    presetUno(),
    presetRadix({
      palette: ["blue", "green", "red"],
      aliases: {
        primary: "green",
        base: "slate",
      },
    }),
  ],
});
```

## Alphas

Alphas are available as extra steps in each scale. For example `bg-blue5` for solid, `bg-blue5A` for alpha.

## Foregrounds

The optimized foreground colors are available as `-fg` steps. For example `text-blue-fg` for white `text-amber-fg` for white. These colors are based on [the Radix docs](https://www.radix-ui.com/colors/docs/palette-composition/composing-a-palette#choosing-a-brand-scale). This also works with hues and aliases.

## Options

### palette

An array of the Radix UI Colors you'd like to include. Dark mode and alpha variants are automatic. Overlay colors are added by default.

### prefix

The prefix used for the CSS variables generated by the preset. Default is `--un-preset-radix`.

### darkSelector

The selector used for dark mode palette. Default is `.dark-theme`.

### lightSelector

The selector used for dark mode palette. Default is `:root, .light-theme`.

### aliases

A key value object of color aliases in the format `alias: target` that allows you to set aliases for the color palette. You cannot set aliases to other aliases. Alpha variants for aliases are generated automatically, so for then given alias `brand: blue`, an alias `brandA: blueA` will also be generated.

### extend

A boolean that sets whether or not the preset will completely overwrite or merge with the previous palette. Default is `true`.

## `hue`

With any scale included in your palette, you can use the utility `hue-{scale}` like `hue-red` or `hue-sand`. This sets a series of CSS variables that allow usage of `hue` in place of a color. For example:

```html
<div class="hue-red bg-hue4" />
<div class="bg-red4" />
```

are equivilant.

This allows you to add all the `hues` you'd like to use to your safelist, and be able to dynamically choose the colorway of an element using JS or whatever your poison of choice is.

This feature is heavily inspired by [Imba's `hue`](https://imba.io/docs/css/properties/hue)