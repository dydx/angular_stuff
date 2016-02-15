Introduction to AngularJS
---------
## Review
When we started class, our first unit dealt with client-side applications, which were made with HTML, CSS, and JavaScript.

Those three technologies fit together to give us:

* Structure (HTML)
* Layout / Aesthetic (CSS)
* Interaction / Functionality (JavaScript)

As you built your Project 1 games, you might have encountered some limitations in what you could do or had areas where you wished to save something between page refreshes (like allowing a user to 'log in')

As we moved forward into units 2 and 3, we focused heavily on the server side. Ruby on Rails introduced us to several new ideas:

* Storing data in a database (PostgreSQL)
* `Models` to represent "things" like a `User` or a `Product`
* `Controllers` to handle which RESTful actions like `new` or `update`
* `Views` to handle what would ultimately be shown in the browser
* `Routing` to map our `Controllers` to URLs like `root 'welcome#index'`

You might have had JavaScript running in your specific application, but for the most part we focused on server-side applications.

Unit 3 brought us back to JavaScript, but flipped this now-old language around and we learned that it could also run on the server, giving us NodeJS.

Utilizing NodeJS and a small web framework called Express, we were able to write web applications in JavaScript that behaved similarly to the way we might have written them in Ruby using Sinatra or Rails.

There is yet another stage in this journey through web application development, specifically when it comes to JavaScript: client-side MVC.

## Clientside MVC??

Just as we had MVC on the server with Rails, we can have MVC in the client with a framework called Angular.

Using the HTML and CSS that we learned early on, as well as Angular, we can create web applications that run entirely in the browser.

Generally speaking, a back-end server is still required to save data and actually serve the web page to the browser, but with database-as-a-service providers becoming more popular, it is possible to create a feature-filled app hosted statically on something like GitHub Pages or BitBalloon.

## Why?

Some might ask why we would want to do this. There are several pros and cons to having your application run in the browser that should be discussed before a big project:

**Pros**

* User interaction typically improves
* Clear architecture of the front-end
* Responses from the back-end are usually in JSON, which is nice

**Cons**

* Many considerations must be taken about what the many browsers support vs what you wish to deliver (JavaScript and CSS compatibility, mostly)
* Progressive enhancement can be hard (what if the user's browser doesn't have JavaScript turned on?)
* Minimalistic frameworks and libraries can lead to sometimes tough architectural decisions (just like with Rails vs Express)

With all of that in mind, Angular is here to help us. As mentioned earlier, Angular provides a MVC-like environment to work in that has made a few architectural decisions for us. It would be somewhat fair to say that Angular is the Rails of the browser.

## Birds Eye View

When used with a backend server, Angular helps us move a lot of things off of the server and onto the client.

The backend will still generally handle stuff like storing data, handling user sessions, and  Though, now, a lot of our routing and view components will be handled only in the browser.

This helps the application "come alive" and feel very fluid. Think of an application like GMail, where full page refreshes rarely happen.

**Server**

* Database
* Sessions
* Security

**Client**

* Views
* Controllers
* Routing

Lets take a look at the two ways we know how to do HTML and JavaScript, and compare them to this new "Angular Way"

**Using static HTML**
```html
<!doctype html>
<html>
  <head>
  </head>
  <body>
    <h2>Hello John!</h2>
  </body>
</html>
```

Its easy to see what we're doing with the static HTML version, though the limitation of this is that its static. There is zero interaction with the end user. This is probably OK in some cases, but for most websites and apps, you (and your users) would like to be able to "use" it.

**Using jQuery and HTML**
```html
<!doctype html>
<html>
  <head>
    <script src="js/jquery-1.7.2.js"></script>
    <script type="text/javascript">
      $(function() {
        var name     = $('#name');
        var greeting = $('#greeting');

        name.keyup(function() {
          greeting.text('Hello ' + name.val() + '!');
        });
      });
    </script>
  </head>
  <body>
    <input id="name" type="text" placeholder="Enter your name">
    <h2 id="greeting"></h2>
  </body>
</html>
```

In our jQuery version, we've decided to add some interaction with a simple form input and a `keyup` event listener. All we're doing is taking what the user typed into the form field and displaying it in an `<h2>` tag.

**Using Angular**
```html
<!doctype html>
<html ng-app>
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.3.14/angular.min.js"></script>
  </head>
  <body>
    <div>
      <label>Name:</label>
      <input type="text" ng-model="yourName" placeholder="Enter your name">
      <hr>
      <h2>Hello {{yourName}}!</h2>
    </div>
  </body>
</html>
```

The Angular version of this app might seem a bit mysterious. There are new things like `ng-app` in the `<html>` tag, `ng-model` in the `<input>` tag, and then double curly brackets inside of the `<h2>` tag. You might also notice we haven't written any JavaScript; and, yet, the app still works like the jQuery version.

Having had your chance to work on the previous three projects, and knowing how much jQuery (or 'vanilla' JavaScript even) it can take to built a robust experience; which would you like to be using? jQuery or Angular?



## Architecture and Vocabulary

There won't be a spelling quiz at the end of the day, but it is important to familiarize yourself with the names and meanings of things in Angular.

The first thing we saw in our Angular app above was the `ng-app` word used inside of the `<html>` tag. Words that start with `ng-` are called `Directives` in Angular. This particular one tells Angular that everything inside of this HTML tag is the 'app'. There are [many more](http://www.techstrikers.com/AngularJS/angularjs-built-in-directives.php) that come with Angular that handle looping over lists or handling conditionals. 

The creators of Angular understood that the developers who would eventually use it might need `Directives` that do more advanced or specific things than what they had initially thought of, and so you are also able to define and use "custom" `Directives`

Knowing what you know now about `Directives`, it should be clear that `ng-model` is also a `Directive`.

`ng-model` can help us handle many things such as:

* form input binding
* form input validation
* setting CSS attributes (which could then trigger things like animations)

In our example above, `ng-model` replaces the `name` field of the `<input>` element to give us a sort of key to look for when we wish to use the value later.

Think of submitting a form in Rails. Typically the input element would have a `name` attribute with a value of `password`. When the form is then submitted, Rails is able to get the user's password by looking for the `password` key in the form data.

We are doing something very similar here with the combination of `ng-model="yourName"` on the form input and `{{yourName}}` in the HTML. 

Angular knows where to get `yourName` from because we told it where `yourName` was. 

## Review
In this brief introduction, we revisited some techniques on writing web applications, as well as introduced a new tool for writing them called Angular.

We discussed a few pros and cons of using Angular, as well as gave some examples of Angular as compared to "old" methods.

Also introduced were some new words that we will be using from now on and gave some context about what they mean to Angular.

## What's To Come

There is so much to learn when getting into a topic like clientside MVC and Angular. Future discussions will cover more about `Directives`, introduce the idea of `Controllers`, as well as `Modules` in Angular
