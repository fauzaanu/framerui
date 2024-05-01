---
author: Fauzaan Gasim
title: Reverse Proxying Framer through Gin (GO)
date: 2023-04-06
description: Learn how to reverse proxy your Framer designs and prototypes through the Gin web framework in Go, enabling you to serve your Framer content seamlessly.
tags: [Framer, Gin, Go, reverse proxy, web development, prototyping]
thumbnail: "/techs/gin.jpg"
---

## Reverse Proxying Framer with Gin

Integrating Framer, a powerful design and prototyping tool, with a backend framework can be a powerful combination for web development. In this article, we'll explore how to reverse proxy your Framer designs and prototypes through the Gin web framework in Go, allowing you to serve your Framer content seamlessly.

## Setting up the Gin Reverse Proxy

1. **Install Gin**: Start by installing the Gin web framework in your Go project. You can do this by running the following command:

   ```
   go get -u github.com/gin-gonic/gin
   ```

2. **Create a Gin Server**: In your Go code, create a new Gin server instance:

   ```go
   r := gin.Default()
   ```

3. **Configure the Reverse Proxy**: To reverse proxy your Framer content, you'll need to set up a route that will handle the requests and forward them to the Framer server. Here's an example:

   ```go
   r.GET("/", func(c *gin.Context) {
       // Specify the URL of your Framer server
       framerId := "your-framer-project-id"
       framerdUrl := fmt.Sprintf("https://framer.com/projects/%s", framerId)

       // Create a new HTTP client
       client := &http.Client{}

       // Forward the request to the Framer server
       req, err := http.NewRequest("GET", framerdUrl, nil)
       if err != nil {
           c.AbortWithStatus(http.StatusInternalServerError)
           return
       }

       // Pass along any headers from the original request
       for k, v := range c.Request.Header {
           req.Header[k] = v
       }

       // Send the request to the Framer server
       resp, err := client.Do(req)
       if err != nil {
           c.AbortWithStatus(http.StatusInternalServerError)
           return
       }
       defer resp.Body.Close()

       // Copy the response from the Framer server to the Gin response
       for k, v := range resp.Header {
           c.Writer.Header()[k] = v
       }
       c.Writer.WriteHeader(resp.StatusCode)
       _, err = io.Copy(c.Writer, resp.Body)
       if err != nil {
           c.AbortWithStatus(http.StatusInternalServerError)
           return
       }
   })
   ```

   In this example, we're creating a route that will forward any requests to the root URL (`/`) to the Framer server. You'll need to replace `"your-framer-project-id"` with the actual ID of your Framer project.

4. **Start the Gin Server**: Finally, start the Gin server and let it handle the reverse proxy requests:

   ```go
   r.Run() // listen and serve on 0.0.0.0:8080 (or any other port)
   ```

## Benefits of Reverse Proxying Framer with Gin

By reverse proxying your Framer designs and prototypes through Gin, you can enjoy several benefits:

1. **Seamless Integration**: Your Framer content will be served directly through your Gin-powered backend, providing a seamless user experience.

2. **Improved Security**: By using Gin as a reverse proxy, you can add additional security measures, such as authentication and authorization, to protect your Framer content.

3. **Flexibility**: Gin's powerful routing and middleware capabilities allow you to customize the reverse proxy behavior to fit your specific needs.

4. **Performance**: Gin is a fast and efficient web framework, which can help improve the overall performance of your Framer-powered web application.

## Enhancing Reverse Proxying with Caching

In addition to seamlessly integrating content from any source into your web application, it's crucial to optimize the performance of your reverse proxy setup. One effective way to do this is by implementing caching. This ensures that once a design or content is loaded, subsequent requests for the same content are served from the cache, reducing the load on the original servers and improving the response time for your users.