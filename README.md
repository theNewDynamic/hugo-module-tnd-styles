# TND Styles Hugo Module

This is a Hugo Module set to be used internally by TND but not ready for distribution outside of TND. Use at your own risk.

The module uses Hugo Pipes to process stylesheet files with SASS and/or Tailwindcss. It also make sure self hostesd fonts are loaded properly. 

## Requirements

Requirements:
- Go 1.14
- Hugo 0.64.0


## Installation

If not already, [init](https://gohugo.io/hugo-modules/use-modules/#initialize-a-new-module) your project as Hugo Module:

```
$: hugo mod init {repo_url}
```

Configure your project's module to import this module:

```yaml
# config.yaml
module:
  imports:
  - path: github.com/theNewDynamic/hugo-module-tnd-styles
```



## Configuration

### Registered styles.
Without any configuration, the Module will consider `/assets/css/style.{css|scss}` a lone registered script of name `main`.

In order to add more registrered style, user should reference them (including main) under the `styles` key of the module configuration with the following keys:
__name__: The name of the style. Will be use to call it through the `tnd-styles/tags` partial`
__path__: The `path` relative to the project's assets directory.
__inline__: If set to true, the style will be printed inside a `<style>` tag rather than as a `<link>` request.

```yaml
tnd_styles:
  styles:
    - name: main
      path: css/styles/index.scss
    - name: carousel
      path: css/styles/carousel.css
      inline: true
```

### Tailwind and PostCSS

In order to use Tailwind and/or PostCSS user should 
1. install the following npm deps:
```
npm install postcss-cli postcss-import tailwindcss autoprefixer
```
2. Add a postcss.config.js file at the root of the repo: (tailwind config path should match user's)
```js
module.exports = {
  plugins: [
    require("postcss-import")({
      path: ["assets/css"],
    }),
    require("tailwindcss")("./assets/css/config/tailwind.config.js"),
  ],
};name: 

```

### Fonts

The module automatically generate a <style> tag containing `@fontface` declarations for every declaration set through the module settings on the condition that at least one file matching the base filename set in the declaration exists.

```yaml
tnd_styles:
  fonts:
  - family: Open
    file: fonts/files/open-sans-v17-latin-regular
    weight: 400
    style: normal
  - family: Open
    file: fonts/files/open-sans-v17-latin-italic
    weight: 400
    style: italic
  - family: Open
    file: fonts/files/open-sans-v17-latin-700
    weight: 700
  - family: Open
    file: fonts/files/open-sans-v17-latin-700italic
    weight: 700
    style: italic
```

With the presents of the following files:

```
assets/fonts
└── files
    ├── open-sans-v17-latin-300.eot
    ├── open-sans-v17-latin-300.svg
    ├── open-sans-v17-latin-300.ttf
    ├── open-sans-v17-latin-300.woff
    ├── open-sans-v17-latin-300.woff2
    ├── open-sans-v17-latin-300italic.eot
    ├── open-sans-v17-latin-300italic.svg
    ├── open-sans-v17-latin-300italic.ttf
    ├── open-sans-v17-latin-300italic.woff
    ├── open-sans-v17-latin-300italic.woff2
    ├── open-sans-v17-latin-700.eot
    ├── open-sans-v17-latin-700.svg
    ├── open-sans-v17-latin-700.ttf
    ├── open-sans-v17-latin-700.woff
    ├── open-sans-v17-latin-700.woff2
    ├── open-sans-v17-latin-700italic.eot
    ├── open-sans-v17-latin-700italic.svg
    ├── open-sans-v17-latin-700italic.ttf
    ├── open-sans-v17-latin-700italic.woff
    ├── open-sans-v17-latin-700italic.woff2
    ├── open-sans-v17-latin-italic.eot
    ├── open-sans-v17-latin-italic.svg
    ├── open-sans-v17-latin-italic.ttf
    ├── open-sans-v17-latin-italic.woff
    ├── open-sans-v17-latin-italic.woff2
    ├── open-sans-v17-latin-regular.eot
    ├── open-sans-v17-latin-regular.svg
    ├── open-sans-v17-latin-regular.ttf
    ├── open-sans-v17-latin-regular.woff
    └── open-sans-v17-latin-regular.woff2
```

Will produce the following fontface declarations

```css
@font-face {
  font-family: Open;
  font-style: normal;
  font-weight: 300;
  src: local("Open"),
    url("/fonts/files/open-sans-v17-latin-300.eot") format("embedded-opentype"),
    url("/fonts/files/open-sans-v17-latin-300.svg") format("svg"),
    url("/fonts/files/open-sans-v17-latin-300.ttf") format("truetype"),
    url("/fonts/files/open-sans-v17-latin-300.woff") format("woff"),
    url("/fonts/files/open-sans-v17-latin-300.woff2") format("woff2");
}
@font-face {
  font-family: Open;
  font-style: italic;
  font-weight: 300;
  src: local("Open"),
    url("/fonts/files/open-sans-v17-latin-300italic.eot") format("embedded-opentype"),
    url("/fonts/files/open-sans-v17-latin-300italic.svg") format("svg"),
    url("/fonts/files/open-sans-v17-latin-300italic.ttf") format("truetype"),
    url("/fonts/files/open-sans-v17-latin-300italic.woff") format("woff"),
    url("/fonts/files/open-sans-v17-latin-300italic.woff2") format("woff2");
}
@font-face {
  font-family: Open;
  font-weight: 400;
  src: local("Open"),
    url("/fonts/files/open-sans-v17-latin-regular.eot") format("embedded-opentype"),
    url("/fonts/files/open-sans-v17-latin-regular.svg") format("svg"),
    url("/fonts/files/open-sans-v17-latin-regular.ttf") format("truetype"),
    url("/fonts/files/open-sans-v17-latin-regular.woff") format("woff"),
    url("/fonts/files/open-sans-v17-latin-regular.woff2") format("woff2");
}
@font-face {
  font-family: Open;
  font-style: italic;
  font-weight: 400;
  src: local("Open"),
    url("/fonts/files/open-sans-v17-latin-italic.eot") format("embedded-opentype"),
    url("/fonts/files/open-sans-v17-latin-italic.svg") format("svg"),
    url("/fonts/files/open-sans-v17-latin-italic.ttf") format("truetype"),
    url("/fonts/files/open-sans-v17-latin-italic.woff") format("woff"),
    url("/fonts/files/open-sans-v17-latin-italic.woff2") format("woff2");
}
@font-face {
  font-family: Open;
  font-style: normal;
  font-weight: 700;
  src: local("Open"),
    url("/fonts/files/open-sans-v17-latin-700.eot") format("embedded-opentype"),
    url("/fonts/files/open-sans-v17-latin-700.svg") format("svg"),
    url("/fonts/files/open-sans-v17-latin-700.ttf") format("truetype"),
    url("/fonts/files/open-sans-v17-latin-700.woff") format("woff"),
    url("/fonts/files/open-sans-v17-latin-700.woff2") format("woff2");
}
@font-face {
  font-family: Open;
  font-style: italic;
  font-weight: 700;
  src: local("Open"),
    url("/fonts/files/open-sans-v17-latin-700italic.eot") format("embedded-opentype"),
    url("/fonts/files/open-sans-v17-latin-700italic.svg") format("svg"),
    url("/fonts/files/open-sans-v17-latin-700italic.ttf") format("truetype"),
    url("/fonts/files/open-sans-v17-latin-700italic.woff") format("woff"),
    url("/fonts/files/open-sans-v17-latin-700italic.woff2") format("woff2");
}
```

Accepted style settings are:

- family
- weight
- style
- display
- variant
- feature-settings
- variation-settings

The module also prefetches every declared font files

## Usage

### tnd-styles/tags

The partial should be invoked in your `<head>` and will print all the necessary tags discussed above.

If user need to single out styles from the registered styles, context shoulce be a slice:

```
<head>
[...]
{{ partialCached "tnd-styles/tags.html" (slice "main" "carousel") "main" "carousel" }}
</head>
```

If another type is passed as context, module will print all the registered styles. If no registered styles is found, module will print the main style.

```
<head>
[...]
{{ partialCached "tnd-styles/tags.html" "tags" }}
</head>
```
### tnd-styes/tags only for fonts
If user only needs to use the fonts:
 
```
<head>
[...]
{{ partialCached "tnd-styles/tags.html" (slice "fonts") "fonts" }}
</head>
```
  
## theNewDynamic

This project is maintained and love by [thenewDynamic](https://www.thenewdynamic.com).
