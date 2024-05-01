---
author: Fauzaan Gasim
title: Reverse Proxying Framer through Hapi.js
date: 2023-04-06
description: Learn how to reverse proxy your Framer designs and prototypes through the Hapi.js framework, enabling you to serve your Framer content within a single request-response cycle.
tags: [Framer, Hapi.js, reverse proxy, web development, prototyping]
thumbnail: "/techs/hapi.png"
---

## Understanding Reverse Proxying with Hapi.js

Reverse proxying is a powerful technique that allows you to serve content from one framework or application through another. In the context of Framer, you can use Hapi.js, a popular Node.js web framework, to reverse proxy your Framer designs and prototypes, providing a seamless user experience within a single request-response cycle.

## Setting up the Hapi.js Reverse Proxy

1. **Install Hapi.js**: Begin by installing the Hapi.js framework in your project. You can do this using npm or yarn:

   ```
   npm install @hapi/hapi
   ```

2. **Create a Hapi.js Server**: In your project, create a new file (e.g., `server.js`) and set up a basic Hapi.js server:

   ```javascript
   const Hapi = require('@hapi/hapi');

   const init = async () => {
     const server = Hapi.server({
       port: 3000,
       host: 'localhost',
     });

     await server.start();
     console.log('Server running on %s', server.info.uri);
   };

   init();
   ```

3. **Configure the Reverse Proxy**: To reverse proxy your Framer content, you'll need to add a route in your Hapi.js server that will handle the requests and forward them to Framer. Here's an example:

   ```javascript
   const Hapi = require('@hapi/hapi');
   const axios = require('axios');

   const init = async () => {
     const server = Hapi.server({
       port: 3000,
       host: 'localhost',
     });

     server.route({
       method: 'GET',
       path: '/{path*}',
       handler: async (request, h) => {
         try {
           const response = await axios.get(`https://framer.com/${request.params.path}`);
           return h.response(response.data).type(response.headers['content-type']);
         } catch (error) {
           return h.response(error.message).code(error.response?.status || 500);
         }
       },
     });

     await server.start();
     console.log('Server running on %s', server.info.uri);
   };

   init();
   ```

   In this example, we're using the `axios` library to make a GET request to the Framer site and then returning the response data to the client. The `{path*}` parameter in the route captures the entire URL path, allowing the reverse proxy to handle any request to your Framer content.

4. **Run the Hapi.js Server**: Start the Hapi.js server by running the `server.js` file:

   ```
   node server.js
   ```

## Benefits of Reverse Proxying Framer through Hapi.js

1. **Unified User Experience**: By reverse proxying your Framer content through Hapi.js, you can provide a seamless user experience where the Framer prototype is served within a single request-response cycle, without the user being aware of the underlying architecture.

2. **Improved Performance**: Reverse proxying can help improve the performance of your Framer-based application by reducing the number of network hops and eliminating the need for the client to make a separate request to the Framer site.

3. **Flexibility and Control**: Using Hapi.js as a reverse proxy allows you to add additional functionality, such as authentication, caching, or custom routing, to your Framer-based application, giving you more control over the overall user experience.

4. **Easier Deployment**: By encapsulating the Framer content within your Hapi.js server, you can simplify the deployment process and ensure that your Framer-based application is served alongside your other backend services.

## Enhancing Reverse Proxying with Caching

In addition to seamlessly integrating content from any source into your web application, it's crucial to optimize the performance of your reverse proxy setup. One effective way to do this is by implementing caching. This ensures that once a design or content is loaded, subsequent requests for the same content are served from the cache, reducing the load on the original servers and improving the response time for your users.