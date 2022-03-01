# Create a Video Gallery and Upload Videos with Sveltekit

At the end of this article you'll know how to build a video (or other media for what matters) gallery using SvelteKit as the foundational layer, and at the same time uploading videos using the framework.

To accomplish this goal, through the article you'll find content and concepts about

- What is Svelte and SvelteKit (and how to setup it).
- How to use tailwindCss for quick prototyping.
- How to use Cloudinary as media storage.
- How to use SvelteKit api endpoints to manage the content from Cloudinary.

You can find the result of this article in [this github repository ](https://github.com/matiasfha/sveltekit-video-gallery-demo), deployed [in this site](https://sveltekit-demo-video-gallery.netlify.app/) or play around in the following sandbox.

## Get up running with SvelteKit

SvelteKit is a framework on top of Svelte, at the time of this writing is still in beta and under active development. This framework can be seen as the Next.js version for Svelte, full of features and opinions about routing, layout, state management, api endpoints, SSG and SSR.

SvelteKit allows you to build full fledged applications with an outstanding developer experience and an incredibly fast user experience coming from the Svelte philosophy.

Let's start by setting up your SvelteKit powered site

```other
npm init svelte@next video-gallery
cd video-gallery
npm install
```

> Add an image here

Then to add some style will add tailwind

```other
npx svelte-add tailwindcss
```

With that you'll have the base setup for an SvelteKit site that will use Tailwindcss and

```other
├── README.md
├── package.json
├── postcss.config.cjs
├── src
│   ├── app.html
│   ├── app.postcss
│   ├── global.d.ts
│   ├── lib
│   └── routes
│       └── index.svelte
├── svelte.config.js
├── tailwind.config.cjs
└── tsconfig.json
```

The video gallery will be a simple site with one page, this page (the main route) will show a list of the existing videos.

SvelteKit use a file-base routing system, meaning that every `.svelte` file stored under  `src/routes` will be considered a page/route

```other
├── src
│   └── routes
│       └── index.svelte
```

You can try this right away by running `npm run dev` and visiting  `http://localhost:3000`

### Layouts

SvelteKit support the concept of Layouts, this is an special component that can wrap every page under it, this is very useful to create shared elements that should be visible across every page.

To create a layout component you just need to create a file named as `__layout.svelte`

Upon creation of your site code there is a base layout component under `src/routes/__layout.svelte`

## Create a list of video files

# Conclusion

Use markdown is an easy and simple way to create your content but it lack interactivity and dinamism, that piece can be added by using the power of pre-processors and AST that enable the existance of tools like MDsveX.

MDsveX let you use the full power of Svelte components from inside your markdown content adding that dynamic layer.

Anything that you can think and build as an Svelte component can be used inside MDsveX files.

