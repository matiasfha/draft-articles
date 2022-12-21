# Embed media components using MDX and SvelteKit

In this article you'll learn how to effectively use this new authoring media to embed rich media components into your SvelteKit site while keeping the simplicity of markdown format for writing your content.

To accomplish this goal, through the article you'll learn:

- What is MDX and MDsveX
- How to setup an SvelteKit app
- How yo create your own media components to use in markdown

At the end you'll be able to add custom media elements to your markdown files like:

- Embeded youtube videos
- CodeSandbox
- Twitter items
- Snack
- Custom components

> You can find An example of what MDsveX can achieve directly [in the playground](https://mdsvex.com/playground)

You can find the result of this tutorial in [this github repository](https://github.com/matiasfha/mdsvex-sveltekit-demo) or play around in the following sandbox.

## What is MDX and MDsveX?

[MDX](https://github.com/mdx-js/mdx) is a combination of Markdown and JSX

An authoring format that allow the writer to write the well known markdown format but also introduce dynamic content by using the power of JSX components

It takes a string of markdown and transform it to a Javascript string (React or Svelte)

It basically allow you to consider a regular markdown file as a component opening the door to add working code into markdown file.

The MDX parser will insert your custom components into the parsed result, you can even replace base elements as paragraphs, headings and others HTML elements.

This allow you to have a better authoring experience and to create all of your content using your lovely markdown, but also empowering your content with rich media elements like interactive charts, alerts or embed external content like youtube video, images, etc.

[MDsveX](https://mdsvex.com/) is the answer from [Svelte ](https://svelte.dev)world to the same need of authoring rich media content using a powerfull media like markdown. This is a markdown pre-processor for Svelte components, this pre-processor allows you to use Svelte components in your markdown, or viceversa. It supports all Svelte syntax and alsmot all Markdown

It uses  [remark](https://remark.js.org/) and [rehype](https://github.com/rehypejs/rehype) so you can use several plugins to enhace your experience.

## Get up running with SvelteKit

SvelteKit is a framework on top of Svelte, at the time of this writing is still in beta and under active development. This framework can be seen as the Next.js version for Svelte, full of features and opinions about routing, layout, state management, api endpoints, SSG and SSR.

SvelteKit allows you to build full fledged applications with an outstanding developer experience and an incredibly fast user experience coming from the Svelte philosophy.

One way to try Sveltekit is by building a website to share content like a blog or articles site, this is the easier way to accomplish two things:

- Get to know SvelteKit
- Integrate MDsveX to the mix.

Let's start by setting up your SvelteKit powered site

```other
npm init svelte@next my-site
cd my-site
npm install
```

> Add an image here

Then to add some style will add tailwind

```other
npx svelte-add tailwindcss
```

and similar to add MDsveX

```other
npx svelte-add mdsvex
```

With that you'll have the base setup for an SvelteKit site that will use Tailwindcss and MDsveX

```other
├── README.md
├── mdsvex.config.cjs
├── mdsvex.config.js
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

Let's say that you want to have a list of blog posts and you love markdown to write those

Since SvelteKit use a file-base routing system you can leverage this and quickly setup your markdown files (or .svx) inside `src/routes` everything under that folder will be considered as a page.

```other
├── src
│   └── routes
│       ├── blog
│       │   └── my-first-post.svx
│       └── index.svelte
```

This means that there is a new page under `https://your-site.com/blog/my-first-post`

You can try this right away by running `npm run dev` and visiting  `http://localhost:3000/blog/my-first-post`

## Markdown Content

The markdown file will support frontmatter, this content will act as metadata variables that can be used by the svelte components.

```other
--- 
title: Svex up your markdown 
description: My first markdown content
keywords:
 - Keyword 1
 - Keyword 2
--- 

# { title }

This is more markdown **base content**
```

To enhace the experience further, mdsvex allow the use of custom layouts

### Layouts

SvelteKit support the concept of Layouts, this is an special component that can wrap every page under it, this is very useful to create shared elements that should be visible across every page.

To create a layout component you just need to create a file named as `__layout.svelte`

Upon creation of your site code there is a base layout component under `src/routes/__layout.svelte`

A similar concept exist for mdsvex files. There is a configuration option that allow you to provide a custom layout component that will wrap your mdsvex file like

```javascript
<Layout {...props}>
	<YourMarkdownContent />
</Layout>
```

This layout components will receive all the frontmatter data of your files as props

To configure this layout you can update the `mdsvez.config.js` file and pass a `string` that represents the path to your layout component

```other
mdsvex({
	layout: join(__dirname, './src/components/PostLayout.svelte')
})
```

There are cases where you have different markdown based pages with completely different content, in this cases would be useful to have different layouts for the content, that can be addressed by passing an object to the `layout` property where each key of the object is considered as the name of the layout.

```javascript
mdsvex({
	layout: {
		blog: "./path/to/blog/layout.svelte",
		article: "./path/to/article/layout.svelte",
		_: "./path/to/fallback/layout.svelte" // Default layout
	}
});
```

By using this configuration you can decide with layout should be user to which file by declaring it in the frontmatter section of the file, but if that option is not present, MDsveX will try to pick the correct layout based in your folder structured.

Let's create a layout for your current post files, just create a new folder and file under `src/lib/components/PostLayout.svelte`

> In this example, components are saved under the `lib` folder to make it easy to access them. SvelteKit [comes with some default paths](https://kit.svelte.dev/docs#modules-$lib) as `$lib` that make reference to the `src/lib` folder.

This component can looks something like this

```javascript
<script>
	export let title;
	export let description;
	export let keywords;
</script>

<svelte:head>
	<title>{title}</title>
 	<meta name="description" content={description} />
	<meta name="keywords" content={keywords.join(', ')} />
</svelte:head>

<article  class="w-full p-8">
	<header>
		<h1>{title}</h1>
	</header>
	<main>
		<slot />
	</main>
	<footer>
		<a href="">Share this on Twitter</a>
	</footer>

</article>
```

at the very top you can see a few variables exposed as props inside the script tag, the value for this props come directly from the markdown frontmatter.

> Check [this page under the svelte documentation](https://svelte.dev/docs#script https://svelte.dev/docs#script) to learn more about props

Then, in the component code you can see a reference to `svelte:head`, this is an svelte element that make its possible to manage the content of the `document.head`. By using it, all the content you add as a child of `svelte:head` get's inserted in the `<head />` of the current document. This way you can handle some of the SEO content for your article.

Then, is the actual post layout that in this case consist just in a few html elements. You can see that this component refernce the `title` prop to render the title. And use another svelte especial element `<slot />`. This is a way to define child components, meaning that the "user" of this component can insert any element into this place. This slot is used by MDsveX to insert the parsed markdown content.

This way you can control the way your markdown content is rendered, if you want to have more granular control over how the html elements inside the slot are created, then you can use what is known as `custom components`.

### Custom components

Under MDsveX, custom components are a way to replace the elements that markdown would normally generate, for example, the example markdown content used here

```other
--- 
title: Svex up your markdown 
description: My first markdown content
keywords:
 - Keyword 1
 - Keyword 2
--- 

# { title }

This is more markdown **base content**
```

Will be compiled to

```html
<h1>title</h1>

<p>This is more markdown <strong>base content</strong></p>
```

By using custom components in your layout you can replace this elements by more complex constructions, like a custom blockquote section.

Let's create the custom blockquote first, this is basically a blockquote element that, when hover, will show a tweet button.

You can check the source code and demo of the component [directly in svelte REPL](https://svelte.dev/repl/904383078c13496f9eab293bf04debce?version=3.42.4)

Save this component under `src/lib/components/Quote.svelte` and now, let's use it as a custom component.

In your `PostLayout` component, add a new script tag. Custom components are defined in the "compilation" step, so to be able to use them you need to import them inside a  `context="module"` script and export it.

The export of this custom component has to be a **named export** each named expoer must be named after the actual element you want to replace,

```javascript
<script context="module">
    import blockquote from '$lib/components/Quote.svelte'
	export { blockquote }
</script>
```

Now, after compilation, MDsveX will generate a new content using your custom `Quote` component each time a `blockqote` is required.

# Media elements

Now that the setup part is done, is time to harness the power of MDsveX by adding media elements to your markdown content.

Let's start by adding some video content, for that you'll create a new component under `src/lib/Youtube.svelte`

```html
<script>
	export let videoId
</script>

<div class="video-container">
  <iframe title="Youtube video" src={`https://www.youtube.com/embed/${videoId}`} frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

<style>
.video-container {
    overflow: hidden;
    position: relative;
    width:100%;
}

.video-container::after {
    padding-top: 56.25%;
    display: block;
    content: '';
}

.video-container iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}
</style>
```

This component allow you to embed a youtube video by just passing the video identifier as prop under `videoId.`

Now, in your markdown file you can do

```other
--- 
title: Svex up your markdown 
description: My first markdown content
keywords:
 - Keyword 1
 - Keyword 2
--- 

<script>
import Youtube from '$lib/components/Youtube.svelte'
</script>

# { title }

This is more markdown **base content**

<Youtube videoId="AdNJ3fydeao" />
```

Just like another svelte component, now you can import components into the markdown and render it!. MDsveX will take care of parse and transform the entire page and to correctly render the content.

Take a look at the result by running (in case you don't have it running) `npm run dev` and visiting `http://localhost:3000/blog/my-first-post`

Now you'll see the video content directly in your post.

Now, let's create another component to embed codeSandbox examples.

```html
<script>
	export let sandboxId
</script>

<iframe
	data-testid="codesandbox"
	title={`codeSandbox-${sandboxId}`}
	src={`https://codesandbox.io/embed/${sandboxId}`}
	allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
	sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
/>

<style>
	iframe {
		width: 100%;
		height: 500px;
		border: 0;
		border-radius: 4px;
		overflow: hidden;
	}
</style>
```

As you can see, the process to create this embed media components is fairly simple with Svelte, just add the corresponding iframe element and the required custom properties, in this case the prop `sandboxId`.

Now, in your markdown content you can use it like

```other
--- 
title: Svex up your markdown 
description: My first markdown content
keywords:
 - Keyword 1
 - Keyword 2
--- 

<script>
import Youtube from '$lib/components/Youtube.svelte'
import CodeSandbox from '$lib/components/CodeSandbox.svelte'
</script>

# { title }

This is more markdown **base content**

<Youtube videoId="AdNJ3fydeao" />

<CodeSandbox sandboxId="j0y0vpz59"/>
```

> [Check this sandbox](https://codesandbox.io/s/wispy-tdd-3oq00?file=/App.svelte)  to review how this component works

Now let's create a Tweet component to embed a particular tweet into the markdown content.

This is a bit different the previous one since it requires to load an external script, but svelte give you simple tools to accomplish this.

```html
<script>
  import { onMount } from "svelte";
  let mounted = false;
  export let id;

  onMount(() => {
    // The payment-form is ready.
    mounted = true;
  });

  function scriptLoaded() {
    if (mounted) {
      var tweet = document.getElementById("tweet");
      window.twttr.widgets
        .createTweet(id, tweet, {
          conversation: "none", // or all
          cards: "hidden", // or visible
          linkColor: "#cc0000", // default is blue
          theme: "light" // or dark
        });
    }
  }
</script>

<svelte:head>
  <script src="https://platform.twitter.com/widgets.js" on:load={scriptLoaded}></script>
</svelte:head>
    

<div id="tweet" />
```

First thing is to use the `svelte:head` component to add a script tag into the document head.

This script tag point to the twitter widget library and triggers a callback function when is loaded by using the `on:load` event.

Then, in the `script` tag of this component you can see that the calback function `scriptLoaded` function check if the component is already mounted and then run the twitter setup to show the tweet.

To identify what tweet you want to render, just use the prop `id`

This way you have all you need to add external media components into your markdown files powered by MDsveX.

## Create a list of markdown files

Something worth to mention when working with MDsveX is about how to create a page to list your markdown files or posts, to do this you'll use the power of [endpoint routes](https://kit.svelte.dev/docs#routing-endpoints) and [glob import](https://vitejs.dev/guide/features.html#glob-import) feature from Vite.

First let's create an api endpoint to gather the content; create a file under `src/routes/api/blog/index.json.ts`

Inside that file, create and export a `get` function, this function will load, parse and return a json object with the list of your files.

```javascript
async function getPosts() {
	const modules = import.meta.glob(`../routes/blog/post/*.svx`);  // load all the svx files 

	const postPromises = [];
	for (const [path, resolver] of Object.entries(modules)) {
		const promise = resolver().then((post) => {
			const slug = path.match(/([\w-]+)\.(svelte\.svx)/i)?.[1] ?? null; // Create the slug
			return {
				slug,
				...post.metadata
			};
		});
		postPromises.push(promise);
	}

	const posts = await Promise.all(postPromises);
	return posts
}


export async function get() {
	const posts = await getPosts();
	return {
		body: {
			posts
		}
	};
}
```

The key here is the `import.meta.glob` that will retrieve all the files that match the glob, in this case, that ends with the `.svx` extension

Now, create the page to list this files under `src/routes/blog/index.svelte`

This svelte component will use a [loader function](https://kit.svelte.dev/docs#loading) to fetch the data from the previous api endpoint and then render that in the screen.

```html
<script lang="ts" context="module">
	export async function load({ fetch }) {
		const url = '/api/blog.json';
		const res = await fetch(url);
		if (res.ok) {
			const { posts  } = await res.json();
			return {
				props: {
					posts
				}
			};
		}

		return {
			status: res.status,
			error: new Error(`Could not load ${url}`)
		};
	}
</script>

<script >
	export let posts; // The prop value comes from the module above
</script>

<ul>
{#each posts as post}
<li>
	<a href={`/blog/posts/${post.slug}`}>
		{post.title}
	</a>
</li>
{/each}
</ul>
```

This component will just get the data returned by the api endpoint and create a list of titles from your `.svx` files.

# Conclusion

Use markdown is an easy and simple way to create your content but it lack interactivity and dinamism, that piece can be added by using the power of pre-processors and AST that enable the existance of tools like MDsveX.

MDsveX let you use the full power of Svelte components from inside your markdown content adding that dynamic layer.

Anything that you can think and build as an Svelte component can be used inside MDsveX files.

