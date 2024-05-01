---
author: Fauzaan Gasim
title: Reverse Proxying Framer through Koa.js
date: 2023-04-06
description: Learn how to reverse proxy your Framer designs and prototypes through Koa.js, a popular Node.js web framework, to serve your interactive content seamlessly.
tags: [Framer, Koa.js, reverse proxy, web development, prototyping]
thumbnail: "techs/koa.jpg"
---

## Reverse Proxying Framer with Koa.js

In the world of web development, reverse proxying can be a powerful technique to enhance the delivery and performance of your web applications. When it comes to Framer, a popular design and prototyping tool, leveraging a reverse proxy can provide several benefits, such as improved security, caching, and load balancing.

In this article, we'll explore how to set up a reverse proxy using Koa.js, a modern and lightweight Node.js web framework, to serve your Framer-based content.

## Prerequisites

Before we begin, ensure that you have the following set up:

1. **Node.js**: Make sure you have Node.js installed on your system. You can download the latest version from the official [Node.js website](https://nodejs.org/).
2. **Framer**: You'll need to have Framer installed and a project ready to be served through the reverse proxy.
3. **Koa.js**: We'll be using Koa.js as our reverse proxy framework, so you'll need to install it. You can do this by running `npm install koa` in your project directory.

## Setting up the Reverse Proxy

1. **Create a new Koa.js server**: Start by creating a new Koa.js server in your project. Create a new file, e.g., `server.js`, and add the following code:

```javascript
const Koa = require('koa');
const app = new Koa();
```

2. **Proxy the Framer request**: Next, we'll set up the reverse proxy to handle requests to your Framer project. Add the following code to your `server.js` file:

```javascript
const { createProxyMiddleware } = require('http-proxy-middleware');

// Proxy the Framer request
app.use(
  createProxyMiddleware('/framer', {
    target: 'https://your-framer-project.framer.website',
    changeOrigin: true,
    pathRewrite: {
      '^/framer': '/',
    },
  })
);
```

In this example, we're using the `createProxyMiddleware` function from the `http-proxy-middleware` package to set up the reverse proxy. The `'/framer'` path will be used to proxy requests to your Framer project, which is hosted at `'https://your-framer-project.framer.website'`. The `changeOrigin` option ensures that the correct host header is sent to the Framer server, and the `pathRewrite` option removes the `/framer` prefix from the URL.

3. **Serve the Framer project**: Finally, add the following code to serve your Framer project through the reverse proxy:

```javascript
// Serve the Framer project
app.use(async (ctx) => {
  await ctx.render('index', {
    title: 'My Framer Project',
  });
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

This code sets up a default route that renders the `index.html` file, which should contain your Framer project. You can customize this part to fit your specific setup.

## Benefits of Reverse Proxying Framer with Koa.js

By reverse proxying your Framer project through Koa.js, you can enjoy several benefits:

1. **Improved Security**: Koa.js can handle tasks like SSL/TLS termination, authentication, and authorization, enhancing the security of your Framer-based web application.
2. **Caching and Performance**: Koa.js can cache responses and optimize the delivery of your Framer content, leading to faster load times and improved user experience.
3. **Flexibility and Scalability**: Koa.js is a lightweight and modular framework, making it easy to integrate with other services and scale your application as needed.
4. **Unified User Experience**: By serving your Framer project through a single, unified endpoint, you can provide a seamless user experience, hiding the complexity of the underlying technologies.

## Enhancing Reverse Proxying with Caching

In addition to seamlessly integrating content from any source into your web application, it's crucial to optimize the performance of your reverse proxy setup. One effective way to do this is by implementing caching. This ensures that once a design or content is loaded, subsequent requests for the same content are served from the cache, reducing the load on the original servers and improving the response time for your users.