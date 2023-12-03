# Shopify | Vite | Tailwind | Alpine - Dev Start

## Prerequisites

I'm assuming you've already have a working environment w/ Node, NPM, Git, Vite, Shopify-cli installed.
If not, please install them first.

## Installation

1. Download Shopify's theme file

```sh
shopify theme init [name]
```

_NOTE:_ this line of code will download latest Shopify | Dawn version

2. Start w/ Project Basics

```sh
npm init -y
npm create vite
npm install
npm install --save-dev tailwindcss postcss autoprefixer
npx tailwindcss init -p
npm install --save-dev vite
npm install --save-dev vite-plugin-shopify
npm install --save-dev npm-run-all
```

_NOTE:_ While creating vite (project name is non important), select Vanilla > Javascript

_NOTE:_ --force package if any errrors

3. Create and modify the ./vite.config.js

```js
import shopify from "vite-plugin-shopify";

export default {
  plugins: [
    shopify({
      themeRoot: "./",
    }),
  ],
};
```

4. Update the tailwind.config.js file

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./config/*.json",
    "./layout/*.liquid",
    "./assets/*.liquid",
    "./sections/*.liquid",
    "./snippets/*.liquid",
    "./templates/*.liquid",
    "./templates/*.json",
    "./templates/customers/*.liquid",
  ],
  theme: {
    screens: {
      sm: "320px",
      md: "750px",
      lg: "990px",
      xlg: "1440px",
      x2lg: "1920px",
      pageMaxWidth: "1440px",
    },
    extend: {},
  },
  plugins: [],
};
```

5. Create tailwindcss file

```sh
mkdir src
cd src
vim tailwind.css
```

6. Add the next info

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

7. Update (first line) postcss.config.js

```javascript
// export default {
module.exports = {
```

_NOTE:_ this is because of the Vite version

8. Create tailwind build process

```sh
npx tailwindcss -i ./src/tailwind.css -o ./assets/tailwind.css
```

9. Update package.json > scripts

```json
{
  "scripts": {
    "dev": "run-p -sr dev:shopify-sync dev:vite dev:tailwind",
    "dev:shopify": "shopify theme dev",
    "dev:shopify-sync": "shopify theme dev --theme-editor-sync",
    "dev:tailwind": "npx tailwindcss -i ./src/tailwind.css -o ./assets/tailwind.css --watch",
    "dev:vite": "vite",
    "build": "vite build",
    "build:css": "npx tailwindcss -o assets/tailwind.css --minify",
    "build:deploy": "run-p -sr build:css && run-p -sr build:push",
    "build:push": "shopify theme push",
    "preview": "vite preview"
  }
}
```

10. Install scripts

```ssh
$ npm install
```

11. Update Shopify Project files

1. Update the layout/theme.liquid file (just before the </head> tag)

```liquid
{% comment %} Tailwind CSS Configs {% endcomment %}
{{ 'tailwind.css' | asset_url | stylesheet_tag }}
```

2. Update the layout/theme.liquid file (just before the </body> tag)

```liquid
{% comment %} Alpine JS {% endcomment %}
<script
  defer
  src="https://cdn.jsdelivr.net/npm/alpinejs@3.13.1/dist/cdn.min.js"
></script>
```

Alpine JS example

```html
<div x-data="{ open: false }">
  <button @click="open = true">Expand</button>

  <span x-show="open"> Content... </span>
</div>
```

12. Add Shopify Prettier support

```ssh
npm install --save-dev prettier @shopify/prettier-plugin-liquid
```

13. Optionals

- Download .gitignore file
- Download .prettierrc file
- Download .shopifyignore file
- Remove the vite vanilla files from base directory

## Shopify Dev Start

```sh
npm run dev
```

## Shopify Push (select theme to update)

```sh
npm run build:push
```

## Shopify Deploy

```sh
npm run build:deploy
```

NOTE: you need to have an already theme created on the store just to be pushed

---

Updated 20231202
