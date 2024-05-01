---
author: Fauzaan Gasim
title: Reverse Proxying Framer Through Ruby on Rails
date: 2023-04-06
description: Learn how to reverse proxy your Framer designs and prototypes through a Ruby on Rails backend, enabling you to seamlessly integrate your designs into a web application.
tags: [Framer, Ruby on Rails, reverse proxy, web development, prototyping]
thumbnail: "/techs/ruby.png"
---

## Understanding Reverse Proxying with Ruby on Rails

Reverse proxying is a technique where a backend framework, in this case, Ruby on Rails, acts as an intermediary between the client (user) and the Framer application. This allows you to serve the Framer-generated content through your Rails application, providing a unified and seamless user experience.

## Setting up the Reverse Proxy

1. **Create a Ruby on Rails Application**: Start by creating a new Ruby on Rails application or using an existing one.

2. **Install the Necessary Gems**: You'll need to add the following gems to your Rails application's `Gemfile`:
   - `framer-ruby`: This gem provides a convenient way to interact with the Framer API from your Rails application.
   - `httparty`: This gem simplifies making HTTP requests from your Rails application.

3. **Configure the Reverse Proxy**: In your Rails application, create a new controller that will handle the reverse proxying. You can name it something like `FramerController`.

   ```ruby
   class FramerController < ApplicationController
     def index
       # Fetch the Framer project data
       project_id = 'your_framer_project_id'
       @project_data = Framer::Project.find(project_id)

       # Render the Framer project within your Rails application
       render layout: 'framer'
     end
   end
   ```

4. **Create a Framer-specific Layout**: Create a new layout file, `framer.html.erb`, that will be used to render the Framer project within your Rails application.

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <title>Framer Project</title>
       <%= stylesheet_link_tag 'application', media: 'all' %>
       <%= javascript_pack_tag 'application' %>
     </head>

     <body>
       <%= yield %>
     </body>
   </html>
   ```

5. **Configure the Routes**: Add a route in your Rails application's `config/routes.rb` file to handle the Framer reverse proxy.

   ```ruby
   Rails.application.routes.draw do
     get 'framer', to: 'framer#index'
   end
   ```

6. **Fetch and Render the Framer Project**: In your `FramerController#index` action, use the `framer-ruby` gem to fetch the Framer project data and pass it to the `framer.html.erb` layout for rendering.

   ```ruby
   class FramerController < ApplicationController
     def index
       # Fetch the Framer project data
       project_id = 'your_framer_project_id'
       @project_data = Framer::Project.find(project_id)

       # Render the Framer project within your Rails application
       render layout: 'framer'
     end
   end
   ```

7. **Access the Framer Project**: In your `framer.html.erb` layout, you can access the Framer project data and render it using the appropriate HTML and JavaScript code.

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <title>Framer Project</title>
       <%= stylesheet_link_tag 'application', media: 'all' %>
       <%= javascript_pack_tag 'application' %>
     </head>

     <body>
       <%= @project_data.html.html_safe %>
     </body>
   </html>
   ```

## Benefits of Reverse Proxying Framer through Ruby on Rails

- **Unified User Experience**: By serving the Framer content through your Rails application, you can provide a seamless and integrated user experience, where the Framer project feels like a natural part of your web application.

- **Improved Security**: Reverse proxying allows you to handle authentication, authorization, and other security-related concerns within your Rails application, rather than exposing the Framer project directly to the internet.

- **Easier Deployment**: Deploying your Framer project becomes more straightforward, as it is integrated into your Rails application's deployment process.

- **Flexibility**: Reverse proxying gives you the ability to customize and extend the Framer project's functionality by leveraging the full power of Ruby on Rails.

## Enhancing Reverse Proxying with Caching

In addition to seamlessly integrating content from any source into your web application, it's crucial to optimize the performance of your reverse proxy setup. One effective way to do this is by implementing caching. This ensures that once a design or content is loaded, subsequent requests for the same content are served from the cache, reducing the load on the original servers and improving the response time for your users.