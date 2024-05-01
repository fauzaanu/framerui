---
author: Fauzaan Gasim
title: Reverse Proxying Framer through Falcon (Python)
date: 2023-04-06
description: Learn how to reverse proxy your Framer designs and prototypes through the Falcon Python web framework, enabling you to serve your Framer content within a single request-response cycle.
tags: [Framer, Falcon, reverse proxy, Python, web development, prototyping]
thumbnail: "/techs/falcon.png"
---

## Understanding Reverse Proxying with Framer

Reverse proxying is a powerful technique that allows you to serve your Framer designs and prototypes through a backend framework, in this case, the Falcon Python web framework. By using a reverse proxy, you can seamlessly integrate your Framer content into your web application, providing a unified user experience.

## Setting up the Falcon Reverse Proxy

1. **Install Falcon**: Begin by installing the Falcon web framework. You can do this using pip:

   ```
   pip install falcon
   ```

2. **Create a Falcon Application**: In your Python file, import the necessary modules and create a Falcon application instance:

   ```python
   import falcon
   import requests

   app = falcon.App()
   ```

3. **Define the Reverse Proxy Route**: Create a resource class that will handle the reverse proxy functionality:

   ```python
   class FramerProxy:
       def on_get(self, req, resp):
           # Fetch the Framer content
           framer_url = 'https://your-framer-project.framer.website'
           response = requests.get(framer_url)

           # Set the response headers
           resp.content_type = response.headers['Content-Type']
           resp.status = falcon.HTTP_200

           # Return the Framer content
           resp.text = response.text
   ```

4. **Mount the Reverse Proxy Route**: Add the reverse proxy route to your Falcon application:

   ```python
   app.add_route('/framer', FramerProxy())
   ```

Now, when a user visits the `/framer` route in your Falcon application, the request will be proxied to your Framer project, and the response will be served back to the user.

## Customizing the Reverse Proxy

You can further customize the reverse proxy behavior to suit your needs. For example, you can:

- **Handle different HTTP methods**: Extend the `FramerProxy` class to handle other HTTP methods, such as `POST`, `PUT`, or `DELETE`.
- **Modify the response headers**: Adjust the response headers to fit your application's requirements, such as adding CORS headers or caching directives.
- **Implement error handling**: Add error handling to gracefully handle any issues that may arise during the reverse proxy process.

## Enhancing Reverse Proxying with Caching

In addition to seamlessly integrating content from any source into your web application, it's crucial to optimize the performance of your reverse proxy setup. One effective way to do this is by implementing caching. This ensures that once a design or content is loaded, subsequent requests for the same content are served from the cache, reducing the load on the original servers and improving the response time for your users.