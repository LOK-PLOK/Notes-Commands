# Deploying app to internent

In the previous part, the frontend could ask for the list of notes from the json-server we had as a backend, from the address http://localhost:3001/notes. Our backend has a slightly different URL structure now, as the notes can be found at http://localhost:3001/api/notes. Let's change the attribute baseUrl in the frontend notes app at src/services/notes.js like so:

```javascript
import axios from 'axios'
const baseUrl = 'http://localhost:3001/api/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

// ...

export default { getAll, create, update }
```

## Same Origin Policy and CORS

When you visit a website (e.g. http://example.com), the browser issues a request to the server on which the website (example.com) is hosted. The response sent by the server is an HTML file that may contain one or more references to external assets/resources hosted either on the same server that example.com is hosted on or a different website. When the browser sees reference(s) to a URL in the source HTML, it issues a request. If the request is issued using the URL that the source HTML was fetched from, then the browser processes the response without any issues. However, if the resource is fetched using a URL that doesn't share the same origin(scheme, host, port) as the source HTML, the browser will have to check the Access-Control-Allow-origin response header. If it contains * on the URL of the source HTML, the browser will process the response, otherwise the browser will refuse to process it and throws an error.

The same-origin policy is a security mechanism implemented by browsers in order to prevent session hijacking among other security vulnerabilities.

In order to enable legitimate cross-origin requests (requests to URLs that don't share the same origin) W3C came up with a mechanism called CORS(Cross-Origin Resource Sharing). According to Wikipedia:

Cross-origin resource sharing (CORS) is a mechanism that allows restricted resources (e.g. fonts) on a web page to be requested from another domain outside the domain from which the first resource was served. A web page may freely embed cross-origin images, stylesheets, scripts, iframes, and videos. Certain "cross-domain" requests, notably Ajax requests, are forbidden by default by the same-origin security policy.

The problem is that, by default, the JavaScript code of an application that runs in a browser can only communicate with a server in the same origin. Because our server is in localhost port 3001, while our frontend is in localhost port 5173, they do not have the same origin.

Keep in mind, that same-origin policy and CORS are not specific to React or Node. They are universal principles regarding the safe operation of web applications.

We can allow requests from other origins by using Node's cors middleware.

In your backend repository, install cors with the command

```bash
npm install cors
```

#### In the backend folder
```javascript
const cors = require("cors");
app.use(cors());
```

### Dont Fly.io because it doesnt work

### Where gonna be using render 
#### -  To test the backend folder/directory you need to copy all of its contents and put it in a repository and deploy it in render


##### We notice now from the logs that the app has been started in the port 10000. The app code gets the right port through the environment variable PORT so it is essential that the file index.js has been updated in the backend as follows:

```javascript
const PORT = process.env.PORT || 3001
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```
Frontend production build
So far we have been running React code in development mode. In development mode the application is configured to give clear error messages, immediately render code changes to the browser, and so on.

When the application is deployed, we must create a **production build** or a version of the application that is optimized for production.

A production build for applications created with Vite can be created with the command **npm run build**.

Let's run this command from the root of the notes frontend project that we developed in Part 2.

This creates a directory called dist which contains the only HTML file of our application (index.html) and the directory assets. **Minified** version of our application's JavaScript code will be generated in the dist directory. Even though the application code is in multiple files, all of the JavaScript will be minified into one file. All of the code from all of the application's dependencies will also be minified into this single file.


### Serving Static files from the backend
- We begin by copying the production build of the frontend to the root of the backend. With a Mac or Linux computer, the copying can be done from the frontend directory with the command:
```bash
cp -r dist ../<Backend Folder>
```

To make Express show static content, the page index.html and the JavaScript, etc., it fetches, we need a built-in middleware from Express called **static**.

When we add the following amidst the declarations of middlewares
```javascript
app.use(express.static('dist'))
```

whenever Express gets an HTTP GET request it will first check if the dist directory contains a file corresponding to the request's address. If a correct file is found, Express will return it.

Now HTTP GET requests to the address www.serversaddress.com/index.html or www.serversaddress.com will show the React frontend. GET requests to the address www.serversaddress.com/api/notes will be handled by the backend code.

Because of our situation, both the frontend and the backend are at the same address, we can declare baseUrl as a relative URL. This means we can leave out the part declaring the server.

```javascript
import axios from 'axios'

const baseUrl = '/api/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

// ...
```

Because in development mode the frontend is at the address localhost:5173, the requests to the backend go to the wrong address localhost:5173/api/notes. The backend is at localhost:3001.

If the project was created with Vite, this problem is easy to solve. It is enough to add the following declaration to the vite.config.js file of the frontend repository.

```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],

  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:3001',
        changeOrigin: true,
      },
    }
  },
})
```