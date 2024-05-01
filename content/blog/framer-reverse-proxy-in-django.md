---
author: Fauzaan Gasim
title: Reverse proxying framer through django web framework
date: 2023-04-06
description: Learn how to reverse proxy your Framer designs and prototypes through the Django web framework, enabling you to seamlessly integrate your interactive content into your web application.
tags: [Framer, Django, reverse proxy, web development, prototyping]
---

## Reverse Proxying Framer with Django

Integrating Framer's interactive prototypes into a web application can be a powerful way to bring your designs to life. However, directly embedding Framer's output within your Django application can sometimes pose challenges. This is where reverse proxying comes into play, allowing you to seamlessly serve your Framer content through your Django backend.

## Setting up the Reverse Proxy

1. **Install the Necessary Dependencies**: Begin by ensuring you have the required dependencies installed in your Django project. You'll need to install the `requests` library, which will be used to make the proxied requests to Framer.

```
pip install requests
```

2. **Create a Reverse Proxy View**: In your Django application, create a new view that will handle the reverse proxying of Framer requests. This view will forward the incoming request to the Framer server and return the response back to the client.

```python
import requests
from django.http import HttpResponse

def framer_proxy(request):
    # Forward the request to the Framer server
    framer_url = 'https://framer.com/your-framer-project'
    response = requests.get(framer_url)

    # Return the Framer response to the client
    return HttpResponse(response.content, content_type=response.headers['Content-Type'])
```

3. **Configure the URL Routing**: Update your Django URL routing to map the Framer proxy view to a specific URL in your application. For example, you could use the `/framer/` endpoint.

```python
from django.urls import path
from . import views

urlpatterns = [
    path('framer/', views.framer_proxy, name='framer_proxy'),
]
```

4. **Embed the Framer Content**: In your Django templates, you can now embed the Framer content by referencing the proxy URL you've set up.

```html
<iframe src="{% url 'framer_proxy' %}" frameborder="0"></iframe>
```

## Benefits of Reverse Proxying Framer

- **Seamless Integration**: By reverse proxying Framer through your Django application, you can seamlessly integrate your interactive prototypes into your web application, creating a cohesive user experience.

- **Improved Security**: Reverse proxying allows you to control the access and security of your Framer content, as it passes through your Django backend before reaching the client.

- **Customization Opportunities**: With the reverse proxy in place, you can further customize the Framer content, such as adding authentication, modifying the response, or integrating it with other parts of your Django application.

## Enhancing Reverse Proxying with Caching

In addition to seamlessly integrating content from any source into your web application, it's crucial to optimize the performance of your reverse proxy setup. One effective way to do this is by implementing caching. This ensures that once a design or content is loaded, subsequent requests for the same content are served from the cache, reducing the load on the original servers and improving the response time for your users.
