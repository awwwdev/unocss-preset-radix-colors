# unocss-preset-radix-ui-colors

> [!WARNING]
> This preset is still in alpha. It is not recommended to use in production.

A preset for UnoCSS to let you use Radix color palette. 


## Installation

```
npm install -D  unocss-preset-radix-ui-colors
```
or with pnpm 
```
pnpm i -D  unocss-preset-radix-ui-colors
```

## Vite/UnoCSS Configuration

```ts
// uno.config.ts (or vite.config.ts)
import { defineConfig, presetUno } from "unocss";
import { presetRadix } from "unocss-preset-radix-ui-colors";

export default defineConfig({
  presets: [
    presetUno(),
    presetRadix({
      aliases: {
        primary: "green",
        base: "slate",
      },
    }),
  ],
});
```

## Usage

You will now have access to colors from your palette, like:

```html
<p class="text-red9">Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
```

Which will render as:

<p class="text-red9">
	Lorem ipsum dolor sit amet consectetur adipisicing elit.
</p>

The colors automatically support dark mode, so you can use:

```html
<div class="p-4 bg-pink3 text-pink12">Text on gray background</div>
```

Which will render as:

<div class="p-4 bg-pink3 text-pink12">Text on gray background</div>

You can switch the docs theme in the ... menu in the top right corner of the page.

> [!NOTE]
> You don't need to add @dark:bg-red9 or anything for dark mode. When `darkSelector` is applied to a scope colors are switched to dark mode.

## Usage in CSS  (Adding Radix colors as CSS Variables)

You can use css variables like `var(--un-preset-radix-red9)`, `var(--un-preset-radix-red9 , red)` in your project and it adds the corresponding colors.

For example:

```html
<div class="p-4" style="border-color: var(--un-preset-radix-blue5A); color: var(--un-preset-radix-gray12, 'darkgray')">
  Text on gray background
</div>
```

Which will render as:

<div class='p-4'
  style="background: var(--un-preset-radix-blue5A); color: var(--un-preset-radix-sky11, 'skyblue')"
  >
  Text on gray background
</div>

> [!NOTE]
> The preset removes the space after `var(`, the trailing space, the trailing comma or the closing bracket. So all of these works:

> - var(--un-preset-radix-red9)
> - var(--un-preset-radix-red9 )
> - var( --un-preset-radix-red9)
> - var( --un-preset-radix-red9 )
> - var(--un-preset-radix-red9,red)
> - var(--un-preset-radix-red9 ,red)
> - var( --un-preset-radix-red9, red)
> - var( --un-preset-radix-red9,red)
> - var(--un-preset-radix-red9,red)
> - var(--un-preset-radix-red9,var(--...))

If you use this in CSS files, make sure UnoCSS watches these CSS files.

If you change `prefix` option, you will need to change the CSS variables as well. For example, if you set prefix to `my-prefix`, you will need to change the CSS variables to `var(--my-prefix-red9)`, `var(--my-prefix-red9 , red)`.


## Alias Utility

You can reset an alias to a different hue by using the utility `alias-{aliasName}-{hue}` like `alias-accent-violet`.

With any scale included in your palette, you can reset aliases to a new hue by using the utility `alias-{aliasName}-{hue}` like `alias-accent-violet`. This sets a series of CSS variables that allow usage of `danger` in place of a color. For example:

```html
<button class="px-4 py-2 rd-2 alias-accent-violet bg-accent4">Button</button>
```

Which renders as:

<button class="px-4 py-2 rd-2 alias-accent-violet bg-accent4 bg-my-color">Button</button>

