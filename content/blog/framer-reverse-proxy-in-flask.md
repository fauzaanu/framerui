---
author: Fauzaan Gasim
title: How to Reverse Proxy Framer Through Flask
date: 2023-04-06
description: Learn how to reverse proxy your Framer application through a Flask backend, enabling you to serve your Framer site within a single request-response cycle.
tags: [Framer, Flask, reverse proxy, web development, prototyping]
---

## Understanding Reverse Proxying with Flask

Reverse proxying is a technique where a backend server, in this case, Flask, acts as an intermediary between the client and the Framer application. This allows you to serve the Framer site within a single request-response cycle, providing a seamless user experience.

## Setting up the Flask Reverse Proxy

1. **Install Flask**: Begin by installing the Flask web framework. You can do this using pip:

   ```
   pip install flask
   ```

2. **Create a Flask Application**: Create a new Python file (e.g., `app.py`) and set up a basic Flask application:

   ```python
   from flask import Flask, render_template, request

   app = Flask(__name__)

   @app.route('/')
   def index():
       return render_template('index.html')

   if __name__ == '__main__':
       app.run()
   ```

3. **Serve the Framer Application**: In the `templates` directory, create an `index.html` file and include the Framer code. This can be done by copying the HTML code generated by Framer after exporting your project.

4. **Reverse Proxy the Framer Application**: To reverse proxy the Framer application, you'll need to use the `requests` library to fetch the Framer site and then serve it back to the user. Update your `app.py` file:

   ```python
   from flask import Flask, render_template, request
   import requests

   app = Flask(__name__)

   @app.route('/')
   def index():
       # Fetch the Framer site
       response = requests.get('https://your-framer-site.com')
       
       # Render the Framer site within the Flask template
       return render_template('index.html', framer_content=response.content.decode())
   
   if __name__ == '__main__':
       app.run()
   ```

   In the updated code, we use the `requests` library to fetch the Framer site and then pass the content to the `index.html` template.

5. **Update the `index.html` Template**: Modify the `index.html` template to display the Framer content:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Framer Reverse Proxy</title>
   </head>
   <body>
       {{ framer_content | safe }}
   </body>
   </html>
   ```

   The `| safe` filter ensures that the Framer content is rendered as HTML, rather than being displayed as plain text.

6. **Run the Flask Application**: Start the Flask application using the following command:

   ```
   python app.py
   ```

   Your Framer application should now be accessible through the Flask reverse proxy at `http://localhost:5000`.

## Benefits of Reverse Proxying Framer with Flask

- **Seamless User Experience**: By serving the Framer site within a single request-response cycle, you can provide a more seamless and responsive user experience.
- **Improved Security**: Reverse proxying can help improve the security of your application by adding an additional layer of protection and control.
- **Flexibility**: Using Flask as a reverse proxy allows you to integrate your Framer application with other backend services or functionality, expanding the capabilities of your overall solution.

## Conclusion

Reverse proxying Framer through Flask is a powerful technique that can enhance the user experience and integration of your Framer-based projects. By following the steps outlined in this guide, you can easily set up a Flask reverse proxy to serve your Framer application, unlocking new possibilities for your web development projects.