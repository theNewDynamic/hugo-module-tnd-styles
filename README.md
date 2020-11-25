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

## Usage

User should respect a file structure in its asset directory:

```
assets
├── css
│   ├── style.css
│   └── tailwindcss
│       ├── modules
│       │   ├── _hovers.css
│       │   ├── _buttons.css
│       │   └── _cards.css
│       └── tailwind.config.js
```

Module will look for an `assets/css/style.css` or `assets/css/style.scss`. 

If the project's main style asset lives elsewhere, user can use the `styles` settings to register any number of files which will be processed and loaded by the Module.

```yaml
tnd_styles:
  styles:
    - path: css/styles/index.scss
    - path: css/styles/home-hero.css
      type: critical
```

### Tailwind and PostCSS

In order to use Tailwind and/or PostCSS user should 
1. install the following npm deps:
```
npm install postcss-cli postcss-import tailwindcss autoprefixer
```
2. Add a postcss.config.js file at the root of the repo:
```
const purgecss = require("@fullhuman/postcss-purgecss")({
	content: ["./hugo_stats.json"],
	defaultExtractor: (content) => {
		let els = JSON.parse(content).htmlElements;
		return els.tags.concat(els.classes, els.ids);
	},
	whitelist: [
		"extra_class"
	],
});
module.exports = {
	plugins: [
		require("postcss-import")({
			path: ["assets/css"],
		}),
		require("tailwindcss")("./assets/css/tailwindcss/tailwind.config.js"),
		require("autoprefixer"),
		...(process.env.HUGO_ENV !== "development" ? [purgecss] : []),
	],
};
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

```
<head>
[...]
{{ partial "tnd-styles/tags.html" . }}
</head>
```

## theNewDynamic

This project is maintained and love by [thenewDynamic](https://www.thenewdynamic.com).
