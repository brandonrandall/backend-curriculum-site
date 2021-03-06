---
layout: page
title: jQuery DOM Traversal, Manipulation, and Events
length: 180
tags: javascript, jquery
status: draft
---

## Learning Goals

* Use jQuery selectors to find content
* Understand that jQuery collections allow you to manipulate multiple elements with a single method
* Use jQuery's DOM traversal methods to move around the DOM
* Add CSS styles using jQuery
* Append new content to the DOM
* Add event listeners to elements currently in the DOM
* Understand that adding an event listener will not effect elements you add to the DOM in the future

![js logo][js-logo]

[js-logo]: /assets/images/lessons/jquery/js-logo.png

---

## Getting Up to Speed with JavaScript

JavaScript was created to make the web more dynamic. It is an object-oriented scripting language made to run inside a host environment like a web browser and provide programatic control over the objects of that environment. For example, when you click a button and you want something about the webpage to change, you will use JavaScript.

JavaScript can be _client-side_ and _server-side_, meaning that it can be used to control user-facing interfaces as well as handle the server-side extensions that connect with a database.

It’s a highly versatile and flexible language, and has become the most commonly used language of the web.

Today we'll be talking about using JavaScript and the jQuery library to manipulate the DOM in a rails application.

### Syntax, Variables and Functions

#### Syntax

Each statement in JavaScript ends with a semicolon (`;`). 

JS has automatic semicolon insertion, so technically you can omit them most of the time and your code will still work. Use them anyway, at least for the first year you work with JavaScript.

Camel case `camelCase` is prefered over snake case `snake_case`.

Comments are made with `//`.

#### Variables

JS variables MUST be declared/created using `var`.

```js
var isSnowing = true;
```

_Note: if you work in es6, you can also use `let` or `const`._

If you redefine a variables, you will not use the `var` keyword.

```js
var isSnowing = true;

// looks out window

isSnowing = false;
```

Variables can be _declared_ without being _defined_.

```js
var yetToBeDefined;
yetToBeDefined;
// => undefined
yetToBeDefined = true;
yetToBeDefined;
// => true
```

#### Functions

Functions are a way to group statements together to perform a specific task. Functions are reusable blocks of code. To create a function, you must give it a name and then write the statements required for the function to achieve its task inside the function’s curly braces

Functions will follow the following general structure:

```js
var doSomething = function(thing) {
  return thing;
};
```

or 

```js
function doSomething(thing) {
  return thing;
};
```

