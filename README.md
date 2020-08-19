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
├── fonts
│   ├── files
│   │   ├── play-v11-latin-regular.woff
│   │   └── play-v11-latin-regular.woff2
│   └── index.css
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

Add a `assets/fonts/index.css` containing the fontface declarations
Add the font files to `assets/fonts/files`

### tnd-styles/tags

The partial should be invoked in your `<head>` and will print all the necessary Stylesheet tags.

## theNewDynamic

This project is maintained and love by [thenewDynamic](https://www.thenewdynamic.com).