# Setup a Developer Blog with Social Images using Sveltekit

The end goal for this article is to implement a blog site that includes the use of open graph images for social sharing. It will consist of a:

- A Home page with a list of some articles.
- An article page to allow the user to read the articles.

Each of these pages will have its own social image. To accomplish this, set the corresponding meta tag into the `<head>` of the current page.

You can check out [this repository](https://github.com/matiasfha/sveltekit-blog) that will get you to the end result of this article, clone it, run it and play around to grasp the way of work with SvelteKit, or you can just play around with this code sandbox.

# ¿What is Sveltekit?

SvelteKit is Svelte's take on the web application framework. This is the new (still in beta) official way to develop applications with Svelte. It is packed with features like routing, layouts, stage management, API routes, SSG, and SSR. If you come from React or Vue world, SvelteKit is the counterpart of Nextjs or Nuxt.

SvelteKit helps you to make static sites, server-rendered sites, and even hybrid static/server-rendered apps. It delivers an outstanding developer experience and fast user experience like the Svelte philosophy describes.

In general words, SvelteKit is a tool to take your Svelte code and transform it into a node app or static files. You can consider Svelte as the underlying language and SvelteKit the set that brings the server-side and some options about how an application should be architected.

## What are Social Images

One big part of content creation is to share your content on social media, but just tweeting about your newest post is not enough to stand out in a never-ending stream of content.

One way to stand out on all of that noise is by sharing images or "[Open Graph Images](https://ogp.me)" for your content.

These images are used by different social network applications to create a "content card" and show some kind of preview of the link you're sharing.

![Screen Shot 2021-07-05 at 23.30.39.png](https://res.craft.do/user/full/0a4fa0f6-6e08-58de-6dbc-6c56902ee35d/doc/0975E901-EF6C-4C06-87ED-0742402C1FF2/3D783C62-3CCA-44DE-8C63-6992A8AED80C_2/Screen%20Shot%202021-07-05%20at%2023.30.39.png)

You can manually create these images for each post you publish in your post, but it is a significant burden that can quickly slow down your publishing workflow. But, we are developers, and we love to craft solutions based on automation.

By the end of this article, you'll be able to generate social media images by using Cloudinary transformation API as part of your SvelteKit site. You'll also learn how to get up to speed with SvelteKit by creating your first blog.

## **Develop your blog**

This section will drive you through the steps required to create the blog described at the top of the article. This particular implementation uses:

- Prismic as headless CMS to write and deliver the articles
- tailwindcss for styling

![Screen Shot 2021-07-05 at 23.36.27.png](https://res.craft.do/user/full/0a4fa0f6-6e08-58de-6dbc-6c56902ee35d/doc/0975E901-EF6C-4C06-87ED-0742402C1FF2/6EC54153-AB56-46CF-BC5B-93935F23A442_2/Screen%20Shot%202021-07-05%20at%2023.36.27.png)

To start, we just need to run the SvelteKit scaffold script.

```shell
pm init svelte@next my-app
cd my-app
npm install
npm run dev
```

Now go ahead and visit `localhost:3000` in your browser, and you'll see the base SvelteKit project running.

Now let's set up tailwindcss to implement the base layout. For this purpose, you can use [`svelte-add`](https://github.com/svelte-add/tailwindcss)   so just run in console

```shell
npx svelte-add tailwindcss
```

With that, you are ready to go.

To start, let's create the layout of the page and think about the components. With SvelteKit, each page is a Svelte component, and to create a page, you just need to add the corresponding component under `src/routes`.

Let's start by defining the layout base structure. SvelteKit enables this by using a specially named file under `src/routes/__layout.svelte`. The content of this file will be applied to every page.

In this layout component you will use the `<slot />` component to define where the content of the pages will be rendered.

> If you come from React world, the `<slot />` component is very similar to the `children` prop.

Now you can add the first page under `src/routes/index.svelte` Check the source code of this example implementation directly here.

The site will list the articles retrieved from [Prismic](https://prismic.io) on this home page. Here is where you'll use the new features of SvelteKit.

> [Prismic](https://prismic.io) is a headless CMS that easily integrates with your choose technology to manage and deliver the content. In this case, Prismic is used in a very contrived way to store and provide the website's articles.

To start, SvelteKit has the concept of **endpoints** These are "special" modules that live under `src/route` are unique because instead of exporting a svelte component, these routes export a function corresponds with the HTTP methods.

In this example, the home page will render a list of articles. The page will hit an internal endpoint under /`API/blog/articles` to get those. This endpoint will return a JSON structure with the data from Prismic.

These are just standard javascript functions, as you can see in this example.

```javascript
// /api/blog/articles.json
import Client, { Predicates } from '$lib/PrismicClient';

export async function get() {
	const response = await Client.query(Predicates.at('document.type', 'article'));
	const result = response.results.map((r) => {
		return {
			...r.data,
			id: r.id,
			href: r.href,
			uid: r.uid,
			featured: r.data.featured
		};
	});
	const { featured, articles } = result?.reduce(
		(acc, current) => {
			if (current.featured) {
				acc.featured = current;
			} else {
				acc.articles.push(current);
			}
			return acc;
		},
		{ featured: null, articles: [] }
	);

	return {
		body: {
			featured,
			articles
		}
	};
}
```

This exported function is mapped to the get method. It uses the `PrismicClient` methods that you [can see here](https://github.com/matiasfha/sveltekit-blog/blob/main/src/lib/PrismicClient.ts) and just fetch the data, parse it and then return the new object in the body of the request.

Now, how the home page will get this data?. A page, that is, in fact, a Svelte component, can define an export a  function called `load` that will run before the component is created. The trick here is that this function runs server-side and also client-side, allowing you to retrieve data on build time if you want to get an SSG page.

You can just add a new script tag to the home page with the load function to accomplish this feat. Why a new script tag? Since this function runs before the component is rendered, it lives in a different context `<script context=" module">`.

So, open your `src/routes/index.svelte` component and add the load function that perform the fetch request to `/api/blog/articles`

```html
<script context="module" lang="ts">
	export async function load({ fetch }) {
		const url = `/api/blog/articles.json`;
		const res = await fetch(url);
		const { articles, featured } = await res.json();

		if (res.ok) {
			return {
				status: res.status,
				props: {
					articles,
					featured
				}
			};
		}

		return {
			status: res.status,
			error: new Error(`Could not load ${url}, status: ${res.status}`)
		};
	}
</script>
```

As you can see, retrieve data for your page is really straightforward. Now that the `props` object is returned, it will be available for your component, which means that in the Svelte component under this page, you can access  `articles` and `featured` objects.

```html
<script lang="ts">
	import PrismicDOM from 'prismic-dom';
	import ArticleCard from '$lib/components/ArticleCard.svelte';
	export let articles: Article[];
	export let featured: Article;
	const list = [...articles].splice(1, articles.length);
	const listHeader = articles[0];
</script>
```

Since the data comes from Prismic, you need a helper to parse and render the content of the rich text field. This page also uses a component to render a card. The component can be found in `src/lib/components/ArticleCard.svelte`

Let's check how to render the article.

To render an article, the site uses another SvelteKit feature called **dynamic parameters**. This allows you to create routes that accept a parameter like the `slug` or `uid` of the article.

The **dynamic parameter** let you load a particular element based on some identifier. To do this, let's create the dynamic page under `src/route/blog/[uid].svelte`

This page will get the `uid` from the URL and pass it down to the API to retrieve the corresponding data. This is done again in the `load` function of the page.

```html
<script context="module" lang="ts">
	export async function load({ fetch, page }) {
		const { uid } = page.params;
		const url = `/api/blog/${uid}.json`;
		const res = await fetch(url);
		const article = await res.json();
		if (res.ok) {
			return {
				status: res.status,
				props: {
					article
				}
			};
		}

		return {
			status: res.status,
			error: new Error(`Could not load ${url}, status: ${res.status}`)
		};
	}
</script>
```

This load function will get the `uid` and use it to run the endpoint get function defined in `API/blog/[uid].json.ts` that perform the query to Prismic based on the `uid` parameter.

Then, this page will have access to the corresponding article instance to render it as is shown here.

Here is where the social images come into play.

# Setup Social Images

Some of the most shared content of a site or blog is the articles, so each piece should have its own social images (and SEO data) based on the content that is actually present. This can be easily accomplished by a neat Svelte feature: the `<svelte:head>` component.

The `svelte:head` component allows you to easily  insert elements into the `<head>` tag of the document, and since the Sveltekit pages are a document by itself, by using `svelte:head`, you'll end up defining the meta tags for each article in just one file :D

Now you have a few options to generate or retrieve the social image for your article. You can directly use the same image you added to the article and put it into the corresponding meta tag or use a neat trick provided by Cloudinary. Add overlays to the image so you can share more information in it.

## With Cloudinary

If you decided to use Cloudinary, then you have options too:

- Upload the header image of the article to Cloudinary and apply transformations to it like adding text overlay, your logo etc
- or, use a pre-defined template with the corresponding text overlay for the article title.

Personally, I prefer the second option, have a template image that shows my personal brand where I can include the post title and keywords or subtitle. To do this, you first need to define the template image.

![example_og_image.jpeg](https://res.craft.do/user/full/0a4fa0f6-6e08-58de-6dbc-6c56902ee35d/doc/0975E901-EF6C-4C06-87ED-0742402C1FF2/85228054-0750-41C3-87E8-49776CA7AC31_2/example_og_image.jpeg)

### What this template image should have?

To create a reusable template for your social image, need you need an image that can work for any post you make. This image will include

- Your logo or profile picture
- The post title: This is the dynamic part of the template, so in the template, you need to reserve space for it. There are some guidelines for the length of the title. You can use some tools like the [SEO snippet generator](https://app.sistrix.com/en/serp-snippet-generator) to check the size of your post titles
- Subtitle, keywords, or tagline: Just an additional text space to show some extra data.

> You can find a base template [in this Figma board](https://www.figma.com/file/MZjbFJW3vHNKhv9Cd1hDlp/social-image-template?node-id=0%3A1)

> Or just [use this image](https://res.cloudinary.com/matiasfha/image/upload/v1624803419/example_og_image.jpg) for testing purposes

With your base image done, you need to use Cloudinary transformation capabilities to add the corresponding text overlay. This can be simple done in a function to be reused across your project.

Let's use the `Cloudinary nodejs package that can be found in npm

"`shell
npm install cloudinary --save

```other
> You can check in deep documentation in [cloudinary site](https://cloudinary.com/documentation/node_integration#overview)

For this function, you'll use the `cloudinary.url` method to define transformations using javascript objects and return the corresponding URL for it.

Let's start by creating the function for this job. It will lib under `src/lib` . SvelteKit comes with some default configurations for some folders like this one allowing you to access the modules from that library by using `$lib`.

So, how this function looks like?

"`javascript
import cloudinary from 'cloudinary';

cloudinary.v2.config({
	cloud_name: import.meta.env.VITE_CLOUDINARY_CLOUD_NAME,
});


async function getOgImage({ title, subTitle }) {
	const url = cloudinary.v2.url('example_og_image.jpg', {
		transformation: [
			{ width: 1200, height: 627, crop: 'fill', quality: 'auto', format: 'auto' },
			{
				crop: 'fit',
				width: 700,
				x: 480,
				y: 254,
				gravity: 'south_west',
				color: 'white',
				effect: 'shadow:40',
				overlay: {
					font_family: 'roboto',
					font_size: 54,
					font_weight: 'bold',
					text: encodeURIComponent(title)
				}
			},
			{
				crop: 'fit',
				width: 700,
				x: 480,
				y: 154,
				gravity: 'south_west',
				color: 'white',
				overlay: {
					font_family: 'roboto',
					font_size: 34,
					font_weight: 'bold',
					text: encodeURIComponent(subTitle)
				}
			}
		]
	});

	return URL;
}

export default getOgImage;
```

Let's break down that code.

First, you'll find the configuration for using cloudinary. This config method is using some environment variables. If you need to use custom environment variables inside your code, you need to name it with the prefix `VITE_`. The use of the prefix will make the variable immediately available under the `import.meta` object.

> Sveltekit uses Vite for building the application, and Vite uses dotenv to load the variables from the `.env` file.

> Vite enables the `import.meta` object to expose context-specific data to a module.

> You can find more information about environment variables in [Sveltekit site](https://kit.svelte.dev/faq#env-vars) and [Vite docs](https://vitejs.dev/guide/env-and-mode.html#modes)

Then, you have the function definition that is receiving an object as an argument that has 2 attributes: `title` and `subTitle`. These are the overlays you want to create

Now is the time to use cloudinary transformations.

The `cloudinary.url` method accepts 2 arguments, the name of the base image and an object with options. This object is where the magic happens. One of the properties of the options object is the `transformation` array that accepts multiple transformation object descriptors. For this case, you'll use 3 transformations.

1. First,  define the "canvas" to work on. Will set format, quality, and size for the base image.
2. Second will define the first text overlay for the title
3. Third one defines the transformation to create another text overlay for the subTitle

> You can find more in deep documentation about transformation in [Cloudinary documentation site](https://cloudinary.com/documentation/node_image_manipulation#apply_common_image_transformations)

Now is time to use this function and really use your newly created social image.

Let's go back to the `get` function in our endpoint route for an article `src/routes/api/blog/[uid].json.ts`

```javascript
import Client from '$lib/PrismicClient';
import PrismicDOM from 'prismic-dom';
import getOgImage from '$lib/getOgImage';


export async function get({ params }) {
	const { uid } = params;
	const response = await Client.getByUID('article', uid);
	const { title, subTitle } = response.data;

	return {
		body: {
			...response.data,
			id: response.id,
			href: response.href,
			uid: response.uid,
			ogImage: getOgImage({
				text: PrismicDOM.RichText.asText(title),
				subTitle: PrismicDOM.RichText.asText(subTitle)
			})
		}
	};
```

Here you can just use your `getOgImage` function passing down the text you want to show in the social image card, then the URL generated by the function will be available in the svelte component/page found in `src/routes/blog/[uid].svelte`

### Let's show the social image card

Now that the svelte component has the URL, we can just use `<svelte:head>` and add the corresponding meta tags, but it will be way better to have a reusable component for future pages, right?.

Let's build an SEO component under `src/lib/components/Seo.svelte`

And write down the meta tags we need for a basic Seo data definition.

```html
<script lang="ts">
	export let title: string;
	export let description: string = undefined;
	export let keywords: string;
	export let canonical: string = undefined;
	export let type: string;
	export let image: string;
</script>

<svelte:head>
	<title>{title}</title>
	<meta name="robots" content="index, follow" />
	<meta name="googlebot" content="index,follow" />
	{#if description}
		<meta name="description" content={description} />
	{/if}
	{#if canonical}
		<link rel="canonial" href={canonical} />
	{/if}
	<meta name="keywords" content={keywords} />

	<meta property="og:title" content={title} />
	{#if description}
		<meta property="og:description" content={description} />
	{/if}
	{#if canonical}
		<meta property="og:url" content={canonical} />
	{/if}
	<meta property="og:type" content={type ? type : 'site'} />
	<meta property="og:image" content={image} />

	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:title" content={title} />
	{#if description}
		<meta name="twitter:description" content={description} />
	{/if}
	<meta name="twitter:image" content={image} />
</svelte:head>
```

This component exposes a few props. You can found them defined in the `<script>` tag as an exported variable.

Then we have the component definition that creates some meta tags.
The ones that use the social image you created are `og:image` and `twitter:image`

Let's go back to the article page`src/routes/blog/[uid].svelte` and use the new Seo component

```html
<script lang="ts">
	import PrismicDOM from 'prismic-dom';
	import Seo from '$lib/components/Seo.svelte';
	export let article;
	const title = PrismicDOM.RichText.asText(article.title);
	const subTitle = PrismicDOM.RichText.asText(article.subTitle);

</script>

<Seo {title} {subTitle} keywords="" image={article.ogImage} type="article" />
```

Here the code import the Seo component from `$lib/components/Seo.svelte`, define the `article` prop with the data from the `load` function, and create a new variable with the `title` and `subtitle`

Then the code just uses the `Seo` component bypassing the required props. Note here that neat syntax `{title}` that is actually a shortcut to `title={title}`.

Now, if you run your project and go to an article, you'll find the `og:image meta tag with the corresponding cloudinary URL of your social image.

## Without Cloudinary

If you don't want to use Cloudinary transformation API and prefer to just use the same image you defined for your article, you can use the same Seo component. Still, instead of passing the `article.image` as value to `image` prop, you'll need to give the URL of the image. Since this project is using Prismic, you'll access `article.image.url`

```other
<Seo {title} {subTitle} keywords="" image={article.image.url} type="article" />
```

# Summary

In this article, you were able to review the power of the new toolset for Svelte and create a basic website to publish your content using SvelteKit. At the same time, you learned the importance o

