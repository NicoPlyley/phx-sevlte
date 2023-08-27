# PHX Svelte

PHX Svelte, crafted by Phoenix Tech Lab, is an internal template tailored for expedited SvelteKit 
development. With a focus on TypeScript, Tailwind CSS, and Playwright integration, 
PHX Svelte streamlines the creation of dynamic websites. It offers an efficient foundation,
enabling developers to leverage modern tools and technologies seamlessly.

## Table Of Contents

1. Tailwind 
   - [Tailwind Theme](#tailwind-theme)
   - [Tailwind Stylesheet](#tailwind-stylesheet)
2. Components
    - [Component Structure](#component-structure)
    - [Component Styles](#component-styles)
    - [Page Components](#page-components)
    - [Component Props](#component-props)

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

## Component Structure

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

## Page Components

In Svelte development, you often need components that work closely with specific pages, 
or extend an existing components' functionality. 
While the usual way is directly in the `+page.svelte` file, this can become confusing 
if your pages have lots of content. As your project grows, finding the right parts of code might get tricky.

To make things simpler, there's the idea of page components. These components are designed for individual 
pages and don't have a broader use. They're stored in the same folder as the page they belong to.
So in our final `+page`, there should only be a component for each section on the page.

```html
<!-- Here's how a page component fits into the structure -->

<!-- src/routes/about/+page.svelte -->
<script lang="ts">
   import { Header } from '$lib';
   import About from './About.svelte'; // This is a page component

   export let data = {};
</script>

<Header data={data.header} /> <!-- You can use this without a page component -->

<About data={data.about} /> <!-- Using the page component for specific content -->

```

> In this case the About component may still call multiple other components inside of it and extend them


## Component Props

When working with components that are utilized across various pages or those nested within page components, 
employing a structured approach to props can greatly enhance clarity and organization.

For instance, if you have a component like a header that requires both a title and an image, 
consider passing them together as a dictionary through the data prop. This way, you're consolidating 
related information, which often originates from `+page.server` 
(a topic we'll delve into later in this documentation).

```html
<!-- +page.svelte -->
<script lang="ts">
   import { Header } from '$lib';
   
   export let data = {};
</script>

<Header data={data.header} />
```

```html
<!-- components/header/index.svelte -->
<script lang="ts">
   export let data = {};
   
   const { title, image } = data;
</script>

<div style="background: url('{image}')">
   <h1>Title</h1>
</div>

```
When your components are designed to be used within page components, the choice between utilizing a 
single data prop or employing multiple props depends on your specific use case. You can often simplify 
by using a single data prop or even harnessing
[Named Slots](https://svelte.dev/docs/special-elements#slot) or
[Svelte Fragments](https://svelte.dev/docs/special-elements#svelte-fragment), 
complemented with necessary props passed 
along, such as adding a background through CSS. This flexibility ensures your components are adaptable to 
various scenarios, all while maintaining code clarity and cohesion
