---
author: Fauzaan Gasim
title: How to reverse proxy framer through Fiber (GO)
date: 2023-04-06
description: Learn how to reverse proxy your Framer application through the Fiber Go framework, enabling you to serve your Framer site within a single request-response cycle.
tags: [Framer, Fiber, Go, reverse proxy, web development]
---

## Reverse Proxying Framer with Fiber (Go)

Framer is a powerful design and prototyping tool that allows you to create interactive and dynamic user interfaces. However, when it comes to deploying your Framer-based application, you may want to consider using a reverse proxy to serve your Framer site within a single request-response cycle. This can be achieved by leveraging the Fiber Go framework, a fast and efficient web framework for building modern web applications.

## Setting up the Fiber Reverse Proxy

1. **Install Fiber**: Start by installing the Fiber Go framework. You can do this by running the following command in your terminal:

   ```
   go get -u github.com/gofiber/fiber/v2
   ```

2. **Create a new Fiber application**: In your Go project, create a new file (e.g., `main.go`) and initialize a new Fiber application:

   ```go
   package main

   import (
   	"github.com/gofiber/fiber/v2"
   )

   func main() {
   	app := fiber.New()

   	// Add your Framer reverse proxy logic here

   	app.Listen(":3000")
   }
   ```

3. **Implement the Framer Reverse Proxy**: To reverse proxy your Framer application, you'll need to create a middleware function that will handle the request and response. Here's an example:

   ```go
   package main

   import (
   	"fmt"
   	"io/ioutil"
   	"net/http"

   	"github.com/gofiber/fiber/v2"
   )

   func main() {
   	app := fiber.New()

   	app.Use("/", func(c *fiber.Ctx) error {
   		// Fetch the Framer site
   		resp, err := http.Get("https://your-framer-site.com")
   		if err != nil {
   			return c.Status(http.StatusInternalServerError).SendString(err.Error())
   		}
   		defer resp.Body.Close()

   		// Read the response body
   		body, err := ioutil.ReadAll(resp.Body)
   		if err != nil {
   			return c.Status(http.StatusInternalServerError).SendString(err.Error())
   		}

   		// Set the appropriate headers
   		c.Set("Content-Type", resp.Header.Get("Content-Type"))

   		// Send the response back to the client
   		return c.Send(body)
   	})

   	app.Listen(":3000")
   }
   ```

   In this example, the middleware function fetches the Framer site, reads the response body, and then sends the response back to the client with the appropriate headers.

4. **Run the Fiber application**: Start your Fiber application by running the following command in your terminal:

   ```
   go run main.go
   ```

   Your Framer application will now be available at `http://localhost:3000`.

## Benefits of Reverse Proxying Framer with Fiber

- **Single Request-Response Cycle**: By reverse proxying your Framer application through Fiber, you can serve the entire Framer site within a single request-response cycle, improving the user experience and reducing latency.

- **Unified Backend**: Integrating your Framer application with a backend framework like Fiber allows you to create a more cohesive and unified web application, where the design and functionality are seamlessly combined.

- **Flexibility**: Fiber's middleware system provides a flexible and extensible way to handle the reverse proxy logic, allowing you to customize the behavior as needed.

- **Performance**: Fiber is a fast and efficient Go web framework, which can help improve the overall performance of your reverse-proxied Framer application.

By following the steps outlined in this guide, you can successfully reverse proxy your Framer application through the Fiber Go framework, creating a more integrated and efficient web experience for your users.