More Angular
----------

# Review
Previously we started to understand the concept of client-side MVC, and looked
at how we can use AngularJS to build our applications.

We used `Directives` and `Models` to get a very basic application that took a
user's name and displayed it in an `<h2>` tag.

To reflect back on that simple app, we learned that a `Directive` is an
extension to standard HTML, given to us by Angular. We can use them to do
several things, such as:

* tell Angular where our application starts and stops in the HTML
* Describe special attributes on existing HTML tags
* show and hide things

We also learned that a `Model` is a special type of `Directive` that allows us
to "bind" data from one area of our application to another (like we saw with the
`<input>` area and the `<h2>` tag.

# New Stuff
Building on top of that knowledge, we're going to explore some more built in
features that Angular gives us. Namely `Controllers` and a few more special `Directives`

## A "Real" Angular App
Before we get too deep into exploring more specific Angular features, we should
take a minute to discuss how to actually write a "real" Angular app.

In our previous example, we didn't explicitly define any new JavaScript. This
can be fine for very basic applications, though you will not get very far
without getting your hands dirty with some new JavaScript.

To start with, you need to include Angular in a script tag in the `<head>` of
your HTML, like we did with the previous example.

Right below that, we should also include a JavaScript file that we create,
usually named `app.js`, as well as place the `Directive` "`ng-app='app'`" either
on the `<html>` tag or the `<body>` tag.

Here's an example of some "boilerplate" HTML for starting an Angular app:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js"></script> 
    <script src="app.js"></script> 
  </head>
  <body ng-app="app">

  </body>
</html>
```

Inside of our `app.js` file, we are going to first create an "Immediate Invoked
Function Expression" (or IIFE for short).

```javascript
(function () {
  // our code goes here
})();
```

This is important for several reasons, though the biggest reason is that it
keeps all of our variables and methods from polluting the rest of the document.
If that were to happen, you might be OK for a while, but with a growing app,
things might start to break for seemingly no reason.

Imagine having a house full of people with the same name, and you're trying to
call out a specific one-- there would be a little confusion for a while. The
same thing can happen in JavaScript, though it's not always as easy to sort out.

IIFE's give us some protection from this nightmare.

The code that goes inside of our IIFE is pretty straight forward:

```javascript
(function () {
  angular
    .module('app', []);
})();
```
Here, we are calling the `angular` object given to us by including
`angular.min.js` in the previous `<script>` tag, and then defining the base
`module` that our app lives in.

In this example, our app is simply named `app`. This could be anything, really,
though it is important to make the connection between what we have called named
it in `app.js` and what we have called it in our `ng-app` directive in the HTML:
they should be identical, otherwise Angular will have no clue what you're
telling it to do.

> **Note**
> Angular is also generally nice enough to give us helpful errors when we make
> mistakes. Supposing you did misspell `app` in either `app.js` or your `ng-app`
> `Directive`, you'll get a lovely error message like this:
> ![App Error](app-error.png)
> This might look scary at first, but notice that it gives us a link to click
> on:
> > https://docs.angularjs.org/error/$injector/modulerr?p0=app&p1=Error:%20%E2%80%A6ogleapis.com%2Fajax%2Flibs%2Fangularjs%2F1.5.0%2Fangular.min.js:20:449

Another important thing is the empty array, `[]`, that we place after the comma
in the `module` method:

```javascript
  //...           |
  //...           V
  .module('app', []);
  //...
```

In the future, you'll be listing "dependencies" in here,
which can either be built-in functionality that you'd like to use, or 3rd-party
libraries that are made to work with Angular. We'll explore more of that later.
For the time being, make sure to not forget this empty array, as Angular will
complain quite a bit without it.

## Our First Controller

To speak on a high level, a `Controller` is created by the Angular `Directive`
`ng-controller`. A `Controller` is a regular JavaScript object that allows us to
handle data contained within Angular's `$scope` object.

To bring it down a bit, lets take a look at an example of a `Controller` and see
what it lets us do.

In our `app.js` file, let's add the following code

> **app.js**
> ```diff
> (function () {
> + function MainController () {
> +   var vm = this;
> +   vm.name = "Johnny";
> + }
> 
>   angular
>     .module('app', [])
> +   .controller('mainController', MainController);
> })();
> ```

> **Note**
>
> The green lines that start with `+`'s denote what is *new* to the file

In `app.js`, we are defining a new JavaScript function called `MainController`.
Inside of `MainController`, we are assigning a variable `vm` (short for
ViewModel) to `this`.

> **Note**
>
> For small applications, `var vm = this;` might not be very important, though
> as your app grows, and the code you write in your controller gets longer, the
> meaning of `this` will start to become ambigious.
> 
> Some of this ambiguity is handled nicely with newer ES6 syntax, though let's
> stick to "best practices" in ES5 for the time being.


After we've defined `vm`, we then add a property to it called `name`, and assign
it a value of `"Johnny"`.

Next, we have to inject this `Controller` into Angular. We can do this with the
`controller` method that comes with Angular. To do this, all we need is a string
that contains the name of the `Controller` as we wish to reference it, and then
the function name passed in afterward.

In our example right now, we're referencing the controller with the string
`'mainController'`, and then passing in the proper name, `MainController`.

```javascript
  //...
  .controller('mainController', MainController);
  //...
```

With that saved, we now need to add some stuff to our HTML code:

> **index.html**
> ```diff
> <!DOCTYPE html>
> <html lang="en">
>   <head>
>     <meta charset="UTF-8">
>     <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js"></script> 
>     <script src="app.js"></script> 
>   </head>
>   <body ng-app="app">
> +   <div ng-controller="mainController as vm">
> +     <h2>Hello, {{vm.name}}</h2>
> +   </div>
>   </body>
> </html>
> ```

Now in our HTML, we are simply asking for the value contained inside of
`vm.name` and displaying it onto the page.

Save your work and open it up in the browser. If all went well, you should end
up with a page that looks like this:

![First Controller Demo](first-controller.png)

## Let's Add Some More Functionality

Going with what we know about `Controller`'s, let's explore adding some new
features to our app.

The first thing we're going to look at is adding methods to our `MainController`
inside of `app.js`.


> **app.js**
> ```diff
>  (function () {
>    function MainController () {
>      var vm = this;
> -    vm.name = "Jonny";
> +    vm.count = 0;
>
> +    vm.reset = function () {
> +      vm.count = 0;
> +    }
>
> +    vm.increment = function () {
> +      vm.count++;
> +    }
>
> +    vm.decrement = function () {
> +      vm.count--;
> +    }
> +  }
>
>    angular
>      .module('app', [])
>      .controller('mainController', MainController);
>  })();
> ```
>
> **Note:**
> The red lines starting with `-` tell us what we're deleting in our changes

All we've done here is attached three functions to our `vm`:

* `reset()`
* `increment()`
* `decrement()`

and attached a new variabled called `count` to our `vm` that contains the
current count. `increment()` and `decrement()` add to and remove 1 from the
`count`, and `reset()` does what it sounds like-- it resets the `count` back to
0.

Now that we've added the logic for a simple counter to `app.js`, we need to wire
the functionality into our HTML

> **index.html**
> ```diff
> <!DOCTYPE html>
> <html lang="en">
>   <head>
>     <meta charset="UTF-8">
>     <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js"></script> 
>     <script src="app.js"></script> 
>   </head>
>   <body ng-app="app">
>     <div ng-controller="mainController as vm">
> -     <h2>Hello, {{vm.name}}</h2>
> +     <h2>Counter App</h2>
>
> +     <div>{{vm.count}}</div>
> 
> +     <button ng-click="vm.increment()">+</button>
> +     <button ng-click="vm.decrement()">-</button>
> +     <button ng-click="vm.reset()">reset</button>
>     </div>
>   </body>
> </html>
> ```

Let's take a second to go over what we're doing in the HTML. The reference to
`vm.name` is now `vm.count`, which reflects the `vm.count` we defined in
`app.js`. This is going to initially be set to 0, and will show up on the
website as 0

Secondly, we've added some buttons with `ng-click` `Directives`. Inside of these
`ng-click`'s, we've specified our `Controller` methods:

* `vm.increment()`
* `vm.decrement()`
* `vm.reset()`

With our `app.js` and `index.html` saved, lets view it in the browser:

![In Action](http://g.recordit.co/dQRIJyoD5y.gif)

Using the app for a minute, you might notice that clicking the `-` button a
bunch of times will result in us eventually getting negative numbers. Maybe we
could write some more functionality that would limit how low the numbers could
get?

Let's work out how we'd make the `-` button stop working after the user gets to
0:

> **app.js**
```diff
> (function () {
>   function MainController () {
>     var vm = this;
>     vm.count = 0;
> 
>     vm.reset = function () {
>       vm.count = 0;
>     }
> 
>     vm.increment = function () {
>       vm.count++;
>     }
> 
>     vm.decrement = function () {
>       vm.count--;
>     }
> 
> +   vm.isCountZero = function () {
> +     return vm.count === 0;
> +   }
>   }
> 
>   angular
>     .module('app', [])
>     .controller('mainController', MainController);
> })();
```

Here we've just defined a predicate function that will return true or false
depending on whether `vm.count` is 0. We will use this in our HTML to turn off
to `-` button when the user clicks down to 0.

> **index.html**
> ```diff
> <!DOCTYPE html>
> <html lang="en">
>   <head>
>     <meta charset="UTF-8">
>     <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js"></script> 
>     <script src="app.js"></script> 
>   </head>
>   <body ng-app="app">
>     <div ng-controller="mainController as vm">
>       <h2>Hello, {{vm.name}}</h2>
>       <h2>Counter App</h2>
>
>       <div>{{vm.count}}</div>
> 
>       <button ng-click="vm.increment()">+</button>
> -     <button ng-click="vm.decrement()">-</button>
> +     <button ng-click="vm.decrement()"
> +             ng-disabled="vm.isCountZero()">-</button>
>       <button ng-click="vm.reset()">reset</button>
>     </div>
>   </body>
> </html>
> ```

By just adding a call to the `ng-disabled` `Directive and supplying it our
`Controller's` `vm.isCountZero()` method, we can turn the button off when the
counter is at zero.

On a stylistic or aesthetic level, we've also made the button take up two lines
in our markup instead of just on. I feel like this can be a good decision once
we start adding many CSS attributes and `Directives` to our elements because we
can quickly see, line by line, what it's doing.

## Something different
In the previous example, we saw how we can use `Controller` methods to manage
the state of a `Controller` variable; specifically, using `vm.increment()`,
`vm.decrement()`, and `vm.reset()` to manage `vm.count`.

Switching to a different gear, lets take a look at consuming a resource of sorts
and displaying it with some more advanced `Directives`

Starting with a new `app.js` and a new `index.html`, we can get to work.

Here is the boilerplate code we're going to be starting with:

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

First, let's add a new `Controller` to `app.js` called `PetsController`:

> **app.js**
> ```diff
> (function () {
> + function PetsController () {
> +   var vm = this;
> +   vm.pets = [
> +     {name: 'Luna', type: 'dog', owner: 'Jos', gender: 'female'},
> +     {name: 'Boozer', type: 'dog', owner: 'Jos', gender: 'male'},
> +     {name: 'Sally', type: 'cat', owner: 'Josh', gender: 'female'}
> +   ];
> + }
> 
>   angular
>     .module('petsApp', [])
> +   .controller('petsController', PetsController');
> })();
> ```

Here, we've created a very simple `Controller` that just contains a list of
pets. We've also injected it into our Angular app through the `controller`
method and given it the name `'petsController'`.

Next, lets work on our HTML and adding `Directives` to display our data:

> **index.html**
> ```diff
> <!DOCTYPE html>
>   <html lang="en">
>     <head>
>       <meta charset="UTF-8">
>       <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js"></script> 
>       <script src="app.js"></script>
>     </head>
>   <body ng-app="petsApp">
> +   <div ng-controller="petsController as vm">
> +     <h2>Pets</h2>
> +     <ul>
> +       <li ng-repeat="pet in vm.pets | orderBy : 'owner'">
> +         {{pet.owner}} has a {{pet.type}} named {{pet.name}}
> +       </li>
> +     </ul>
> +   </div>
>   </body>
> </html>
> ```

There's a lot more magic happening in our HTML this time. Starting in familiar
territory, though, we're using our `ng-controller` directive to use our
`petsController` and alias it as `vm`.

In our `<li>` element we're taking advantage of another great `Directive` that
Angular gives us out of the box: `ng-repeat`. If you're used to writing our
loops and know what they do, the `ng-repeat` directive behaves very similarly to
JavaScript's `for...in` or `.forEach()` loop.

On the face of it, we're iterating over the `vm.pets` array. Each item in the
array is aliased to `pet`, which we can then use inside of the loop to display
bits of information, such as `pet.name` or `pet.owner`.

While still discussing `ng-repeat` as we've used it, you may also notice the `|`
and the words `orderBy : 'owner'`-- the `|` is called a pipe, and `orderBy`
is a built in `Filter` that takes an array of Objects and arranges them
according to the argument supplied to it.

In our instance, we're telling Angular to order the pets alphabetically by the
owner's name. The string could be read as "alphabetically order `vm.pets` by the
owners name, then iterate over `vm.pets`, aliasing each item as `pet`"

How cool is that?

If we view it in the browser, it should look something like this:

![Pets App](pets-app.png)

## Vocabulary

We built on top of the previous lesson and got more comfortable using Angular's
terminology, namely with `Directives` and `Controllers`. We also learned about
Immediately Invoked Function Expressions (IIFEs). We learned what `Filter`'s are
and that we can use them to do things like sort lists.

## Review

In this section, we learned how to:

* Start writing a 'real' Angular app
* Use Immediately Invoked Function Expressions (IIFEs)
* Use more of Angular's `Directives`
* Write and inject `Controllers`
* 

## What's To Come

In the next section, we're going to look at using `Services` and `Factories` to
contain our data (such as lists, or even calls to APIs). 
