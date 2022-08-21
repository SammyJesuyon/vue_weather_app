# .

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
npm init vue@latest
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Tailwind configuration

```sh
npm install -D tailwindcss postcss autoprefixer
```

### tailwind.config.js file
```sh
npx tailwindcss init -p
```
Create a tailwind.css file in the assets directory.
import it in the main.js file.

API: mapbox API, Weatherbit.io

### About the app:
My weather cast collects a query from the user and displays the weather information for that particular day and also displays forecast for the next 16 days.
The app is also able to track any region that is previewed as the first step of the app is to preview a region.