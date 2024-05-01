---
author: Fauzaan Gasim
title: Reverse Proxying Framer Through ASP.NET
date: 2023-04-06
description: Learn how to reverse proxy your Framer designs and prototypes through an ASP.NET backend, enabling you to seamlessly integrate your interactive content into your web application.
tags: [Framer, reverse proxy, ASP.NET, web development, prototyping]
thumbnail: "/techs/workers.png"
---

## Understanding Reverse Proxying with Framer

Reverse proxying is a powerful technique that allows you to serve your Framer-based content through a backend framework, in this case, ASP.NET. By using a reverse proxy, you can integrate your Framer designs and prototypes directly into your web application, providing a seamless user experience.

## Setting up the Reverse Proxy

1. **Create an ASP.NET Project**: Begin by creating a new ASP.NET project in your preferred development environment.

2. **Install the Necessary Packages**: You'll need to install the necessary packages to enable the reverse proxy functionality. In your ASP.NET project, add the following packages:
   - `Microsoft.AspNetCore.Proxy`
   - `Microsoft.AspNetCore.StaticFiles`

3. **Configure the Reverse Proxy**: In your ASP.NET project's `Startup.cs` file, add the following code to configure the reverse proxy:

   ```csharp
   public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
   {
       // ...

       app.UseStaticFiles();

       app.UseRouting();

       app.UseEndpoints(endpoints =>
       {
           endpoints.MapReverseProxy(config =>
           {
               config.RoutePrefix = "framer";
               config.ProxyPass = "https://framer.com";
           });

           endpoints.MapDefaultControllerRoute();
       });
   }
   ```

   This configuration sets up the reverse proxy to handle requests to the `/framer` route and forward them to the Framer website.

4. **Serve Framer Content**: In your ASP.NET project, create a controller or action method that will serve the Framer content. For example:

   ```csharp
   public IActionResult Index()
   {
       return Redirect("/framer");
   }
   ```

   This action method will redirect the user to the `/framer` route, which will be handled by the reverse proxy.

## Integrating Framer into Your ASP.NET Application

1. **Embed Framer Content**: Within your ASP.NET views or layouts, you can embed the Framer content using an `iframe` or by rendering the Framer code directly. For example:

   ```html
   <iframe src="/framer" frameborder="0"></iframe>
   ```

   This will embed the Framer content within your ASP.NET application.

2. **Customize the Integration**: Depending on your requirements, you can further customize the integration between Framer and your ASP.NET application. This may include passing data between the two, handling user interactions, or integrating Framer's prototyping features with your application's functionality.

## Benefits of Reverse Proxying Framer through ASP.NET

- **Seamless Integration**: By reverse proxying Framer through ASP.NET, you can seamlessly integrate your interactive Framer designs and prototypes into your web application, providing a cohesive user experience.

- **Improved Security**: The reverse proxy acts as an intermediary, shielding your ASP.NET application from direct exposure to the Framer platform, enhancing the overall security of your web application.

- **Centralized Control**: Hosting your Framer content through an ASP.NET backend gives you more control over the delivery, caching, and management of your interactive content.

- **Expanded Functionality**: Combining Framer's prototyping capabilities with the robust features of ASP.NET allows you to create more sophisticated and feature-rich web applications.

## Enhancing Reverse Proxying with Caching

In addition to seamlessly integrating content from any source into your web application, it's crucial to optimize the performance of your reverse proxy setup. One effective way to do this is by implementing caching. This ensures that once a design or content is loaded, subsequent requests for the same content are served from the cache, reducing the load on the original servers and improving the response time for your users.