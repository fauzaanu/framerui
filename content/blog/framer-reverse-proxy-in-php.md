---
author: Fauzaan Gasim
title: How to Reverse Proxy Framer Through Vanilla PHP
date: 2023-04-06
description: Learn how to reverse proxy your Framer designs and prototypes through a vanilla PHP backend, enabling you to seamlessly integrate your designs into your web application.
tags: [Framer, reverse proxy, PHP, web development, prototyping]
---

## Understanding Reverse Proxying with Framer

Reverse proxying is a powerful technique that allows you to serve your Framer-based designs and prototypes through a backend framework, in this case, vanilla PHP. By using a reverse proxy, you can integrate your Framer-powered components directly into your web application, providing a seamless user experience.

## Setting up the Reverse Proxy

1. **Create a PHP Backend**: Start by setting up a basic PHP backend that will serve as the reverse proxy. You can use a simple PHP script or a more robust framework like Laravel or Symfony.

2. **Configure the Reverse Proxy**: In your PHP script, you'll need to set up the reverse proxy functionality. This typically involves forwarding the incoming request to the Framer server and then returning the response back to the client.

   ```php
   <?php
   // Set the URL of the Framer server
   $framer_url = 'https://framer.com';

   // Forward the request to the Framer server
   $response = file_get_contents($framer_url . $_SERVER['REQUEST_URI']);

   // Return the response to the client
   echo $response;
   ?>
   ```

3. **Handle Dynamic Content**: If your Framer project includes dynamic content or data, you'll need to ensure that the reverse proxy can handle these requests. This may involve making additional API calls or integrating with your application's data sources.

4. **Optimize Performance**: To improve performance, you can consider implementing caching mechanisms, such as using a content delivery network (CDN) or implementing server-side caching in your PHP backend.

## Benefits of Reverse Proxying Framer

1. **Seamless Integration**: By reverse proxying Framer through your PHP backend, you can seamlessly integrate your Framer-based designs and prototypes into your web application, creating a cohesive user experience.

2. **Improved Security**: Reverse proxying can help improve the security of your application by adding an additional layer of protection between your Framer project and the public-facing web server.

3. **Flexibility**: Reverse proxying allows you to customize the behavior of your Framer-based components, such as adding server-side logic or integrating with your application's data sources.

4. **Scalability**: By offloading the Framer server requests to your PHP backend, you can potentially improve the scalability of your application, especially if you have a high volume of traffic.

## Conclusion

Reverse proxying Framer through a vanilla PHP backend is a powerful technique that can help you integrate your Framer-based designs and prototypes into your web application. By following the steps outlined in this guide, you can set up a reverse proxy and enjoy the benefits of seamless integration, improved security, and enhanced flexibility. As you continue to develop your web application, consider leveraging the power of reverse proxying to create a cohesive and engaging user experience.