# Contributing to Abell

## Local Setup

- Fork [abelljs/abell](https://github.com/abelljs/abell)
- `git clone https://github.com/:github-username/abell`
- `cd abell`
- `npm install` to install dependencies
- `npm run dev:build` to build a static site!

`npm run dev:serve` to run a dev-server.

## Creating Pull Request

- Create a branch with name of feature you are working on. (e.g. `feat-abell-config`, `fix-serve-fails`, etc)
- Make changes in your locally cloned fork
- Send Pull Request from your branch to main branch.

## Detailed Guide to Code

Abell is a static-site-generator so you can think of this as a script that

1. Takes data from markdown files, and JSON files
2. Applies that data into a layout
3. Renders a static HTML file.

Abell shares 2 repositories

- [abelljs/abell](https://github.com/abelljs/abell)
  Static site generator (reading markdown, adding predefined variables, etc.)
- [abelljs/abell-renderer](https://github.com/abelljs/abell-renderer)
  0 dependency template engine (renders `.abell` files and outputs `.html` file, executes and builds JavaScript written inside brackets)

### - [abelljs/abell-renderer](https://github.com/abelljs/abell-renderer)

In the code of [abelljs/abell](https://github.com/abelljs/abell), you will see code that looks like,

```js
const abellRenderer = require('abell-renderer');
const abellTemplate = `<body>\{{ 3 + 3 }}</body>`;

const htmlTemplate = abellRenderer.render(abellTemplate, {});
// htmlTemplate carries value:
// <body>6</body>
```

Function `abellRenderer.render(abellTemplate, sandbox, options)` executes the JavaScript inside brackets, and renders and outputs HTML content.

Full documentation at: [https://github.com/abelljs/abell-renderer#-javascript-api](https://github.com/abelljs/abell-renderer#-javascript-api)

### - [abelljs/abell](https://github.com/abelljs/abell)

This repository contains the code for static-site-generation. As you can see above that abell-renderer allows us to pass sandbox (an environment/variables, functions, etc.).

In static-site-generation, we need some variables like a variable that gives us the list and details of blogs, a function that lets us import markdown content, a variable to give a path to current blog. Abell provides all these variables and loops over a layout (`[$path]/index.abell` file) to dynamically generate blogs from a single layout. (See [src/content-generator.js](https://github.com/abelljs/abell/blob/main/src/content-generator.js))

Additionally, if you're developing a website, you need a dev-server that reloads when you change the code. Abell's dev server is written from scratch to provide quick reloads. The code related to reloading dev server is at [src/abell-dev-server/](https://github.com/abelljs/abell/blob/main/src/abell-dev-server/) and the code that decides what to reload is at [src/serve.js](https://github.com/abelljs/abell/blob/main/src/serve.js)

Throughout the code you will see a variable called `programInfo` being passed to all the functions. programInfo has all the information related to building a site (directories in the `content`, meta informations of all blogs, etc)

The entry point of the code is [src/index.js](https://github.com/abelljs/abell/blob/main/src/index.js) which on `abell build` calls [src/build.js](https://github.com/abelljs/abell/blob/main/src/build.js) and on `abell serve` calls [src/serve.js](https://github.com/abelljs/abell/blob/main/src/serve.js)

_Note: In most cases, you will not have to understand the whole code and you will mostly need to care about the file related to changes you're making, so even if you don't understand the whole code, that's completely normal and ok_

Thank you and if you need any additional help, you can ask in our [Slack Channel](https://join.slack.com/t/abellland/shared_invite/zt-ebklbe8h-FhRgHxNbuO_hvFDf~nZtGQ) or reach out to me on [Twitter @saurabhcodes](https://twitter.com/saurabhcodes)