> [!WARNING]
> Note alias name must be one of aliases defined in [`aliases` option](/v2/configuration#aliases), otherwise it won't work.

You can reset an alias name multiple times. For example:

```html
<div>
  <div class="alias-accent-red bg-accent4">
    <div class="bg-accent4">This has a red background</div>
    <div class="alias-accent-pink">
      <div class="bg-accent4">This has a pink background</div>
    </div>
  </div>
  <div class="alias-accent-violet">
    <div class="bg-accent4">This has a violet background</div>
  </div>
</div>
```

Also, using this utility, you can reset an alias set in [`aliases` option in configuration](/v3/configuration#aliases) to a different hue.

If you need to safelist specific steps or all steps of an alias, use safelistAliases option in config.


## Advanced Configuration

```ts
// uno.config.ts (or vite.config.ts)
import { defineConfig, presetUno } from "unocss";
import { presetRadix } from "unocss-preset-radix-ui-colors";

export default defineConfig({
  presets: [
    presetUno(),
    presetRadix({
      aliases: {
        primary: "green",
        base: "slate",
      },
      safelist: [
        "amber" /* this adds amber1, amber2, ..., amber12 and 
        amber1A, amber2A, ..., amber12A 
        whether they are used in your project or not. 
        This is useful when you add classes on runtime
        (ex from user input or over network). */,
        "blue4", // only adds blue4 whether it is used in your project or not.
        "green3A", // only adds green4A.
        "white7A", // only adds white7A.
        "primary" /* adds primary1, primary2, ..., primary12
          and primary1A, primary2A, ..., primary12A 
          whether they are used in your project or not. */,
      ],
      prefix: "my-prefix" /* CSS variables will 
      be generated with this prefix  */,
      darkSelector: ".dark-theme" /* CSS variables for dark colors 
      will be applied to this selector */,
      lightSelector: ":root .light-theme" /* CSS variables for light colors 
      will be applied to this selector */,
      useP3Colors: true, // use P3 colors with sRGB fallbacks
      extend: true, // extends the defaults theme instead of overriding it
      onlyOneTheme: "dark" /* if your project has only dark theme, 
      set it here so CSS variables for other theme is not added to CSS.
      If this option is present, darkSelector and lightSelector will be ignored 
      and all CSS variables will be added to the :root selector. */,
    }),
  ],
});
```


### `prefix`

The prefix used for the CSS variables generated by the preset.

Default is `--un-preset-radix`.

### `darkSelector`

The selector used for dark mode palette.

Default is `.dark-theme`.

### `lightSelector`

The selector used for dark mode palette.

Default is `:root, .light-theme`.

### `useP3Colors`

A boolean that sets whether or not to use the P3 colors.

Default is `false`.

Note when using P3 colors, rgb colors are also added as fallbacks.

### `aliases`

A key value object of color aliases in the format `alias: hue` that allows you to set aliases for the color palette. For example:

`aliases: { brand: 'blue', success: 'green', danger: 'tomato' }`

- Alias name must contain only lowercases letters and hyphens (`-`).
- You cannot set aliases to `black` or `white`.
- You cannot set aliases to other aliases.
- The alias name can not be a Radix Hue names, `black`, or `white`.
- Alpha steps and `fg` are generated automatically, so if you have alias `brand: blue`, you can use `bg-brand2A` and `c-brand-fg`.

### `safelist`

An array of colors or aliases you want to preserve. This is useful when you might have classes added to your project on runtime (ex from user input or over network).

example: `safelist: ['blue4', 'red5A', 'amber-fg', 'green', 'danger4A', 'base-fg', 'success']`

You can preserve an specific steps of a hue. Like `blue4`, `red5A`, `amber-fg`, etc.
You can preserve an all steps (12 steps + 12 alpha steps + `fg` step) of a hue. Like `blue`, `red`, `amber`, etc.
You can preserve an specific steps of an alias. Like `success4`, `warning5A`, `base-fg`, etc.
You can preserve an all steps of an alias (12 steps + 12 alpha steps + `fg` step). Like `success`, `warning`, `base`, etc.

::: warning  
Any safelist alias must be defined in aliases option, otherwise it will be ignored.
:::

Dark mode and alpha variants are automatically.

### `onlyOneTheme`

If you only want to use `dark` or `light` theme, set this to `light` or `dark` respectively. So, CSS variables for other theme is not added to CSS.

Default is `undefined`.

When onlyOneTheme is set to `dark` or `light`, the darkSelector and lightSelector will be ignored and all CSS variables will be added to the `:root` selector.

### `layer`

The name of unocss layer that CSS variables are added to.

Default is `undefined`.

### `extend`

A boolean that sets whether or not the preset will completely overwrite or merge with the previous palette.

Default is `false`.


