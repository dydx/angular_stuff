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

From a high level, a `Service` can be used in many ways, but it often used as a
data container. Instead of representing data right in your `Controller`, you can
do it in a `Service` and simply reference that elsewhere.

This helps keep our `Controllers` "skinny".

Using what we have from our previous "pets" example, we can extract some
functionality into a `Service` and make our `Controller` a bit more manageable.

Here is the current code, in case you don't have it readily available or have
skipped to this section from somewhere else:

> **app.js**
> ```javascript
> (function () {
>   function PetsController () {
>     var vm = this;
>     vm.pets = [
>       {name: 'Luna', type: 'dog', owner: 'Jos'},
>       {name: 'Boozer', type: 'dog', owner: 'Jos'},
>       {name: 'Sally', type: 'cat', owner: 'Josh'}
>     ];
>   }
> 
>   angular
>     .module('app', [])
>     .controller('petsController', PetsController);
> })();
> ```

> **index.html**
> ```html
> <!DOCTYPE html>
> <html lang="en">
>   <head>
>     <meta charset="UTF-8">
>     <title></title>
>     <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js"></script> 
>   </head>
>   <body ng-app="app">
>     <div ng-controller="petsController as vm">
>       <h2>Pets</h2>
>       <ul>
>         <li ng-repeat="pet in vm.pets | orderBy : 'owner'">
>           {{pet.owner}} has a {{pet.type}} named {{pet.name}}
>         </li>
>       </ul>
>     </div>
>   </body>
> </html>
> 
> ```

The first thing we're going to do is create a `PetsService` object in `app.js`,
and pull our `pets` data into it:

> **app.js**
> ```diff
> (function () {
> + function PetsService () {
> +   var vm = this;
> +   vm.pets = [
> +     {name: 'Luna', type: 'dog', owner: 'Jos'},
> +     {name: 'Boozer', type: 'dog', owner: 'Jos'},
> +     {name: 'Sally', type: 'cat', owner: 'Josh'}
> +   ];
> +  ]
> + }
> 
> - function PetsController () {
> + function PetsController (petsService) {
>     var vm = this;
> -   vm.pets = [
> -     {name: 'Luna', type: 'dog', owner: 'Jos'},
> -     {name: 'Boozer', type: 'dog', owner: 'Jos'},
> -     {name: 'Sally', type: 'cat', owner: 'Josh'}
> -   ];
> +   var pets = petsService.pets;
>   }
> 
>   angular
>     .module('app', [])
> +   .service('petsService', PetsService)
>     .controller('petsController', PetsController);
> })();
