<img src="https://reactql.org/reactql/logo.svg" alt="ReactQL" width="278" height="77" />

![license](https://img.shields.io/github/license/leebenson/reactql.svg?style=flat-square) [![Twitter Follow](https://img.shields.io/twitter/follow/reactql.svg?style=social&label=Follow)](https://twitter.com/reactql)
[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/leebenson/reactql)

Universal front-end React + GraphQL starter kit, written in Typescript.

https://reactql.org

## Features

### Front-end stack

- [React v16](https://facebook.github.io/react/) for UI.
- [Apollo Client 2.0 (React)](http://dev.apollodata.com/react/) for connecting to GraphQL.
- [MobX](https://mobx.js.org/) for declarative, type-safe flux/store state management (automatically re-hydrated from the server.)
- [Emotion](https://emotion.sh/) CSS-in-JS, with inline `<style>` tag generation that contains only the CSS that needs to be rendered.
- [Sass](https://sass-lang.com/), [Less](http://lesscss.org/) and [PostCSS](https://postcss.org/) when importing `.css/.scss/.less` files.
- [React Router 4](https://github.com/ReactTraining/react-router/tree/v4) for declarative browser + server routes.

- Declarative/dynamic `<head>` section, using [react-helmet](https://github.com/nfl/react-helmet).

### Server-side rendering

- Built-in [Koa 2](http://koajs.com/) web server, with async/await routing.
- Full route-aware server-side rendering (SSR) of initial HTML.
- Universal building - both browser + Node.js web server compile down to static files, for fast server re-spawning.
- Per-request GraphQL store. Store state is dehydrated via SSR, and rehydrated automatically on the client.
- MobX for app-wide flux/store state, with a built-in `<StateConsumer>` for automatically re-rendering any React component that 'listens' to state and full client-side rehydration. Fully typed state!
- Full page React via built-in SSR component - every byte of your HTML is React.
- SSR in both development and production, even with hot-code reload.

### Real-time

- Hot code reloading; zero refresh, real-time updates in development.
- Development web server that automatically sends patches on code changes, and restarts the built-in Web server for SSR renders that reflect what you'd see in production.
- WebSocket `subscription` query support for real-time data (just set `WS_SUBSCRIPTIONS=1` in [.env](.env))

### Code optimisation

- [Webpack v4](https://webpack.js.org/), with [tree shaking](https://webpack.js.org/guides/tree-shaking/) -- dead code paths are automatically eliminated.
- Asynchronous code loading when `import()`'ing inside a function.
- Automatic per-vendor chunk splitting/hashing, for aggressive caching (especially good behind a HTTP/2 proxy!)
- Gzip/Brotli minification of static assets.
- CSS code is combined, minified and optimised automatically - even if you use SASS, LESS and CSS together!

### Styles

- [Emotion](https://emotion.sh/), for writing CSS styles inline and generating the minimal CSS required to properly render your components.
- [PostCSS v7](http://postcss.org/) with [next-gen CSS](https://preset-env.cssdb.org/) and automatic vendor prefixing when importing `.css`, `.scss` or `.less` files.
- [SASS](http://sass-lang.com) and [LESS](http://lesscss.org/) support (also parsed through PostCSS.)
- Automatic vendor prefixing - write modern CSS, and let the compiler take care of browser compatibility.
- Mix and match SASS, LESS and regular CSS - without conflicts!
- CSS modules - your classes are hashed automatically, to avoid namespace conflicts.
- Compatible with Foundation, Bootstrap, Material UI and more. Simply configure via a `.global.(css|scss|less)` import to preserve class names.

### Production-ready

- Production bundling via `npm run production`, that generates optimised server and client code.
- [Static compression](https://webpack.js.org/plugins/compression-webpack-plugin/) using the Gzip and [Brotli](https://opensource.googleblog.com/2015/09/introducing-brotli-new-compression.html) algorithms for the serving of static assets as pre-compressed `.gz` and `.br` files (your entire app's `main.js.bz` - including all dependencies - goes from 346kb -> 89kb!)
- Static bundling via `npm run static`. Don't need server-side rendering? No problem. Easily deploy a client-only SPA to any static web host (Netlify, etc.)

### Developer support

- Written in [Typescript](https://www.typescriptlang.org/) with full type support, out the box (all external `@types/*` packages installed)
- Heavily documented code

## Quick start

Grab and unpack the latest version, install all dependencies, and start a server:

```
wget -qO- https://github.com/leebenson/reactql/archive/4.0.0.tar.gz | tar xvz
cd reactql-4.0.0
npm i
npm start
```

Your development server is now running on [http://localhost:3000](http://localhost:3000)

## Development mode

Development mode offers a few useful features:

- Hot code reloading. Make a change anywhere in your code base (outside of the Webpack config), and changes will be pushed down the browser automatically - without page reloads. This happens for React, Emotion, SASS - pretty much anything.

- Full source maps for Javascript and CSS.

- Full server-side rendering, with automatic Koa web server restarting on code changes. This ensures the initial HTML render will always reflect your latest code changes.

To get started, simply run:

```
npm start
```

A server will be started on [http://localhost:3000](http://localhost:3000)

## Production mode

In production mode, the following happens:

- All assets are optimised and minified. Javascript, CSS, images, are all compiled down to static files that will appear in `dist`.

- Assets are also compressed into `.gz` (Gzip) and `.br` (Brotli) versions, which are served automatically to all capable browsers.

- If files have been generated in a previous run, they will be re-used on subsequent runs. This ensures really fast server start-up times after the initial build.

To build and run for production, use:

```
npm run production
```

Files will be generated in `./dist`, and a server will also be spawned at [http://localhost:3000](http://localhost:3000)

Clean the cached production build with `npm run clean`, or run `npm run clean-production` to both clean and re-run the production build, as needed.

# Build mode

If you only want to build assets and not actually run the server, use:

```
npm run build
```

This is used in the [Dockerfile](Dockerfile), for example, to pre-compile assets and ensure faster start-up times when spawning a new container.

## Project layout

The important stuff is in [src](src).

Here's a quick run-through of each sub-folder and what you'll find in it:

- [src/components](src/components) - React components. Follow the import flow at [root.tsx](src/components/root.tsx) to figure out the component render chain. I've included an [example](src/components/example) component that shows off some Apollo GraphQL and MobX features, including incrementing a local counter and pulling top news stories from Hacker News (a live GraphQL server endpoint.)

- [src/data](src/data) - Data used throughout your app. You'll find [routes.ts](src/data/routes.ts), which defines your React Router routes (currently, just the home page -- but you can easily extend this.) and [state.ts](src/data/state.ts), to show you how simple it is to define your own state data fields that, when modified, automatically re-render any 'observing' component.

- [src/entry](src/entry) - The client and server entry points, which call on [src/components/root.tsx](src/components/root.tsx) to isomorphically render the React chain in both environments.

- [src/global](src/global) - A good place for anything that's used through your entire app, like global styles. I've started you off with a [styles.ts](src/global/styles.ts) that sets globally inlined Emotion CSS, as well as pulls in a global `.scss` file -- to show you how both types of CSS work.

- [src/lib](src/lib) - Internal libraries/helpers. There's an [apollo.ts](src/lib/apollo.ts) which builds a universal Apollo Client, and [mobx.ts](src/graphql/mobx.ts) which sets up default state (automatically rehydrated on the client), for incrementing a local counter. Plus, functions to handle hot-code reloading, Webpack stats helpers (used by the server to automatically load the right `<script>` tags, and some internal stuff to help with hot-code reloading the server when code changes in developmnt.

- [src/queries](src/queries) - Your GraphQL queries. There's just one by default - for pilling the top stories from Hacker News to display in the example component.

- [src/runner](src/runner) - Development and production runners that spawn the Webpack build process in each environment.

- [src/views](src/views) - View components that fall outside of the usual React component chain, for use on the server. In here, [ssr.tsx](src/views/ssr.tsx) takes care of rendering the root HTML that's sent down the wire to the client. Note this is also a React component - your whole app will render as React! - and [static.html](src/views/static.html) serves as a template for rendering a client-side SPA. Update it as needed.

- [src/webpack](src/webpack) - The Webpack 4 configuration files that do the heavy lifting to transform our Typescript code, images and CSS into optimised and minified assets that wind up in the `dist` folder at the root. Handles both the client and server environments.

You'll also find some other useful goodies in the [root]()...

- [.env](.env) - Change your `GRAPHQL` server endpoint, and `WS_SUBSCRIPTIONS=1` for built-in WebSocket support.

- [.nvmrc](.nvmrc) - Specify your preferred Node.js version, for use with NVM and used by many continuous deployment tools. Defaults to v11.8.0

- [netlify.toml](netlify.toml) - Build instructions for fast [Netlify](https://www.netlify.com/) deployments. **Tip: To quickly deploy a demo ReactQL app, [click here](https://app.netlify.com/start/deploy?repository=https://github.com/leebenson/reactql).**

- [types](types) - Some basic types that allow you to import fonts, images, CSS/SASS/LESS files, and allow use of the global `SERVER` boolean in your IDE.

- Typescript configuration via [tsconfig.json](tsconfig.json)

- A sample multi-build [Dockerfile](Dockerfile) based on Node 11.8 and Alpine, for quickly deploying your code base to production.

# Follow @reactql for updates

Get the latest updates by following us on Twitter: https://twitter.com/reactql

[![Twitter Follow](https://img.shields.io/twitter/follow/reactql.svg?style=social&label=Follow)](https://twitter.com/reactql)

# New to GraphQL? Need help?

<img src="https://reactql.org/assets/ext/slack_mark.png" alt="ReactQL" width="70" />

[Join the ReactQL slack channel here.](https://join.slack.com/t/reactql/shared_invite/enQtMjU0MjUzNDEzNzY0LWIyY2MzOGNlYmE1ZjI5ZDZhZTI2ODdiYzM2NjczYzJhZDgxYmJmYzE1NDYzZjRkYmVmNmQ3MzM0NzM3N2M5ODM)

Watch my free [45 minute YouTube video](https://www.youtube.com/watch?v=DNPVqK_woRQ), for a live coding walk-through of putting together a GraphQL server with a database. Learn how to write queries, mutations and handle nested/related data.

# Hire me

I'm a full-stack developer with 20+ years experience. As well as 9 years hands-on dev with Node.js, I'm fluent in Python, Go, SQL and NoSQL. I specialise in building robust, scalable products from scratch, and helping you deploy fast.

If you're looking for a senior developer who can help you get your product out the door quickly, reach me at [lee@leebenson.com](mailto:lee@leebenson.com). I'm occasionally available to take on remote contracts when I'm not working on my own projects.
