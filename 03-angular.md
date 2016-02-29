Even More Angular!
----------

# Review

In the previous section, we learned a lot more about how to structure the pieces
of our application, both in `app.js` and in `index.html`. We also explored some
more functionality available to us with `Directives` and how to utilize
`Controllers` to some extent.

When we're building our Angular applications, its best to start simply and build
our way upward and make changes according to our own needs. By learning more
about the architecture that Angular gives us, we can make informed decisions
that will help prevent us from problems in the future.

# Services

When working with apps, the idea of a data source comes up. Whether its from a
database or a hard-coded object or array, it needs to live somewhere.

With this section, we're going to work on a simple app to showcase pets and
who owns them. We will start by keeping the data in our `Controller` and then
eventually migrating it into what Angular calls a `Service`.

From a high level

Starting with a new `app.js` and a new `index.html`, we can get to work.

> **app.js**
> ```javascript
> (function () {
>   angular
>     .module('petsApp', [])
> })();
> ```

> **index.html**
> ```html
> <!DOCTYPE html>
>   <html lang="en">
>     <head>
>       <meta charset="UTF-8">
>       <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js"></script> 
>       <script src="app.js"></script>
>     </head>
>   <body ng-app="petsApp">
>   </body>
> </html>
> ```
