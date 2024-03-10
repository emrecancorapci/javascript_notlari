# Intermediate React

## Low Priority Rendering

### Deferred Values `useDeferredValue`

Imagine you're doing a re-render of a large newsfeed of information and a user clicks on one of the side widgets that opens the top menu. What would you expect to happen. If a user caught the exact moment that the big newsfeed was re-rendering, it could cause a moment of jank: two big things are happening at the exact same. For most of us that is an acceptable risk and one we wouldn't pursue to solve any further unless it was a massive issue. For Facebook, it is a big issue. Lots of stuff is rendering and users can cause their site to feel janky with their clicking. So Facebook made transitions.

```jsx
function Main(){
  const deferredPosts = useDeferredValue(posts);
  const renderedPosts = useMemo(() => <PostsView posts={deferredPosts}>, [deferredPosts]);

  return (
    <div>
      {renderedPosts}
    </div>
  );
} 
```

### Transition `useTransition`

We have seen how to defer lesser important updates but let's talk specifically about transition states. More often than not this is a loading state e.g. a user clicked a submit button and now we need to hit the API and wait for the API to say "here is the results". What we would have done previously (and did do in the Intro) is have a useState piece of state that keeps track of a isLoading flag.

What's wrong with this? These would be all "high priority" transitions for React. Therefore, it will try to do it as fast and as soon as it can, but it ends up being not a big deal because we can defer showing a loading state until everything else is done (in the name of keeping the UI responsive). This is what useTransition is good for.

```jsx
function Form() {
  const [isPending, startTransition] = useTransition();

  const onSubmit = (e) => {
    e.preventDefault();
    startTransition(() => {
      // do something that takes a long time
    });
  };

  return (<>
    <form onSubmit={onSubmit}>
      […]
      {isPending ? (
        <h2>Loading...</h2>
      ) : (
        <button>Submit</button>
      )}
    </form>
  </>)
}
```

## Advanced React Performance

### Code Splitting

Code splitting is essential to having small application sizes, particularly with React. React is already forty-ish kilobytes just for the framework. This isn't huge but it's enough that it will slow down your initial page loads (by up to a second and a half on 2G speeds.) If you have a lot third party libraries on top of that, you've sunk yourself before they've even started loading your page.

Enter code splitting. This allows us to identify spots where our code could be split and let Vite do its magic in splitting things out to be loaded later. An easy place to do this would be at the route level. So let's try that first.

```jsx
const LoginForm = React.lazy(() => import("./LoginForm"));

[...]

<LoginForm>
```

### Server Side Rendering

Server side rendering is a way to render your React application on the server and send the HTML to the client. This is a great way to improve the performance of your application. It can be a bit tricky to set up, but it's worth it.

#### `App.client.jsx`

```jsx
import { hydrateRoot } from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

hydrateRoot(
  document.getElementById("root"),
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

In the code above, we are using `hydrateRoot` to hydrate the root element with the application. We are also using `BrowserRouter` to wrap the application with a router.

> ***Hydration*** is the process of attaching event listeners to the server-rendered HTML. This is important for making the application interactive. Without hydration, the application would be static and not interactive. `hydrateRoot` is a function that takes a container and a React element and hydrates the container with the element.

#### `App.server.jsx`

```jsx
import { renderToPipeableStream } from "react-dom/server";
import { StaticRouter } from "react-router-dom/server";
import App from "./App";

export default function render(url, opts) {
  const stream = renderToPipeableStream(
    <StaticRouter location={url}>
      <App />
    </StaticRouter>,
    opts
  );
  return stream;
}
```

In the code above, we are using `renderToPipeableStream` to render the application to a pipeable stream. We are also using `StaticRouter` to wrap the application with a router.

> ***Pipeable streams*** are a new feature in Node.js that allows you to create streams that can be piped to other streams. This is useful for creating server-rendered HTML that can be streamed to the client. `renderPipeableStream` is a function that takes a React element and returns a pipeable stream.

#### `package.json`

```json
{
  ...
  "type": "module",
  ...
  "scripts": {
    "start": "npm run build && node server.js",
    "build": "npm run build:client && npm run build:server",
    "build:client": "vite build --outDir ../dist/client",
    "build:server": "vite build --outDir ../dist/server --ssr App.server.jsx",
  }
  ...
}
```

In the code above, we are using the `type` field to specify that we are using ECMAScript modules. We are also using the `build` script to build the client and server applications. We are using the `build:client` script to build the client application and the `build:server` script to build the server application.

#### `index.html`

```html
...
<body>
  <div id="root"><!-- Split --></div>
  <script type="module" src="/App.client.jsx"></script>
</body>
...
```

In the code above, we are using a comment to split the HTML into two parts. We are also using a script tag to load the client application.

#### `server.js`

```js
import express from "express";
import fs from "fs";
import path from "path";
import { fileURLToPath } from "url";
import renderApp from "./dist/server/App.server.js";

const __dirname = path.dirname(fileURLToPath(import.meta.url));

const PORT = process.env.PORT || 3001;

const html = fs
  .readFileSync(path.resolve(__dirname, "./dist/client/index.html"))
  .toString();

// split the HTML into two parts
const parts = html.split("<!-- Split -->");

const app = express();

// serve static assets
app.use(
  "/assets",
  express.static(path.resolve(__dirname, "./dist/client/assets"))
);

app.use((req, res) => {
  // write the first part of the HTML
  res.write(parts[0]);

  // render the application to a pipeable stream
  const stream = renderApp(req.url, {
    onShellReady() {
      // do something when the shell is ready

      // pipe the stream to the response
      stream.pipe(res);
    },
    onShellError() {
      // do error handling
    },
    onAllReady() {
      // last thing to write

      // write the second part of the HTML
      res.write(parts[1]);
      res.end();
    },
    onError(err) {
      console.error(err);
    },
  });
});
```
