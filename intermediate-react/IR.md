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

## Redux

### Redux Toolkit Query & Middleware

`postApiService.js`

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

export const postApi = createApi({
  // The name of the slice of the store for this API
  reducerPath: "postApi",
  // A hook that returns the Redux store's state
  baseQuery: fetchBaseQuery({ baseUrl: "/api" }),
  endpoints: (builder) => ({
    // Get all posts 
    getPosts: builder.query({
      query: () => ({ url: "posts" }),
    }),
    // Get a single post
    getPost: builder.query({
      query: (id) => ({ url: `posts`, params: { id } }),
      transformResponse: (response) => response.data,
    }),
    // Add a new post
    addPost: builder.mutation({
      query: (body) => ({
        url: "posts",
        method: "POST",
        body,
      }),
    }),
  }),
});

export const { useGetPostsQuery, useGetPostQuery, useAddPostMutation } = postApi;
```

`createApi` fonksiyonuyla api servisi oluşturulur. `reducerPath` ile oluşturulan servise bir isim verilir. `baseQuery` ile api servisinin kullanacağı api adresi belirlenir. `endpoints` değerinde ise api servisinin kullanacağı endpointler yazılır.

Bu endpointler oluşturulurken `builder` fonksiyonunun içerisindeki `query` ve `mutation` fonksiyonları kullanılır. `query` fonksiyonu ile get isteği yapılırken `mutation` fonksiyonu ile post, put, delete gibi istekler yapılır.

`transformResponse` fonksiyonu ile gelen response manipüle edilebilir. Örneğin yukarıdaki örnekte `getPost` endpointi için gelen response'un içerisindeki `data` objesi alınmıştır.

`store.js`

```js
import { configureStore } from "@reduxjs/toolkit";
import { postApi } from "./postApiService";

