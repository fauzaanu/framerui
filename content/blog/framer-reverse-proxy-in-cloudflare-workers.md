---
author: Fauzaan Gasim
title: Reverse Proxying Framer through cloudflare workers
date: 2023-04-06
description: Learn how to reverse proxy your Framer site through Cloudflare Workers, enabling you to serve your Framer-powered content directly from the edge.
tags: [Framer, Cloudflare Workers, reverse proxy, web development, edge computing]
thumbnail: "/techs/workers.png"
---

## Understanding Reverse Proxying with Cloudflare Workers

Reverse proxying is a powerful technique that allows you to serve your Framer-powered content directly from the edge, using a service like Cloudflare Workers. By setting up a reverse proxy, you can improve the performance and reliability of your Framer site, as the content is delivered closer to your users, reducing latency and improving the overall user experience.

## Setting up the Reverse Proxy with Cloudflare Workers

1. **Create a Cloudflare Workers Account**: If you haven't already, sign up for a Cloudflare Workers account. Cloudflare offers a free plan that is suitable for many use cases.

2. **Create a New Cloudflare Worker**: In the Cloudflare Workers dashboard, create a new worker by clicking on the "Create a Worker" button.

3. **Configure the Worker**: In the worker editor, you'll need to write the code that will handle the reverse proxying. Here's an example:

```javascript
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url)
  const isFramerRequest = url.hostname.endsWith('.framer.website')

  if (isFramerRequest) {
    const response = await fetch(`https://${url.hostname}${url.pathname}`, {
      headers: {
        'Content-Type': 'text/html',
        'Cache-Control': 'max-age=3600' // Cache the response for 1 hour
      }
    })
    return response
  } else {
    return new Response('Not a Framer request', { status: 404 })
  }
}
```

This code listens for incoming requests and checks if the request is for a Framer-powered site (i.e., the hostname ends with `.framer.website`). If it is, the worker forwards the request to the Framer site and returns the response. If it's not a Framer request, the worker returns a 404 response.

4. **Deploy the Worker**: Once you've written the code, deploy the worker by clicking the "Deploy" button in the Cloudflare Workers dashboard.

## Benefits of Reverse Proxying Framer through Cloudflare Workers

- **Improved Performance**: By serving your Framer content from the Cloudflare edge network, you can reduce latency and improve the overall performance of your site.

- **Increased Reliability**: Cloudflare's global network and robust infrastructure can help ensure that your Framer site remains available and responsive, even during periods of high traffic.

- **Enhanced Security**: Cloudflare Workers can help protect your Framer site from various security threats, such as DDoS attacks, by handling the traffic at the edge.

- **Simplified Deployment**: With Cloudflare Workers, you can easily deploy and manage your reverse proxy without the need for additional infrastructure or server management.

## Enhancing Reverse Proxying with Caching

In addition to seamlessly integrating content from any source into your web application, it's crucial to optimize the performance of your reverse proxy setup. One effective way to do this is by implementing caching. This ensures that once a design or content is loaded, subsequent requests for the same content are served from the cache, reducing the load on the original servers and improving the response time for your users.