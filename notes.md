# Next.js Notes

## Overview
Next.js aims to have best-in-class developer experience and many built-in features, such as:

* An intuitive page-based routing system (with support for dynamic routes)
* Pre-rendering, both static generation (SSG) and server-side rendering (SSR) are supported on a per-page basis
* Automatic code splitting for faster page loads
* Client-side routing with optimized prefetching
* Built-in CSS and Sass support, and support for any CSS-in-JS library
* Development environment with Fast Refresh support
* API routes to build API endpoints with Serverless Functions

## Navigation
* a page is a React Component exported from a file in the pages directory.
* Pages are associated with a route based on their file name. For example, in development:

`pages/index.js` is associated with the `/`route.
`pages/posts/first-post.js` is associated with the `/posts/first-post route`.

## Linking
* In Next.js, you use the Link Component from next/link to wrap the `<a>` tag. `<Link>` allows you to do client-side navigation to a different page in the application.
  
* Client-side navigation means that the page transition happens using JavaScript, which is faster than the default navigation done by the browser.
  
* If you need to link to an external page outside the Next.js app, just use an `<a>` tag without Link.
  
* If you need to add attributes like, for example, className, add it to the `<a>` tag, not to the Link tag

## Code splitting and prefetching
* Next.js does code splitting automatically, so each page only loads what’s necessary for that page. That means when the homepage is rendered, the code for other pages is not served initially.

* Only loading the code for the page you request also means that pages become isolated. If a certain page throws an error, the rest of the application would still work.
  
* in a production build of Next.js, whenever Link components appear in the browser’s viewport, Next.js automatically prefetches the code for the linked page in the background. By the time you click the link, the code for the destination page will already be loaded in the background, and the page transition will be near-instant!

## Static file serving
* Next.js can serve static files, like images, under a folder called `public` in the root directory. Files inside `public` can then be referenced by your code starting from the base URL `/`.

* __Note:__ Be sure to not have a static file with the same name as a file in the `pages/` directory, as this will result in an error.

* __Note:__ Only assets that are in the `public` directory at build time will be served by Next.js. Files added at runtime won't be available. Recommend using a third party service like AWS S3 for persistent file storage.

## Image Component and Image Optimization
* `next/image` is an extension of the HTML `<img>` element, evolved for the modern web.

* Next.js also has support for Image Optimization by default. This allows for resizing, optimizing, and serving images in modern formats like WebP when the browser supports it. 

* This avoids shipping large images to devices with a smaller viewport.

* Automatic Image Optimization works with any image source. Even if the image is hosted by an external data source, like a CMS, it can still be optimized.

* Instead of optimizing images at build time, Next.js optimizes images on-demand, as users request them.

* Images are lazy loaded by default. Page speed therefore isn't penalized for images outside the viewport. Images load as they are scrolled into viewport.

* To use a local image, import your .jpg, .png, or .webp files
* Dynamic `await import()` or `require()` are not supported. The import must be static so it can be analyzed at build time.
* Next.js will automatically determine the width and height of your image based on the imported file.
* To use a remote image, the `src` property should be a URL string, which can be relative or absolute. Because Next.js does not have access to remote files during the build process, you'll need to provide the `width`, `height` and optional `blurDataURL` props manually.

### Domains
* Sometimes you may want to access a remote image, but still use the built-in Next.js Image Optimization API. * To do this, leave the loader at its default setting and enter an absolute URL for the Image src.

* To protect your application from malicious users, you must define a list of remote domains that you intend to access this way. This is configured in your next.config.js file, as shown below:

```
module.exports = {
  images: {
    domains: ['example.com', 'example2.com'],
  },
}
```
* You should add the priority property to the image that will be the Largest Contentful Paint (LCP) element for each page. Doing so allows Next.js to specially prioritize the image for loading (e.g. through preload tags or priority hints), leading to a meaningful boost in LCP.

* The image component has several different layout modes that define how it is sized on the page. If the styling of your image isn't turning out the way you want, consider experimenting with other layout modes.

## Assets, Metadata, and CSS
* To add a title tag to a page, use the `<Head />` component.
* To add external javascript files, use the `<Script />` component.
* Next.js has built in support for a library called __styled-jsx__. It’s a “CSS-in-JS” library — it lets you write CSS within a React component, and the CSS styles will be scoped (other components won’t be affected). See `index.js` for example.
* Next.js has built-in support for CSS and Sass which allows you to import .css and .scss files and also use css modules.
* CSS Modules are useful for component-level styles. But if you want some CSS to be loaded by every page, Next.js has support for that as well.

* To load global CSS files, create a file called `pages/_app.js`
* This `App` component is the top-level component which will be common across all the different pages. You can use this App component to keep state when navigating between pages. 

## Adding Global CSS
* In Next.js, you can add global CSS files by importing them from pages/_app.js. You cannot import global CSS anywhere else.


