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
features that Angular gives us. Namely:

* `Controllers`
* `Services`
* `filters`
* and a few more special `Directives`

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
    <title></title>
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

```javascipt
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

## Controllers

To speak on a high level, a `Controller` is created by the Angular `Directive`
`ng-controller`. A `Controller` is a regular JavaScript object that allows us to
handle data contained within Angular's `$scope` object.

To bring it down a bit, lets take a look at an example of a `Controller` and see
what it lets us do.

