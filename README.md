# PHX Svelte

PHX Svelte, crafted by Phoenix Tech Lab, is an internal template tailored for expedited SvelteKit 
development. With a focus on TypeScript, Tailwind CSS, and Playwright integration, 
PHX Svelte streamlines the creation of dynamic websites. It offers an efficient foundation, 
enabling developers to leverage modern tools and technologies seamlessly.

## Tailwind Theme

To generate a Tailwind theme, follow these steps:

 - Go to [UI Colors](https://uicolors.app/create).
 - Add your base color. The tool will generate shades that you can incorporate into your
`tailwind.config.js` file.

> The theme structure should be as follows:

```js
// tailwind.config.js
export default {
  content: ['./src/**/*.{html,js,svelte,ts}'],
  theme: {
    colors: {
      primary: {
        50: '#fef7ee',
        100: '#fdeed7',
        // 200 - 800
        900: '#763218',
        950: '#40170a',
      },
      secondary: {
        // Define secondary color values
      },
      tertiary: {
        // Define tertiary color values
      },
      grey: {
        50: '#FFF', // Always white 
        // Define grey color values
        950: '#000' // Always black
      },
    },
    fontFamily: {
      sans: ['fontHere', 'sans-serif'],
    },
  },
  plugins: [],
};

```
## Components

All components will be found inside the `lib` directory in the following format

```
project    
│
└─── 📂 lib
     │   📜  index.ts
     └─── 📂 components
         │
         └─── 📂 myComponent
              │   📜 index.svelte
              │   📜 additonalFile.svelte

```

Inside `src/lib/index.ts`, there should be exports for all the components, stores, or other functions, 
organized by type and listed in alphabetical order.

```js
// index.ts

// Components
export { default as ComponentA } from './components/componentA/index.svelte';
export { default as ComponentB } from './components/componentB/index.svelte';

// Stores
export { sotreA } from './store'
export { sotreB } from './store'

// Utilities
export { default as utilityA } from './utils/utilityA';
export { default as utilityB } from './utils/utilityB';
```

> Now, all files can be imported using the following method:
> ```js
> import { ComponentA } from '$lib';
>```

## Tailwind Stylesheet
Inside the `src/app.css` file, styles for the h1 and h2 heading tags should be based 
on or built upon the foundational layer of the Tailwind CSS framework. 
This ensures that the h1 and h2 tags share the exact same styles, conforming to SEO standards. 
Meanwhile, the rest of the titles on a page can maintain uniformity in size. 
If larger headings are necessary, they can be overridden.

```css
/* app.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
    h1, h2, h3, h4, h5, h6 {
        @apply font-main font-serif text-grey-500
    }

    h1, h2 {
        @apply text-2xl md:text-4xl
    }

    h3 {
        @apply text-xl
    }

    p {
        @apply font-sans text-base text-grey-700
    }
}
```

## Component Styles

When it comes to component styling, customization is key. Certain components may demand extra styles 
or even the need to override existing ones. One approach gaining popularity is the utilization of a 
prop named className. Yet, employing this prop introduces a limitation: it doesn't harmonize well with
the tailwindcss prettier plugin, which thrives on optimizing class arrangements.

Fortunately, Svelte extends a solution through its reserved named class feature. This allows you to 
attach a prop that bears the name class.

```html
<script lang="ts">
  let className = ''; // Initialize with an empty string
  
  export { className as class };
</script>

<div class="flex {className}"> <!-- Utilize the class -->
  <slot />
</div>
```

You might encounter scenarios where you need to create a class and tap into your project's 
theme colors or fonts. Fortunately, postcss offers a solution:

```html
<style lang="postcss">
  .myClass {
    background: theme(colors.primary.500);
    font-family: theme(fontFamily.sans);
  }
</style>
```
