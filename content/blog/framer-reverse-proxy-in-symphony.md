---
author: Fauzaan Gasim
title: Reverse Proxying Framer through Symfony (PHP)
date: 2023-04-06
description: Learn how to reverse proxy your Framer designs and prototypes through a Symfony backend, enabling you to seamlessly integrate your designs into a PHP-based web application.
tags: [Framer, Symfony, reverse proxy, PHP, web development, prototyping]
thumbnail: "/techs/symphony.jpg"
---

## Reverse Proxying Framer with Symfony

Integrating your Framer designs and prototypes into a web application can be a seamless process when you leverage the power of reverse proxying. In this article, we'll explore how to set up a reverse proxy using the Symfony PHP framework, allowing you to serve your Framer-based content directly from your backend.

## Prerequisites

Before we begin, ensure that you have the following set up:

1. **Framer Project**: You'll need an existing Framer project that you want to integrate into your Symfony application.
2. **Symfony Installation**: Make sure you have Symfony installed and configured on your development environment.

## Setting up the Reverse Proxy

1. **Create a Symfony Controller**: In your Symfony application, create a new controller that will handle the reverse proxy functionality. This controller will receive the request from the client, forward it to Framer, and then return the response back to the client.

```php
// src/Controller/FramerProxyController.php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Contracts\HttpClient\HttpClientInterface;

class FramerProxyController extends AbstractController
{
    private $httpClient;

    public function __construct(HttpClientInterface $httpClient)
    {
        $this->httpClient = $httpClient;
    }

    public function proxy(Request $request): Response
    {
        $framerId = $request->get('framerId');
        $url = 'https://framer.com/api/v1/projects/' . $framerId . '/publish';

        $response = $this->httpClient->request('GET', $url, [
            'headers' => [
                'Accept' => 'application/json',
            ],
        ]);

        $content = $response->getContent();

        return new Response($content, $response->getStatusCode(), $response->getHeaders());
    }
}
```

2. **Configure the Route**: In your Symfony application's routing configuration, add a route that will map to the `FramerProxyController::proxy` action.

```yaml
# config/routes.yaml
framer_proxy:
    path: /framer/{framerId}
    controller: App\Controller\FramerProxyController::proxy
```

3. **Update Your Framer Project**: In your Framer project, update the `publish` URL to point to the route you just created in Symfony.

```javascript
// Framer project
const publishUrl = 'https://your-symfony-app.com/framer/your-framer-project-id';
```

## Using the Reverse Proxy

1. **Access Your Framer Project**: When a user visits the URL corresponding to your Symfony route (e.g., `https://your-symfony-app.com/framer/your-framer-project-id`), the request will be forwarded to the Framer API, and the response will be returned to the user.

2. **Integrate with Your Symfony Application**: You can now seamlessly integrate your Framer-based content into your Symfony application. The reverse proxy will handle the communication between your Symfony app and the Framer API, allowing you to present your designs and prototypes within your web application.

## Benefits of Reverse Proxying Framer through Symfony

1. **Improved Security**: By using a reverse proxy, you can add an additional layer of security to your Framer-based content. The Symfony application can handle authentication, authorization, and other security-related concerns before forwarding the request to Framer.

2. **Customization and Integration**: Reverse proxying allows you to customize the integration between Framer and your Symfony application. You can add additional functionality, such as caching, logging, or middleware, to enhance the user experience.

3. **Centralized Deployment**: By hosting your Framer-based content through your Symfony application, you can manage the deployment and hosting of your entire web application from a single platform.

## Enhancing Reverse Proxying with Caching

In addition to seamlessly integrating content from any source into your web application, it's crucial to optimize the performance of your reverse proxy setup. One effective way to do this is by implementing caching. This ensures that once a design or content is loaded, subsequent requests for the same content are served from the cache, reducing the load on the original servers and improving the response time for your users.