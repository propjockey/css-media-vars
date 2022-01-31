[![JaneOri](https://img.shields.io/badge/JaneOri%20%F0%9F%91%BD-I%20made%20a%20thing!-blueviolet.svg?labelColor=222222)](https://twitter.com/Jane0ri)

# css-media-vars from PropJockey
A brand new way to write responsive CSS. Named breakpoints, DRY selectors, no scripts, no builds, vanilla CSS.

Docs+Demos at: https://propjockey.github.io/css-media-vars/

NPM: https://www.npmjs.com/package/css-media-vars

GitHub: https://github.com/propjockey/css-media-vars

Install:
`$ npm install css-media-vars`
Then include the `/node_modules/css-media-vars/css-media-vars.css` file before any stylesheets that use it.

OR

Use your favorite NPM CDN and include it on your page before other stylesheets for small projects. Like so:
```html
<link rel="stylesheet" type="text/css" href="https://unpkg.com/css-media-vars/css-media-vars.css">
```

## Example of what your mobile-first CSS would look like
```css
  .breakpoints-demo > * {
    --xs-width: var(--media-xs) 100%;
    --sm-width: var(--media-sm) 49%;
    --md-width: var(--media-md) 32%;
    --lg-width: var(--media-gte-lg) 24%;
    width: var(--xs-width, var(--sm-width, var(--md-width, var(--lg-width))));

    --sm-and-down-bg: var(--media-lte-sm) red;
    --md-and-up-bg: var(--media-gte-md) green;
    background: var(--sm-and-down-bg, var(--md-and-up-bg));
  }
```

VS Writing this same basic responsive CSS in the traditional way:
```css
  .breakpoints-demo > * {
    width: 100%;
    background: red;
  }
  @media (min-width: 37.5em) and (max-width: 56.249em) {
    .breakpoints-demo > * {
      width: 49%;
    }
  }
  @media (min-width: 56.25em) and (max-width: 74.99em) {
    .breakpoints-demo > * {
      width: 32%;
    }
  }
  @media (min-width: 56.25em) {
    .breakpoints-demo > * {
      background: green;
    }
  }
  @media (min-width: 75em) {
    .breakpoints-demo > * {
      width: 24%;
    }
  }
```

css-media-vars let you write responsive, vanilla, CSS with named breakpoints, DRY selectors, and easier to mantain code.

## New in v1.1.0
Write CSS that responds to the browser's support of @property from Houdini:
```css
  .warning-at-property-unsupported {
    --display-block-if-unsupported: var(--media-at-property-unsupported) block;
    display: var(--display-block-if-unsupported, none); /* display: none if @property is supported */
  }
  .congrats-at-property-supported {
    --display-none-if-unsupported: var(--media-at-property-unsupported) none;
    display: var(--display-none-if-unsupported, block); /* display: block if @property is supported */
  }
  :root {
    --at-prop-unsupported-color: var(--media-at-property-unsupported) red;
    --at-prop-color: var(--at-prop-unsupported-color, green);
    /* var(--at-prop-color) will return: */
    /* red if @property is unsupported */
    /* green if @property is supported */
  }
```

## Minification
Environments that force minification with PostCSS and cssnano (or other CSS minifiers) may produce invalid CSS by striping the space from a `--custom-var: ;` assignment. To avoid this with **Create React App**, copy `css-media-vars.css` into the public folder and manually include it in the index.html file to avoid the minification until they're fixed.

You can read more about using the public folder here: https://create-react-app.dev/docs/using-the-public-folder/

## Innovative Possibilities

Responsive CSS written with css-media-vars is so DRY with selecotors, you can skip them entirely.

Easily write responsive CSS inline, directly on the style attribute if you wish. No scripting, no @‍media nesting, just vanilla CSS with some hidden [space toggle](https://github.com/propjockey/css-sweeper#css-is-a-programming-language-thanks-to-the-space-toggle-trick) magic.

[![gif of responsive css changing state based on CSS written inline with the style attribute](https://i.imgur.com/3RkpkEn.gif)](https://codepen.io/propjockey/pen/xxPZLBy?editors=1100)

### Hypothetical innovative example

This may be especially useful for enhancing user generated content without the overhead traditional responsive CSS causes. For example, if your platform supports markdown, you could change the `---` hr separator to support user-supplied color variations that are resposive to light/dark mode:

---

Right now,
`---`
becomes
```html
<hr>
```
with global styles like:
```css
hr {
  border: 1px solid #cccccc;
}
```

---

Using css-media-vars to enhance this feature, you could transpile this

`---#ff0000-#880000---`

into this

```html
<hr style="--light: var(--media-prefers-light) #ff0000; --dark: var(--media-prefers-dark) #880000; border-color: var(--light, var(--dark, #cccccc));">
```
with the same global CSS.

---

To do this same progressive enhancement without css-media-vars, the easiest way looks like this:
```html
<hr style="--light: #ff0000; --dark: #880000;">
```
and update the global CSS to this:
```css
hr {
  border: 1px solid #cccccc;
}

@media (prefers-color-scheme: light) {
  hr {
    border-color: var(--light, #cccccc);
  }
}
@media (prefers-color-scheme: dark) {
  hr {
    border-color: var(--dark, #cccccc);
  }
}
```

---

You may not be sold from that single trade-off alone, but imagine you had even just a few more features you wanted to enhance in a similar way.

Your global css becomes large very quickly, and depending on the complexity of your selecotrs (which would be repeated several times), your global CSS begins to lock in your DOM and make future element changes a tech-debt nightmare.

You avoid all of that with css-media-vars. 🎉

## CHANGELOG:

v1.1.0 - June 27th, 2021:
* added `--media-at-property-unsupported` space toggle that detects CSS @property support

v1.0.0 - July 16th, 2020:
* Initial release