_Note: the first example creates an unnamed function and then assigns it to a variable. Called a `function expression`. The second example is a `function declaration` - at the time you create a function you declare a name for it. This allows you to leverage a concept called `hoisting` in JavaScript. Because of the way JS is interpreted, the second function is available to use anywhere in your code. You can think of it as being 'hoisted' to the top of the file. Read more [here](http://adripofjavascript.com/blog/drips/variable-and-function-hoisting)_

A function is called/invoked/run by using the parens:

```js
doSomething("test");
// => "test"
```

Something that is really powerful about JavaScript is that functions can be passed around.

If a function is called without parens, it is a reference to a function.

```js
  doSomething // will not run
```

Unlike ruby, if you want your function to return something other than undefined, you must explicitly use `return`. When a function hits a `return`, it will immediately stop there and return whatever follows the `return` statement.

![hilarious semicolon reference][semicolons]

[semicolons]: /assets/images/lessons/jquery/semicolons.png

---

### Debugging in Javascript

Debugging client-side and server-side JavaScript is a different beast than debugging Ruby. 

Because client-side JS is run entirely in the browser, the technique for troubleshooting broken code is more complicated than `binding.pry`. Luckily, modern browsers are aware of this and give us a collection of options for digging into your code.

#### 1. Developer Tools
One of the first things you should familiarize yourself with when working with JavaScript (or HTML...or CSS...) are the dev tools. You can find a cool tutorial to dive deeper with  [Code School's Discover-DevTools Tutorial.](http://discover-devtools.codeschool.com/) (Chapters 3 & 4 are particularly helpful)

To open developer tools in Chrome:
-   Mac: `Cmd` + `Opt` + `i` (or `Cmd` + `Opt` + `j`)
-   (or) Right click on the browser window and select `inspect`
-   (or) Select `View` in the navbar, then `Developer`, then `Developer Tools`

When working with JavaScript, it is useful to keep your console open at all times to watch for errors and anything you've told your code to print out. Bringing us to...

#### 2. console.log()
`console.log()` is to JS what `puts` is to Ruby. This line of code will print whatever is provided as an argument to the console.

Given the following function called `printStuff()`, adding console.log() will print the value of `myVariable` to the console.

```
var printStuff = function(){
  var myVariable = 5 + 5
  console.log(myVariable);
}

printStuff()
=> 10
```

If you're confused about what a variable or function is returning, throw `console.log()` into your code or directly into the `console` in your browser to confirm/deny suspicions.

#### 3. Debugging In the Console

`debugger` is the `pry` of JS. Stick `debugger;` within a function to pause the browser from running the script when it hits a particular part of your code.

```
// index.js
$('#search-ideas').on('keyup', function() {
  var currentInput = this.value.toLowerCase();

  $ideas.each(function (index, idea) {
    var $idea = $(idea);
    var $ideaContent = $idea.find('.content').text().toLowerCase();
    debugger;
    if ($ideacontent.indexOf(currentInput) >= 0) {
      $idea.show();
    } else {
      $idea.hide();
    }
  });
```

***Warning***: A `debugger` statment will not trigger unless your inspector tools in the browser are open. This is meant to not interrupt a users experience if a developer accidently commits and deploys a `debugger` statement.

In the browser, if we open up the dev tools, navigate to the console and try to search for something.  The program will freeze on the line `debugger`. This lets us type stuff into our `console` to see what's going on.

For a more indepth lesson on working with DevTools - check out [advanced debugging](http://frontend.turing.io/lessons/debugging-with-devtools.html) or the [Chrome Documentation](https://developer.chrome.com/devtools/docs/javascript-debugging).

![dev tools][dev-tools]

[dev-tools]: /assets/images/lessons/jquery/dev-tools.jpg

---

## Intro to the DOM

DOM stands for Document Object Model. The browser uses it to represent everything on the page. It's an "object model" because it is made of objects. Each element is an object. If you wanted to, you could directly translate the DOM to a JavaScript object.

The DOM is hierarchical. If you have a tag wrapping another tag, then the inner object is a child of the outer object, which is the parent.

```html
<ol>                <!-- parent -->
  <li>First</li>    <!-- child  -->
  <li>Second</li>   <!-- child  -->
</ol>

```

The browser creates the DOM by reading from HTML, but from then on, JavaScript controls any changes to the DOM.

```
HTML --> DOM <--> JS
```


![DOM][dom]

[dom]: /assets/images/lessons/jquery/dom.png

<cite> By Birger Eriksson - Own work, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=18034500 </cite>

---

## Lecture, Introduction

### The Power of JavaScript

As mentioned before, you can use JavaScript to create a script, run a server, interact with a database, etc. But if you already have Ruby & Rails, you'll do these things with Ruby for the most part (at least on the server side). 

The thing you'll really want to use JavaScript to do is manipulate the DOM. JS will give you the powers to change a page without refreshing it, react to user interaction, automatically size things, etc.

### jQuery vs Vanilla JavaScript

When you use the methods that JavaScript has out of the box, you'll often hear this refered to as using `vanilla JS`. There is an excellent [cheatsheet](https://gist.github.com/thegitfather/9c9f1a927cd57df14a59c268f118ce86) that has all of these methods.

The problem is that many of these methods are confusing or hard to work with.

Why is this? JavaScript is run on the browser and for that reason, the maintainers or the language have no control over forcing users to update their browsers. This is the problem that JavaScript has had for it's entire life. Basically, once you add something to JavaScript, you have to support it basically forever.

So changes to JavaScript the language move _very_ slowly. And there are methods in JavaScript that are legacy and kind of broken, but still exist. And a ton of quirks.

For that reason, libraries like [jQuery](https://jquery.com/) exist and are used heavily.

jQuery is a library that wraps all of the DOM manipulation in JavaScript in friendly syntax and safeguards against browser incopatability. Under the hood, it’s just JavaScript. It’s used on about 78% of the top million web pages, so it’s worth while getting comfortable with it.

Anything you can do in jQuery, you can do in vanilla JavaScript (with more lines of code, usually).

John Resig, the creator of jQuery, released an annotated version of the original source code of jQuery. It's [absolutely worth reading through](http://genius.it/5088474/ejohn.org/files/jquery-original.html)

![jquery logo][jquery]

[jquery]: /assets/images/lessons/jquery/jquery.png

---

## Lecture, Part One: Selectors

### Basic Selectors

Out of the box, jQuery supports the selector syntax from CSS to find elements on the page.

That said, let's review some of the different ways we can find an element on page.

* `$('p')`, selects all of a given element.
* `$('#heading')`, selects the element with a given id.
* `$('.important')`, selects all of the elements with a given class.

You can also use multiple selectors in the same statement:

* `$('p, #heading, .important')`, selects everything listed above.

### Chaining Selectors

There are a few different ways to chain selectors to use them together. You can seperate these selectors with a comma, a space, or nothing at all.

* Comma: `$('p, #heading, .important')` just combines all of the selectors together.
* Space: `$('p #heading .important')` treats each selector as a child of the previous. This will give you items of the class `important` that are children of the id `heading` which are inside a `<p>` tag.
* Nothing: * `$('p#heading.important')` matches elements that have all three selectors. This selector would select a paragraph which was defined like this: `<p id="heading" class="important">`

### Attribute Selectors

On top of querying for types of elements, classes, and ids, we can also search for elements by the type of attributes that it has.

Here are some examples:

- `$('input[type="date"]')` will get all of the date selector inputs.
- `$('input[type="number"]')` will get all of the number inputs.
- `$('input[type="submit"]')` will get all of the form submission buttons.

See the API documentation [here](http://api.jquery.com/category/selectors/attribute-selectors/).

### The Special `:checked` Selector

It's not uncommon that you might want to go look for all of the "checked" elements. This includes drop downs, checkboxes, and radio buttons.

- `$('input:checked')` will return all of the checked checkboxes.
- `$('input[type="radio"]:checked')` will return all of the checked radio buttons.

### Laboratory

[Here is an little experiment][exp] where you can play around and try out some different selectors.

Play around with this on your own for a bit.

[exp]: http://codylindley.com/jqueryselectors/

---

![part 1][part-one]

[part-one]: /assets/images/lessons/jquery/part-one.jpg

## Exercise, Part One: The Presidents

For this exercise, we're going to play with [a table of the Presidents of the United States of America](https://github.com/turingschool-examples/jquery-playgrounds).

You can either work from the repo or visit a [hosted version on github pages](https://turingschool-examples.github.io/jquery-playgrounds/presidents.html)

```
git clone https://github.com/turingschool-examples/jquery-playgrounds.git jquery_playgrounds

cd jquery_playgrounds

open presidents.html
```

Let's try out a few things, just to get our hands dirty. We'll use the console in the Chrome developer tools to validate our work.

* Select each `tr` element.
* Select all of the elements with the class of `name`.
* Select all of the elements with either the class of `name` or `term`.
* Select all of the checked —umm— checkboxes. (You'll probably want to check some checkboxes first.)
* Select all of the `td` elements with the class of `number` that appear in a row of a `tr` with the class of `whig`.

(This should take about five minutes total.)

---

![part 2][part-two]

[part-two]: /assets/images/lessons/jquery/part-two.jpg

## Lecture, Part Two: Manipulating CSS

Once we have an element in our sites, we probably want to do something with it, right?

In this case, let's add some CSS styling. Let's say we wanted to grab all of the Federalist presidents and turn their font color pink. We could do something like this:

```js
$('.federalist').css('color', 'pink');
```

One thing you might have noticed about CSS is that it really likes hyphens. So, to change a background color, you would use `background-color`. The thing about hyphens is that they are a no-no in JavaScript. So, we *should* to camel-case our property names in our CSS methods.

```js
$('.federalist').css('backgroundColor', 'pink');
```

You'll notice I said "should" instead of must. At the end of the day that's just a string. You can do it the other way, but it's against convention.

```js
$('.federalist').css('background-color', 'pink');
```

Right now, we're setting individual properties. We can also pass in a conditional object in order to change multiple CSS attributes all at once.

```js
$('.federalist').css({
  backgroundColor: 'pink',
  fontWeight: 'bold'
});
```

If you ignored me earlier and insisted on using hyphens, you're going to have to wrap those property names in quotes now. Yuck.

Writing CSS by hand is probably a bad idea. We're better off using classes to style our content.

We can add and remove classes pretty easily in jQuery.

```js
$('.federalist').addClass('red');
$('.federalist').removeClass('red');
```

Keeping track of state is hard. jQuery is here to help.

```js
$('.federalist').hasClass('federalist'); // Returns true, obviously.
```

The other option is to use `toggleClass`, which will either add or remove the class depending on whether or not the class currently exists.

```js
$('.federalist').toggleClass('red');
```

## Exercise, Part Two: Style the Presidents

* Add the class of `red` to all of the Republicans.
* Add the class of `blue` to all of the Democrats.
* Add the class of `yellow` to the term column of the table.
* Take all the whig presidents and give them a purple background and white text.

---

![part 3][part-three]

[part-three]: /assets/images/lessons/jquery/part-three.png

## Lecture, Part Three: Filtering and Traversal

Let's talk about a few [DOM traversal methods](http://api.jquery.com/category/traversing/tree-traversal/).

Here are some of the all-stars of the DOM traversing world:

### `parent()`

The `parent()` method will take the currently selected element and go one level up the DOM tree.

<p data-height="300" data-theme-id="23788" data-slug-hash="AXQkEZ" data-default-tab="js,result" data-user="turing" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/team/turing/pen/AXQkEZ/">jQuery Parent</a> by Turing School of Software and Design (<a href="http://codepen.io/turing">@turing</a>) on <a href="http://codepen.io">CodePen</a>.</p>

### `parents()`

This one will include all of the parents—all the way up to the `<body>` of the page. Additionally, you can pass it a selector. `$('.some-selector').parents('.active')` will traverse up the DOM, but only return the elements with the class of `.active`.

<p data-height="300" data-theme-id="23788" data-slug-hash="AXQkXA" data-default-tab="js,result" data-user="turing" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/team/turing/pen/AXQkXA/">jQuery Parents</a> by Turing School of Software and Design (<a href="http://codepen.io/turing">@turing</a>) on <a href="http://codepen.io">CodePen</a>.</p>

### `children()`

This method returns all of the direct childen of the given selection. It will _not_ search their children. Like `parents()`, `children()` will also take a selector. `$('.some-selector').children('.active')` will go through the children of the current query and only return the elements with the class of `.active`.

<p data-height="300" data-theme-id="23788" data-slug-hash="VjrOjp" data-default-tab="js,result" data-user="turing" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/team/turing/pen/VjrOjp/">jQuery Children</a> by Turing School of Software and Design (<a href="http://codepen.io/turing">@turing</a>) on <a href="http://codepen.io">CodePen</a>.</p>

### `siblings()`

`siblings()` will select all of the sibling elements based on the current query. Like its friends, it will also take a selector if you're polite.

<p data-height="300" data-theme-id="23788" data-slug-hash="Gqrapr" data-default-tab="js,result" data-user="turing" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/team/turing/pen/Gqrapr/">Siblings</a> by Turing School of Software and Design (<a href="http://codepen.io/turing">@turing</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

### `find()`

One you have queried for some elements using jQuery, you can use `find()` to drill down a little deeper.

It's useful to think of `find()` as a more powerful alternative for `children()`. The `children()` method will look only one level down the tree. `find()` will search the children, the grandchildren, the great-grandchildren, and so on. The method will look at anything you currently have selected and then search within those results.

<p data-height="300" data-theme-id="23788" data-slug-hash="dXVEpN" data-default-tab="js,result" data-user="turing" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/team/turing/pen/dXVEpN/">jQuery Find</a> by Turing School of Software and Design (<a href="http://codepen.io/turing">@turing</a>) on <a href="http://codepen.io">CodePen</a>.</p>

Which one do you use? It depends, do you want to traverse all the way down the tree or just down one level.

## Exercise, Part Three: One-Term Presidents

* Add the `green` class to anyone who served right after a president who died.
* Find all of the presidents who only served one term and add the class `red`.
* Add the class of `blue` to the parent of a checked checkbox.
* Add the class of `yellow` to the siblings of the parent (`td`, in this case) of an unchecked checkbox.

---

![part 4][part-four]

[part-four]: /assets/images/lessons/jquery/part-four.jpg

## Lecture, Part Four: Adding to the DOM

Let's take a look at some approaches of adding/changing content in the DOM.

### `text()`

jQuery's `text()` is like using the vanilla JavaScript `innerText` or `textContent`. Basically, it grabs the inner text of the element that you have selected or overwrite said content.

For example:

```js
  $('.some-element').text('New text.');
```

### `html()`

`html()` is to `text()` as `innerHTML` is to `innerText`. Basically, change the HTML contents of a bigger element, not just the text of it. As a fun experiment, select an element and try to replace the contents to `<script>alert('HACKED!');</script>` using both `text()` and `html()`. Let me know how it goes for you.

### `append()`

`html()` replaces the entire contents of an element. `append()` adds new content onto the end of it.

### `prepend()`

`html()` replaces the entire contents of an element. `prepend()` adds new content onto the beginning of it.

## Exercise, Part Four: Dead Presidents

* Find all of the presidents who died in office (hint: they have a `died` class on their `tr`).
* Append `<span class="died">(Died)<span>` to the the `term` column of presidents who have `.died`.

---

![part 5][part-five]

[part-five]: /assets/images/lessons/jquery/part-five.jpg

## Lecture, Part Five: Simple Event Binding

### Event Driven Programming

Event driven programming relies on some external action to determine how the program behaves. Some external actor (a user or another computer) takes an action, and your program responds.

### Event binding using jQuery

Let's start by looking at the [jQuery Events API](http://api.jquery.com/category/events/).

The Events API tends to mimic the native DOM events, but with some abstraction to standardize across all of the browsers in use today.

Our main focus today is going to be on the `.on()` method. As of jQuery 1.7 and later, this is the preferred method for binding events. You may see `.bind()` as well, but this support older code.

#### Knowing Which Element We Clicked

jQuery makes it do stuff to many elements ate the same time, but if we add event listeners to a bunch of boxes at the same time, then how we know which one the user clicked? Consider a situation where we have three boxes. When that particular box is clicked, we want to toggle a class. How do we know which box was clicked?

It turns out that when we add an event listener using jQuery, we get a special little variable called `this`. Lucky for us, `this` is set to the box we clicked on.

<aside>
  As you hopefully know, it's possible to select elements without jQuery. When we use jQuery our elements get a whole bunch of extra super powers. But, jQuery doesn't <em>know</em> that you want these when you're listening for an event. <code>this</code> is just the regular element. Use <code>$(this)</code> if you want to add those super powers back. At this very moment, it's probably unclear what those super powers are. So, let's just always use <code>$(this)</code> for now.
</aside>

Let's take a look at the example below:

<p data-height="300" data-theme-id="23788" data-slug-hash="EyKxpp" data-default-tab="js,result" data-user="turing" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/team/turing/pen/EyKxpp/">$(this)</a> by Turing School of Software and Design (<a href="http://codepen.io/turing">@turing</a>) on <a href="http://codepen.io">CodePen</a>.</p>

## Exercise, Part Five

As pairs, try to work through the following prompts. We'll do the first one together.

* Add an event handler to all of the checkboxes that when the box is checked, adds the `yellow` class to the parent `tr`.
* Add an event handler that adds the `red` class to a `td` element when it's clicked on.
* Modify the event handler above to remove the `red` class when it is clicked a second time.
* **Bonus**: Add a new `div` to the page, every time a checkbox is checked, add that presidents name to the `div`.
* **Bonus 2**: Remove that presidents name when the box is unchecked.

---

![form][form]

[form]: /assets/images/lessons/jquery/form.png

## Form Challenge

Let's clone down [this simple form](https://github.com/turingschool-examples/jquery-form-challenge).

```bash
git clone git@github.com:turingschool-examples/jquery-form-challenge.git
cd jquery-form-challenge
open index.html
```

Right now, it doesn't work and needs to be wired up.

1.  First, how could we select all `input`s?
2.  How could we use jQuery to fill in the value for the `.link-title`?
3.  How could we use jQuery to fill in the value for the `.link-url`?
4.  How could we click the submit button from our console?

Let's hop out of the console and actually edit the JS now.

5.  Capture the `click` event for the submit button
6.  On submit, let's get the value of the inputs
7.  Now let's append those values under `My Links`

---

![rails][rails]

[rails]: /assets/images/lessons/jquery/rails.png

## Lecture: Now in Rails

Curious about how this all works with Rails?

Check out [the presidents example in Rails](https://github.com/turingschool-examples/presidents-jquery-rails)

Additional things to think about:

* You already submit forms in rails, how exactly does that work?
* What is server-side rendering vs client-side rendering?
* What is `$( document ).ready()` [docs](https://learn.jquery.com/using-jquery-core/document-ready/)

![presidents of the usa][presidents]

[presidents]: /assets/images/lessons/jquery/presidents.jpg
