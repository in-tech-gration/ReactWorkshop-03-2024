# Did you know you can write your own typesafe React router in 500 lines?
> https://sinja.io/blog/build-typesafe-react-router-from-scratch

Besides that, our router will support everything you'd expect from average router: navigation to URLs, route matching, route parameters parsing, back/forward navigation, and navigation blocking.

## Terminology

There will be two kinds of routes in our library: raw routes and parsed routes. Raw route is just a string that looks like /user/:id/info or /login, it's a template for URL. Raw route can contain parameters, which are sections that start with a colon, like :id. This format is easy to use for developers, but not so much for a program, we'll transform those routes into a more machine-friendly format and call it parsed route.

But users don't open routes, they open URLs. And URL is an identifier for resource (page inside the app in our case), it might look like http://example.app/user/42/info?anonymous=1#bio. Our router mostly cares about the second part of the URL (/user/42/info?anonymous=1#bio), that we'll call path. Path consists of pathname (/user/42/info), search parameters (?anonymous=1) and hash (#bio). The object that stores those components as separate fields will be called location.

When building React libraries, I like to go in three steps. First of all, imperative API. Its functions can be called from any place and they're usually not tied to React at all. In case of a router, that will be functions like navigate, getLocation or subscribe. Then, based on those functions, hooks like useLocation or useCurrentRoute are made. And then those functions and hooks are used as a base for building components. This works exceptionally well, in my experience, and allows for making an easily extendable and versatile library.

Router's API starts with defineRoutes function.

```js
const routes = defineRoutes({
    login: '/login',
    user: {
        me: '/user/me',
        byId: '/user/:id/info',
    }
});
```

Next step is to pass parsed routes to createRouter function. This is the meat of our router. This function will create all of the functions, hooks and components.

```js
const { 
    Link, 
    Route, 
    useCurrentRoute, 
    navigate,
    /* etc... */ 
} = createRouter(routes);
```

createRouter will return functions which can be used anywhere in your app (imperative API), hooks which allow your components to react to location changes and three components: Link, Route and NotFound. This will be enough to cover the majority of use-cases and you can build your own components based on those APIs.

Typesafety will protect you form:

```jsx
<Link href="/logim" />

const { userld } = useRoute(routes.user.byId);
```