export const store = configureStore({
  reducer: {
    [postApi.reducerPath]: postApi.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(postApi.middleware),
});
```

`postApi` servisi store'a *reducer* olarak eklendikten sonra `postApi.middleware` middleware'i de store'a eklenir. Bu middleware sayesinde api istekleri yapıldığında store güncellenir.

> `postApi.reducerPath` olarak çağrılan kısım yazdığımız api servisindeki `reducerPath: "postApi"` değeridir. İsterseniz bu değeri kendiniz de string olarak girebilirsiniz fakat bu durumda bir tarafta değişiklik yapıldığında diğer tarafta da yapılması gerektiği unutulmamalıdır.

`Details.jsx`

```jsx
import { useGetPostQuery } from "./postApiService";

function Details({ id }) {
  const { data, error, isLoading } = useGetPostQuery(id);

  if (isLoading) {
    return <div>Loading...</div>;
  }

  if (error) {
    return <div>Error: {error.message}</div>;
  }

  return (
    <div>
      <h1>{data.title}</h1>
      <p>{data.body}</p>
    </div>
  );
}
```

## Testing

`vitest` ve `happy-dom` kullanarak React uygulamaları test edilebilir.

> Neden Happy Dom?
>
> Minimal bir DOM implementasyonu olduğu için daha hızlı çalışır. Yetmediği yerlerde ise `jsdom` kullanılabilir.

`vite.config.js`

```js
import { defineConfig } from "vite";

export default defineConfig({
  plugins: [react()]
  root: "src",
  test: {
    environment: "happy-dom",
  }
});
```

Test dosyalarının `__tests__` klasörü içerisinde bulundurulması `vitest`'in test dosyalarını otomatik olarak bulmasını sağlar. Başka bir klasörde de bulundurulabilir veya `*.test.js` uzantılı dosyalar da test dosyası olarak kabul edilir.

`form.jsx`

```jsx
export default function Form() {
  const [image, setImage] = useState(null);

  return (<>
    <form>
      <input data-testid="form-input" type="text" />
      <button type="submit">Submit</button>
    </form>

    <img src={image}>
    <button data-testid="img-button" type="button" onClick={() => {setImage(image === "2.png" ? '1.png' : '2.png')}}>Change Image</button>
  </>);
}
```

> `data-testid` attribute'u ile element test dosyasında erişilebilir hale gelir.

`form.test.jsx`

```jsx
import { expect, test } from "vitest";
import { render } from `@testing-library/react`;

test("renders form", async () => {
  const form = render(<Form />)

  // Test input
  const input = await form.getByTestId("form-input");
  expect(input).not.toBe(null);
  expect(input).toBeInstanceOf(HTMLInputElement);
  expect(input).toHaveAttribute("type", "text");

  // Test button
  const button = await form.getByText("Submit");
  expect(button).not.toBe(null);
  expect(button).toBeInstanceOf(HTMLButtonElement);
  expect(button).toHaveAttribute("type", "submit");

  form.unmount();
});

test("submits form", async () => {
  const form = render(<Form />)

  // Test form submission
  const submit = jest.fn();
  form.on("submit", submit);
  form.fire("submit");
  expect(submit).toHaveBeenCalled();

  form.unmount();
});

test("changes an image", async () => {
  const form = render(<Form />)

  // Test image change
  // Default image
  const img = await form.getByRole("img");
  expect(img).not.toBe(null);
  expect(img).toBeInstanceOf(HTMLImageElement);
  expect(img).toHaveAttribute("src", null);

  // Change image
  const button = await form.getByTestId("img-button");
  form.fire("click", button);
  expect(img).toHaveAttribute("src", "2.png");

  form.unmount();
});
```

### Testing Hooks

`useCounter.js`

```jsx
import { useState } from "react";

export function useCounter() {
  const [count, setCount] = useState(0);

  const increment = () => setCount((c) => c + 1);
  const decrement = () => setCount((c) => c - 1);

  return { count, increment, decrement };
}
```

`useCounter.test.js`

```jsx
import { expect, test } from "vitest";
import { renderHook, act } from "@testing-library/react-hooks";
import { useCounter } from "./useCounter";
import Provider from "./Provider";

test("increments the counter", () => {
  const {result} = renderHook(() => useCounter(),
  {
    wrapper: ({children}) => <Provider>{children}</Provider>
  });

  act(() => {
    result.current.increment();
  });

  expect(result.current.count).toBe(1);
});
```

### Testing Mocks

Herhangi bir kütüphane kullanılmadan da mocklar oluşturulabilir. Fakat bu örnekte  `vitest-fetch-mock` kullanılmıştır.

`vite.config.js`

```js
import { defineConfig } from "vite";

export default defineConfig({
  plugins: [react()]
  root: "src",
  test: {
    environment: "happy-dom",
    setupFiles: "./setupVitest.js" // Add this line
  }
});
```

`setupVitest.js`

```js
import createFetchMock from "vitest-fetch-mock";
import { vi } from "vitest";

const fetchMock = createFetchMock(vi);
fetchMock.enableMocks();
```

`useCategoryList.test.js`

```js
import { expect, test } from "vitest";
import { renderHook, act, waitFor } from "@testing-library/react-hooks";
import { useCategory } from "./useCategory";
import Provider from "./Provider";

test("gives back categories when given a postId", async () => {
  const categories = [
    "Tech",
    "Science",
    "Art",
    "Music",
    "History",
    "Literature",
  ];

  fetch.mockResponseOnce({
    JSON.stringify({
      postId: 1,
      categories
    })
  });

  const {result} = renderHook(() => useCategory(1),
  {
    wrapper: ({children}) => <Provider>{children}</Provider>
  });

  await waitFor(() => {
    expect(result.current.categories).toEqual(categories);
  });
});
```

### Test Coverage

Kodun ne kadarının test edildiğini görmek için `vitest`'in `--coverage` flag'i kullanılabilir.

`package.json`

```json
{
  "scripts": {
    [...]
    "test": "vitest",
    "test:coverage": "vitest --coverage"
    [...]
  }
}
```

`npm run test:coverage` komutu çalıştırıldığında test edilmeyen kodlar kırmızı, test edilen kodlar yeşil renkte gösterilir.

```terminal
File      | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s
----------|---------|----------|---------|---------|-------------------
All files |   88.89 |       50 |   83.33 |   88.89 |
 src      |   88.89 |       50 |   83.33 |   88.89 |
  App.jsx |   88.89 |       50 |   83.33 |   88.89 | 10
```

Ayrıca `--coverage` flag'i kullanıldığında `coverage` klasörü oluşturulur ve bu klasörde test edilen kodların detaylı raporu bulunur. Bu rapor `src\coverage\index.html` dosyasından görüntülenebilir.
