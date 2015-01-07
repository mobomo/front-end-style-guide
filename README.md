# Prelude

This is to help define the standards to be followed when developing the UI for any project.

Finally, note that much of this (but certainly not all) was originally written by Tait Brown as part of his outstanding resource, [Front End Dev Guidelines](http://taitems.github.io/Front-End-Development-Guidelines/).

# The Front-End Style Guide

## Table of Contents

* [Precompilation](#precompilation)
* [Editor Configuration](#editor-configuration)
* [Vendored UI Libraries](#vendored-ui-libraries-like-twitter-bootstrap)
* [Documentation](#documentation)
* [Naming Conventions and File Organization](#naming-conventions-and-file-organization)
* [Accessibility](#accessibility)
* [JavaScript](#javascript)
* [jQuery](#jquery-specific)
* [CSS](#css)
* [CSS3 and HTML5](#css3--html5)
* [Sass/SCSS (...and Less)](#sassscss-and-less)
* [Resources](#resources)

## Precompilation

Before we get in to the meat of this, the following standards need to be stated:

1. When producing CSS, that code should be created with the use of [Sass](http://sass-lang.com/) (using the SCSS syntax, which is the library's default).
2. When producing JavaScript, that code should be created with the use of CoffeeScript.

These are the Intridea standards, and any deviation from that should be seen as the exception and not the rule.

### Generated Code

Only with very rare exception should generated code get checked in to a code repository. The variance in output alone creates far too much noise to offer any benefit, and offers a false sense of activity and security (and is often the source of merge conflicts).

## Editor Configuration

### Editorconfig

In order to help ensure coding consistency within a project, we rely on a tool called [Editorconfig](http://editorconfig.org/). The boilerplate version that we use is stored in the [dotfiles repository](https://github.com/intridea/dotfiles), but here's what it could look like:

    # top-most EditorConfig file; set to false if not in the project root
    root = true

    [*]
    end_of_line = lf
    insert_final_newline = false
    trim_trailing_whitespace = true
    indent_style = space
    indent_size = 2
    charset = utf-8

Most popular editors have support for the [editorconfig plugin](http://editorconfig.org/#download).

### Spacing/Indentation

We follow the standard rails convention of using soft-tabs, and 2-spaces-per-tab for all JS/CS/SCSS files. No hard-tabs please. If you have the editorconfig plugin (and a matching .editorconfig file) in place, then this should be easy to respect.

Eliminate any and all trailing white-space. It's messy, and it causes Git to complain. Most editors support doing this automatically.

## Vendored UI Libraries (like Twitter Bootstrap)

If the project uses a pre-built UI Library (like [Bootstrap](https://github.com/twbs/bootstrap-sass)), then the variables and mixins included are available for use in the SCSS files. Core library functionality should be overridden, never modified. SCSS variables should be kept in `sass/_variables.scss`.

## Documentation

When creating new files, please add a mast-head and brief description of the module. It should look something like (for SCSS, in a file named `_module.scss`):

    //
    // Module Name
    // A brief description about what the module does.
    // --------------------------------------------------
    
    // First module component
    .module {
      // ...
    }

Please document as you work through the files. For SCSS, use [KSS](https://github.com/kneath/kss/blob/master/SPEC.md), and for JavaScript follow the [JSDoc](http://usejsdoc.org/about-getting-started.html)/TomDoc pattern.

## Naming Conventions and File Organization
### Files
#### CSS / SCSS

SCSS files that are created for the project should live in `sass/`. Files that are provided by a vendor library (for example, SymbolSet) should be in `sass/vendor`.

Files should be small and concise, focused on building modules. A good example of what should be contained in a file would be adding all type rules in `_type.scss`. Alternately, a file for forms is appropriate. Most files will live under the `sass/modules` directory.

Partials should be named using the `_{{filename}}.scss` pattern. Top-level files should be named `{{filename}}.css.scss`.

When writing SCSS, please use the following conventions:

* Use box-model impact as a sorting method for CSS properties. See [Andy Ford's post](http://web.archive.org/web/20130227044124/http://fordinteractive.com/2009/02/order-of-the-day-css-properties) on the reasons why.
  * An easy way to do this is to use the [CSSComb](http://csscomb.com/) tool (there are plugins for most editors; just make sure it's not set to use 'alphabetical').
* Put `@extends` declarations at the top of the property list (followed by an empty space)
* Put `@include` declarations at the bottom of the property list (preceded - and followed - by an empty line).
* Destroy all trailing whitespace.

For more details, read [this article on CSS Tricks](http://css-tricks.com/sass-style-guide).

Finally, do not check in compiled CSS.

#### JavaScript / CoffeeScript

JavaScript/CoffeeScript files that are created for the project should live in scripts. Files that are provided by a vendor library(for example, SymbolSet) should be in `scripts/vendor`.

The directory should be kept as shallow as possible. Files should be modularly built, each file containing one class at a time.

If [Backbone](http://backbonejs.org/) is to be used, then [Marionette](http://marionettejs.com/) should be used as well. As such, the module files should be kept in `scripts/modules`, with a directory for each module. The structrue should look like this:

    scripts
    |-  app.js.coffee
    `-  modules
        `-  users
            |- users-app.js.coffee
            |- users-views.js.coffee
            `- users-controller.js.coffee 

Models and Collections are to be defined in a single file, stored in an `entities` directory.

    scripts
    |-  app.js.coffee
    |-  entities
    |   `- users.js.coffee
    `-  features
        `-  users
            `- ...

If using CoffeeScript, do not check in compiled JS.

## Accessibility

### Use a DOCTYPE

The use of a DOCTYPE is incredibly important, and the omission of it is unacceptable. This will help define which version of (X)HTML is used to process the document, and (importantly) will help prevent IE from rendering in Quirks Mode. [The W3C answers the question of "why"](http://www.w3.org/QA/Tips/Doctype) with:

> Why specify a doctype? Because it defines which version of (X)HTML your document is actually using, and this is a critical piece of information needed by some tools processing the document.
>
> ...
> But the most important thing is that with most families of browsers, a doctype declaration will make a lot of guessing unnecessary, and will thus trigger a "standard" rendering mode.

If you're writing an XHTML-compliant page, you need to use the following `DOCTYPE`:

    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
    "http://w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

Ideally, however, you should be using HTML5, which has a much leaner `DOCTYPE`:

    <!DOCTYPE html>

### Write Valid Semantic Markup

Writing websites with clean, semantic HTML is something we wish we could always do. Sometimes we find ourselves limited by the way pages were setup by our predecessors, or sometimes we're coding an HTML email. The validity of the HTML should never be compromised, even if to solve a browser specific bug.

Headings should be heirarchically created from `<h2>` onwards, paragraphs should always be in `<p>` tags and so on and so forth. If you write semantic HTML, the resultant page will be cleaner, lighter and easily parsed by search engine spiders. This is one of the simplest SEO fixes you can undertake.

#### Which Do You Think Looks Cleaner, This?

    <span class="sectionHeading">A Heading</span>
    <br /> <br />
    Lorem ipsum dolor sit amet. ...
    <br /> <br />

#### Or This?

    <h2>A Heading</h2>
    <p>
      Lorem ipsum dolor sit amet. ...
    </p>

### Fallbacks for Middle Mouse Clicks

One of the most frustrating accessibility and usability flaws of the modern web stems from the remapping of hyperlink click functions. Elements that appear to be hyperlinks may have their single click functionality remapped via JavaScript, breaking middle mouse click (open in new tab) functionality. If they can be opened in a new tab, their href of a single hash sends you back to the same page.

A modern example of a popular website that is contributing to this problem is the Twitter web app. Middle mouse clicking of names or user avatars yields completely different results throughout the web app.

    <!-- The old way, breaking the web -->
    <a href="#"></a>

    <!-- If you can't deliver a page on mouse click, it's not a hyperlink -->
    <button type="button" ng-click="doSomething()"></button>
    <span class="link" role="link"></span>

Another alternative is the use of "hashbangs", that remap normal URLs to hash links and fetch pages via AJAX. Libraries that provide hashbang functionality should be able to display the page normally when middle mouse clicked, or load the content from that page into a designated area when clicked normally. But tread carefully, there are plenty of people who believe [hashbangs are breaking the web](http://isolani.co.uk/blog/javascript/BreakingTheWebWithHashBangs).

### Use Microformats

Microformats are a way of making contact information machine readable. hCard classes (not vCard) are used to define the type of content contained within elements. These are then extracted or highlighted by the browser.

    <span class="tel">
      <span class="type">home</span>:
      <span class="value">+1.415.555.1212</span>
    </span>

If you were to navigate to a page that uses this, you would notice that a program like Skype will easily detect what numbers on the page are phone numbers. Mobile Safari does something similar on iOS devices.

For more information: [http://microformats.org/wiki/hcard](http://microformats.org/wiki/hcard)

### Images Need 'Alt' Text

The `<img>` tag requires `alt` text to both validate and meet accessibility guidelines. The text in the `alt` attribute should be descriptive of what the image shows, or is trying to achieve, unless of course the image is not critical.

If the image is of a list bullet or other trivial icons, it is recommended to simply leave the `alt` attribute empty, but still present. A screenreader will then ignore it, as opposed to having to read out "bullet" 20 times.

    <img src="dog.gif" alt="Fido and I at the park!" />
    <!-- good, descriptive -->
    
    <img src="bullet.gif" alt="bullet" />
    <!-- bad, as silly as it seems -->
    
    <img src="bullet.gif" alt="" />
    <!-- good -->

### Use Tables for Tabular Data Only

Tables should only ever be used for the presentation of tabular data. The only exception is when composing HTML email, in which a table is almost the only thing supported by soul crushing email clients.

For accessibility, table headers should always be presented using `<th>` elements. Remember to also set `cellpadding`, `cellspacing`, and `border` values to `0` as these are more consistently controlled by CSS.

    <table cellpadding="0" cellspacing="0" border="0">
      <thead>
        <tr>
          <th>
            Cell Header
          </th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>
            Cell Item
          </td>
        </tr>
      </tbody>
    </table>

### Use jQuery & jQuery UI Widgets

jQuery and jQuery UI are constructed to look and behave as close to identical as possible on different browsers. jQuery UI is designed to be WAI WCAG 2.0 and WAI ARIA compliant, so using the framework removes any uncertainty about plugins or scripts running on your site.

## JavaScript

It should be noted that CoffeeScript is the preferred tool for writing out JavaScript. These guidelines are listed first for a few reasons: first, CoffeeScript is eventually output as JavaScript, so many of the rules still apply, and second, there are often times where JavaScript needs to be written rather than CoffeeScript.

Rules specific to CoffeeScript will follow this section.

### Whitespacing & Formatting

Any discussion about formatting, whitespacing and the placement of braces is going to be hotly debated. I guess the simplest rule is that, unless you're willing to completely format a whole document, **respect and maintain the formatting of an existing document**. That means: see same-line braces throughout a JS file, continue to write code with same-line braces. Your code should fail the code review process if it doesn't maintain consistency with the rest of the document.

Consistent formatting makes code more readable, and also means the code can be easily modified with find and replace commands. The coding habits we have picked up are thankfully very similar to what jQuery officially encourages. There are a few minor discrepencies, but again, these are personal issues or things that we think cannot be maintained. [Further Reading](http://docs.jquery.com/JQuery_Core_Style_Guidelines).

#### Character Spacing

    // Bad
    if(blah=="foo"){
      foo("bar");
    }
    // Good
    if (blah == "foo") {
      foo("bar");
    }

#### Same Line Braces

    // Bad
    if (foo)
    {
      bar();
    }
    // Good
    if (foo) {
      bar();
    }

#### Always Using Braces

    // Bad
    if (foo)
      bar();
    // Good
    if (foo) {
      bar();
    }

#### String Handling

**Strings should always use double quotes**. Some people are very fond of their C style strings (single quotes), but this leads to conflicting styles within a script. C style string handling dictates that empty and single character strings should be wrapped in single quotations, while phrases and words should be wrapped in double quotations.

#### Variable Declarations

In JavaScript, when defining variables, use comma-separated lists, placed at the top of the function (effectively [hoisting](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20&%20closures/ch4.md) them manually):

  // Bad
  function someOtherFunc() {
    // function logic ...
    var firstVar = 1;
    // more function logic ...
    var secondVar = 2;
    var thirdVar = 3;
    // more function logic ...
  }
  // Good
  function someFunc() {
    var firstVar  = 1,
        secondVar = 2,
        thirdVar  = 3;

    // other function stuff here...
  }

### Always Specify the `radix` Parameter When Using `.parseInt()`

When parsing a string to an integer, it is considered good practice to specify the `radix` parameter - which determines to what base the string should be converted to. The default setting will trigger a radix of 16 whenever the string is lead by a 0. Most beginner and intermediate users are only ever going to be using a radix of `10`.

    alert( parseInt("08") ); // alerts: 2
    alert( parseInt("08", 10) ); // alerts: 8

### Avoid Comparing to true and false

Direct comparison to the values of true and false is unnecessary. Sometimes it might be good for clarity, but it's just extra code.

    if (foo == true) {
    // bad as it's redundant
    }
    
    if (foo) {
    // yay!
    }
    
    if (!bar) {
    // the opposite
    }

### Avoid Polluting the Global Namespace

An over-reliance on global variables is something all of us, are guilty of. Arguments as to why globals are bad are fairly straight forward: the chance of script and variable conflicts is increased, and both the source file and the namespace itself become littered with countless ambiguously named variables.

[Douglas Crockford](http://yuiblog.com/) believes that the quality of a JavaScript application can be assessed by the number of global variables it uses; the less the better. Given that not everything can be a local (but let's be honest, that one you're thinking about right now, it can, don't be lazy) you need to find a way of structuring your variables to prevent clashes and minimize the bloat. The easiest way is to employ a single variable or a minimal amount of modules on which the variables are set. Crockford mentions that YUI uses a single global, `YAHOO`. He discusses this in more detail in his blog post "[Global Domination](http://yuiblog.com/blog/2006/06/01/global-domination/)".

Considering that, in the case of small web apps, globals are generally used to store application-wide settings: it's generally better to namespace your project or settings as objects.

    // polluted global name space
    var settingA = true;
    var settingB = false;
    var settingC = "test";
    // a settings namespace
    var settings = {
      settingA: true,
      settingB: false,
      settingC: "test"
    }

But if we're avoiding globals to reduce the chance of conflicts, isn't standardizing the namespaces to be the same going to increase chance of one app's settings overwriting another's? Well, it would make sense. It is instead suggested that you namespace your globals to your own specific app name, or reassign your namespace much in the same way that jQuery uses `$.noConflict()` mode.

    var myAppName = {
      settings: {
        settingA: true
      }
    }
    //accessed as
    myAppName.settings.settingA; // true

### Camel Case Variables

The camel casing (or *camelCasing*) of JavaScript variables is accepted as the standard in most coding environments. The only exception is the use of uppercase and underscores to denote constants.

    var X_Position = obj.scrollLeft;
    var xPosition = obj.scrollLeft; // tidier
    SCENE_GRAVITY = 1; // constant

### Loop Performance - Cache Array Length

Looping is arguably the most important part of JavaScript performance to get right. Shave a millisecond or two off inside of a loop, potentially gain seconds overall. One such way is to cache the length of an array so it doesnt have to be calculated every time the loop is iterated through.

    var toLoop = new Array(1000);
    
    for (var i = 0; i < toLoop.length; i++) {
      // BAD - the length has to be evaluated 1000 times
    }
    
    for (var i = 0, len = toLoop.length; i < len; i++) {
      // GOOD - the length is only looked up once and then cached
    }

#### The Exception

If you're looping through an array to find and remove a particular item, this will alter the array length. Any time you change the array length by either adding or removing items from inside the loop, you will get yourself into trouble. Consider either re-setting the length or avoid caching it for this particular situation.

### Loop Performance - Use 'break;' & 'continue;'

The ability to step over and out of loops is really useful in avoiding costly loop cycles.

If you're looking for something inside of a loop, what do you do once you find it? Say the condition you're looking for is matched halfway through a 1000 item loop. Do you execute whatever you intend to do, and allow the loop to continue to iterate over the remaining 500 items, knowing that there's no chance it will hit an if statement? Nope! You break out of your loop, literally!

    var bigArray = new Array(1000);
    for (var i = 0, len = bigArray.length; i < len; i++) {
      if (i == 500) {
        break;
      }
      console.log(i); // will only log out 0 - 499
    }

Another problem is skipping over a particular iteration and then continuing on with the loop. While things like odds and evens are better managed by replacing `i++` with `i + 2`, some conditions need to be specifically listened for, to then trigger the skip. Anything that prevent's running through an entire iteration is pretty handy.

    var bigArray = new Array(1000);
    for (var i = 0, len = bigArray.length; i < len; i++) {
      if (condition) {
        continue;
      }
      doCostlyStuff();
    }

### Don't Send Too Many Function Parameters

This is a pretty bad idea, more for readability than anything:

    function greet(name, language, age, gender, hairColour, eyeColour) {
      alert(name);
    }

It's a much better idea to construct an object before-hand or to pass the object inline

    function greet(user) {
      alert(user.name);
    }
    greet({
      name: "Bob",
      gender: "male"
    });

### Remap 'this' to 'self'

When writing object-oriented (OO) JavaScript, the scope of `this` must be understood. Regardless of what design pattern you choose to structure your pseudo-classes, a reference to this is generally the easiest way to refer back to an instance. The moment you begin integrating jQuery helper methods with your pseudo-classes is the moment you notice the changing scope of this.

    Bob.findFriend("Barry");
    
    Person.prototype.findFriend = function(toFind) {
      // this = Bob
      $(this.friends).each(function() {
        // this = Bob.friends[i]
        if (this.name == toFind) {
          // this = Barry
          return this;
        }
      });
    }

In the above example, `this` has changed from a reference to `Bob`, to his friend `Barry`. It's important to understand what happened to the value of `this` over time. Inside of the prototyped function, `this` refers to the instance of the pseudo-class (in this case `Bob`). Once we step inside the `$.each()` loop, this is then re-mapped to be item `i` in the parsed array.

The solution is to remap the value of `this` to either `self` or `_self`. While `self` (sans underscore) is not exactly a [reserved keyword](https://developer.mozilla.org/en/JavaScript/Reference/Reserved_Words), it `is` a property of the `window` object. Although my use of `self` was picked up from the jQuery source code, they have realized their mistake and are attempting to [rectify the situation](http://bugs.jqueryui.com/ticket/5404) and instead use `_self`. Personally, I prefer the use of `self` for the sheer cleanliness - but it can throw some pretty confusing bugs for people. Tread carefully.

In the following example I will better utilize the parameters made available with the `$.each()` helper, as well as re-mapping the value of `this`.

    Bob.findFriend("Barry");
    Person.prototype.findFriend = function(toFind) {
      // the only time "this" is used
      var _self = this;
    
      $(_self.friends).each(function(i,item) {
        if (item.name == toFind) {
          return item;
        }
      });
    }

### All Bound Up

Now that you know how to properly store a reference to this when changing scope, you should know that if you're storing this as `_self` in order to access a function, you should instead bind the function and store the bound function in a variable. Using bind, which is an ECMAScript 5 feature (made available to non-ES5 environments through the use of libraries such as [Underscore.js](http://underscorejs/) or [Lo-Dash](http://lodash.com/)), will preserve the context of "this" when used with that function.

    this.x = 9;
    var module = {
      x: 81,
      getX: function() { return this.x; }
    };

    module.getX(); // 81
    var getX = module.getX;
    getX(); // 9, because in this case, "this" refers to the global object

    // create a new function with 'this' bound to module
    var boundGetX = getX.bind(module);
    boundGetX(); // 81

### Storing Booleans

Booleans should be easily identifiable by the way they are named. Use prefixes like `is`, `can` or `has` to propose a question.

    isEditing = true;
    obj.canEdit = true;
    user.hasPermission = true;

### Minimizing Repaints & Reflows

Repaints and reflows relate to the process of re-rendering the DOM when particular properties or elements are altered. Repaints are triggered when an element's look is changed without altering its layout. Nicole Sullivan describes these changes in a thorough [blog post](http://www.stubbornella.org/content/2009/03/27/reflows-repaints-css-performance-making-your-javascript-slow/) as style changes such as visibility or background-color. Reflows are the more costly alternative, caused by changes that alter the layout of the page. Examples include the addition or removal of elements, changes to an element's width or height, and even resizing the browser window. Worst yet is the domino effect of reflows that cause ancestor, sibling and child elements to reflow.

There is no doubt that both reflows and repaints should be avoided if possible, but how?

#### A Reflow Example

It's not that the following snippet is "bad code" exactly. But let's assume that the array arr has 10 items.

    var myList = document.getElementById("myList");
    for (var i = 0, len = arr.length; i < len; i++) {
      myList.innerHTML += "<li>" + arr[i].title + "</li>"; //reflow - appending to element
    }

In the above for loop, a reflow will be triggered for every iteration of the loop. 10 iterations cause 10 reflows.

Now consider the following:

    var constructedHTML = "";
    for (var i = 0, len = arr.length; i < len; i++) {
      constructedHTML += "<li>" + arr[i].title + "</li>"; //no reflow - appending to string
    }
    document.getElementById("myList").innerHTML = constructedHTML; //reflow

In this scenario, the elements are being constructed within a string. Not a single reflow is created by the loop, as the DOM is not being altered. Only once the array has been completely looped through is the string then applied as the `innerHTML` of an object, causing the only reflow of the function.

There are endless types of reflows and repaints that can be avoided, and lucky you gets to go on an read about them. Reading material on the subject matter is plentiful, but most of it is linked to from the excellent starting point that is Nicole Sullivan's [blog post](http://www.stubbornella.org/content/2009/03/27/reflows-repaints-css-performance-making-your-javascript-slow/). There are important lessons to be taken away from this when it comes to a multitude of technologies synonymous with "web 3.0" and HTML5. The lesson above can be directly applied to writing jQuery. It's also important to consider when fiddling with `canvas`, and trying to keep a frame rate in the 30-60 range.

### Don't Use Milliseconds to Generate Unique IDs

There is a method for generating unique IDs that has hung around since the early days of web dev. It involved appending the elapsed milliseconds since January 1, 1970 to your static ID by way of:

    var myID = "static" + new Date().getTime();

This was a fairly foolproof method originally, because even if two ofthe above lines were performed one after the other, a few millisecondsnormally separated their execution. New browsers brought with them newJavaScript engines, coupled with ever increasing clock speed. Thesedays it's more likely that your milliseconds match than are slightlyincremented.

This leads to bugs that are near impossible to debug by conventionalmeans. Because your DOM is created on the fly, traditional validationof the page source won't identify multiple IDs as an error. JavaScriptand iQuery error handling dictates that the first match for the IDwill be utilized and other matches ignored. So it doesn't even throw aJS error!

No, the only real method to debug it is line by line breakpoint ingand logging - but "pause" at the wrong line and your milliseconds willno longer clash!

The good thing is that there are plenty of alternatives. To be pedantic, it's worth noting that a computer's random function is not truly random as it is seeded by system time- but the probability of clashes is rather minuscule.

    var myID = "static" + Math.round(Math.random() * 10000);

Personally, I'm partial to a bit of faux GUID generation. Technically a GUID is generated according to your hardware, but this JavaScript function does the next best thing. The following is a handy function I've borrowed from a [stack overflow post](http://stackoverflow.com/questions/105034/how-to-create-a-guid-uuid-in-javascript).

    function S4() {
    return (((1+Math.random())*0x10000)|0).toString(16).substring(1);
    }
    
    function guid() {
    return (S4()+S4()+"-"+S4()+"-"+S4()+"-"+S4()+"-"+S4()+S4()+S4());
    }
    
    var myID = "static" + guid();

### Feature Sniff, Don't Browser Sniff

Does the client's browser support geolocation? Does the client's browser support web workers? HTML5 video? HTML5 audio? The answer used to be:

    if ($.browser.msie) {
      // no it doesn't
    }

But things are rapidly changing. The latest version of IE is almost a modern browser, but as usual it's making front end development a pain. Earlier versions of IE were generally as equally sucky as their predecessors, so it enabled lazy JavaScript developers to simply detect `if (ie)` and execute some proprietary Microsoft slops syntax. Now IE9 has done away with these functions, but that old `if (ie)` chestnut is throwing a spanner in the works.

So what if you could detect support for individual features without sniffing the (unreliable and cloakable) user-agent?

If you answered "*that would be ghetto*", then you are correct.

In steps [Modernizr](http://www.modernizr.com/), a JavaScript library developed in part by industry dream-boat Paul Irish. With wide adoption, tiny file-size and plenty of [documentation](http://www.modernizr.com/docs/#s1): implementing it is a no-brainer. It creates a `Modernizr` object that contains the results of its detection tests, so checking feature support is as simple as the following:

    // old way of detecting canvas support
    if (!!document.createElement('canvas').getContext) { ... }

    // with Modernizr
    if (Modernizr.canvas) { ... }

### Readable Milliseconds

A handy way of writing milliseconds in a readable format. Great for beginners, but mostly a gimmick.

    // is this 3, 30 or 300 seconds?
    var timeout = 30000;
    // an extra calculation, but easier to read and modify
    var timeout = 30 * 1000;

## jQuery Specific

### Chain, Chain, Chain

One of the best parts of jQuery is its function chaining. You've probably used it a bit, maybe a few simple calls one after another&hellip; but have you ever traversed the DOM like a mad dog? Take some time to familiarize yourself with the [.end()](http://api.jquery.com/end/) function. It is critical for when you begin stepping up and down the DOM tree from your original selector.

    $(".quote")
      .hide()
      .find("a").text("Click here").bind("click",doStuff).end()
      .parent().removeClass().addClass("testimonial").draggable().end()
      .fadeIn("slow");

In the example above, the [.end()](http://api.jquery.com/end/) function is used once we have finished doing things with a particular DOM object and want to traverse back up the DOM to the original object we called. We then load back up and dive back into the DOM.

### Using `data-*` Attributes

Those of you who have been writing JavaScript (and not jQuery) for a good length of time are most likely familiar with attributes. Setting them. Getting them. Abusing `rel` and `title` instead&hellip;

So when *isn't* HTML5 or jQuery coming the rescue? New specs allow the use of `data-` prefixes on HTML elements to indicate attributes which can hold data, and jQuery does an awesome job of converting the designated string into the correct JavaScript type. It's a beautiful partnership. Let's create a `DIV` with some data attributes.

    <div id="test" data-is-bool="true" data-some-number="123"></div>

Now even though our values are wrapped in quotation marks, they won't be handled as strings:

    typeof $("#test").data("isBool"); // boolean
    
    typeof $("#test").data("someNumber"); // number

#### Special Casing

It's also important to notice the lower casing required to get these snippets to work. But if you're a great front end developer, you will still want to camel case your data variables. Like many places in JavaScript, a preceding hyphen signifies camel casing of the next letter. The following camel casing of the HTML attribute **does not work** and the same JavaScript used above will return `undefined`.

**Does not work**

    <div id="test" data-isBool="true" data-someNumber="123"></div>

**Does work**

    <div id="test" data-is-bool="true" data-some-number="123"></div>

#### '`.stop()`' Collaborate & Listen

Binding jQuery animations to mouse events is a key part of modern web-based user interaction. It's also something that you see done poorly on even the most famous of web sites. [This article](http://www.learningjquery.com/2009/01/quick-tip-prevent-animation-queue-buildup) provides a straight forward example of built up animations and demonstrates how visually jarring they can be. Thankfully it's easily fixed with a single function prefix or a parameter added to `$.animate` calls.

When using `$.animate`, `queue: false` can be added to the parameters to prevent chaining. Animation shortcuts such as `$.fadeIn` or `$.slideDown` do not take `queue` settings. Instead you have to pre-empt these animations with the `$.stop` method of pausing currently executing animations. Certain scenarios require the animation to stop dead in its tracks, or to jump to the end of the transition. It is recommended you familiarize yourself with the [documentation](http://api.jquery.com/stop/) of the parameters `clearQueue` and `jumpToEnd`, because god knows I can't help you there.

    $("selector").stop(true,true).fadeOut();
    $("selector").animate({
      property: value
    }, {
      duration: 1000,
      queue: false
    }

#### Optimize Your Selectors

jQuery is pretty laid back. It can do pretty much everything but make you coffee, and I hear that's in the roadmap for 2.0. One thing you have to be careful about is abusing the power that is the [sizzleJS](http://sizzlejs.com/) selector engine. There are two strategies to overcome this: *caching the selector results* and *using efficient selectors*.

##### Caching Selector Results

Do a costly DOM query every time you want to change something, or store a reference to the element? Pretty clear choice.

    // before
    $(".quote a").bind("click", doStuff); // DOM query eww
    // now
    $(".quote a").addClass("quoteLink"); // DOM query eww
    // later
    $(".quote a").fadeIn("slow"); // DOM query eww

Ignoring chaining, this is better:

    // before
    var $quoteLinks = $(".quote a"); // the only DOM query
    $quoteLinks.bind("click", doStuff);
    // now
    $quoteLinks.addClass("quoteLink");
    // later
    $quoteLinks.fadeIn("slow");

#### Using Efficient Selectors

So jQuery/sizzleJS can use CSS3 selectors well, but what's the real cost? Behind the scenes the browser is hopefully using `document.querySelector()`, but there's also a fair chance it will be breaking down your selector string and querying the DOM manually.

    // an ID search is the quickest possible query, then it just takes a list of the childNodes and matches the class
    $("#quoteList").children(".quotes");
    // looks for the "foo" class only in the pre-defined bar element
    $(".foo",bar);

#### A 'for' Loop is Always Quicker Than an 'each()' Loop

No matter what happens in the next few years of browser development, a native `for` loop will always be quicker than a jQuery `$.each()` loop. When you think of what jQuery really is (a library wrapped around native JS functions) you begin to realize that the native underlying JavaScript is always going to be quicker. It's a tradeoff of run speed versus authoring speed.

It is vital that a native `for` loop is always used for performance critical functions that could fire potentially hundreds of times per second. Examples include:

* Mouse movement
* Timer intervals
* Loops within loops

### CoffeeScript

TBD

## CSS

### Understanding the Box Model is Key

The "box model" is a key determining factor in how a browser renders your page. A healthy understanding of it's intricacies will make your job so indescribably easier. The box model denotes the way in which the physical dimensions of a HTML element are calculated. If a block element has a fixed width of say, 100px, then how should the padding, border and margin be placed?

Plenty of websites offer in depth descriptions, but put simply: the standards compliant implementation places the border and padding outside of the specified width. It's best explained with a graphic. Consider this code:

    /* the old way (178 + 20 + 2 = 200) */
    .foo {
      width: 150px;
      height: 150px;
      padding: 25px;
      border: 25px solid;
      margin: 20px;
    }

#### What You Would Expect (Quirks Mode)

The padding and border are calculated inward, preserving the height and width specifically set to be 150px.

![Box Model in QuirksMode](images/box-model-quirks.png)

#### What You Get (Standards Compliant Mode)

Instead, you get 250px. 150px + (2 * 25) + (2 * 25).

![Box Model in Standards Mode](images/box-model-standard.png)

If you think it seems odd, you're not alone. There is a fix at hand, and it involves a CSS property called `box-sizing`, and it works in **IE8 and above**. It allows you to choose the exact way in which an elements dimensions are calculated, and its a lifesaver. Parameter support varies and vendor prefixes apply, so consult [caniuse](http://caniuse.com/css3-boxsizing) for specifics.

    /* the old way (178 + 20 + 2 = 200) */
    .foo {
      width: 178px;
      padding: 10px;
      border: 1px;
    }
    
    /* a better way */
    .foo {
      width: 200px;
      padding: 10px;
      border: 1px;
      -webkit-box-sizing: border-box;
         -moz-box-sizing: border-box;
              box-sizing: border-box;
    }

While it was always possible to mentally calculate widths by removing pixel units from each other (as per the first method), it was never entirely clear how to do so with variable width units like percentages and EMs. There was no other solution at this point besides wrapping elements in parent elements to ensure widths and `padding`/`margin`/`borders` could all be separate.

### Know when to Float, and when to Position

Gone are the days of table based layouts. The moment we admit that we can concentrate our efforts into better understanding the way floats and positions work. There's a particular mental model that needs to be grasped, and I believe this is something best done with practise.

Floats are great for sucking elements out of the DOM and forcing them hard up against a left or a right edge. They became the bread and butter of the post table layout stage in front end dev, possibly because of the poor browser support of `display: inline` and `inline-block`, as well as `z-index` bugs stemming from position support. These days there really is no excuse. Inline-block is fairly well supported, and a quick hack will get it working in IE7.

The arguments that previously held back absolutely positioning elements with CSS have thankfully died down. In theory, positioning allows you to place elements on a page (or within any container for that matter) with Xs and Ys in a straightforward manner that should be familiar to people like Flash developers.

#### Understanding Positions

It's important to understand one fact when positioning elements with CSS: the position is always relative to the nearest positioned parent element. When people first start dabbling with CSS, there's a common misconception that `position: absolute;` positions right up to the page root. I think this stems from the fact that, yes, without any parent elements with position styles - this is true. It traverses up the DOM tree, not finding any positioned elements, and settles on the root of the page.

So if `position: absolute;` pulls elements out of their normal flow, how do you position an element relative to it's parent? That's straight forward. The parent element needs to by styled `position: relative;`, and then all child elements will draw from the top, right, bottom and left of this parent container. Using this knowledge, how would you go about achieving the following straightforward layout?

![Float or Position?](images/float-or-position.png)

Using `float`, you would need to wrap the items in a clearfix, float `.one` left, and fiddle with floats and margins on both `.two` and `.three`. You would end up with something similar to the following:

    .parent {
      /* poor man's clearfix */
      width: 310px;
      overflow: auto;
    }
    .one {
      width: 200px;
      height: 210px;
      float: left;
    }
    .two {
      width: 100px;
      height: 100px;
      float: right;
      margin-bottom: 10px;
    }
    .three {
      width: 100px;
      height: 100px;
      float: right;
    }

As mentioned earlier, there are `z-index` issues to be considered. While the above example might seem a bit excessive, once you start thinking with positions, it will opens a world of possibilities.

### Whitespacing

Whitespacing of CSS can be difficult as we chop and change between single and multi line CSS arguments. I'm not going to get into that.

#### Proper Spacing

    /* BAD */
    .selector {display:none;background:#ff0000;color:#000000;}
    
    /* GOOD - SINGLE LINE */
    .selector { display: none; background: #ff0000; color: #000000; }
    
    /* GOOD - MULTI-LINE */
    .selector {
     display: none;
     background: #ff0000;
     color: #000000;
    }

#### Same Line Braces

    .selector {
      display: none;
      background: #ff0000;
      color: #000000;
    }

#### Grouping & Indenting Vendor Prefixes

    .selector {
      background: #fff;
      border: 1px solid #000;
      color: #eaeaea;
      -webkit-border-radius: 3px;
        -moz-border-radius: 3px;
              border-radius: 3px;
    }

### IDs and Classes

IDs should be written in camelCase (as is true of JavaScript and CoffeeScript), and are not to be used for styling. We build components, building-blocks with which we can construct our applications. IDs are single-use. IDs should be used to target DOM nodes with JavaScript.

Class names should be written using dash-separation, and if a class is to be used for a JavaScript behavior (state change, hiding/showing, etc), it should be prefixed with `.js-` and should not have any styles (save those needed for the JavaScript behavior) applied to it.

    /* Bad */
    #some-element
    .someElement
    
    /* Good */
    #someElement
    .some-module
    .js-toggle-visibility

### Build from small to large

Build up styles starting with the smallest of pieces. If you're building a contact page, build the button, then the form. The header and footer of the page should not need to know anything about the form, even if a form lives in the footer.

Naming should follow the [SMACSS](http://smacss.com/) methodologies / [OOCSS](https://github.com/stubbornella/oocss/wiki) principles. Focus on creating re-usable components, and not crafting hyper-specific styles.

Finally, everyone should read this article on [Cleaning Out Your SASS Junk Drawer](http://blackfalcon.roughdraft.io/4436524-clean-out-your-sass-junk-drawer).

### CSS Shorthand
#### Grouping Properties

Grouping properties together is one of the single most effective methods to greatly reduce the size of a CSS file. It's important to understand how properties are ordered (clockwise - top, right, bottom, left) and how they can be further shortened (top and bottom, left and right). 

    /* LONG CODE IS LONG */
    padding-top: 1px;
    padding-right: 2px;
    padding-bottom: 1px;
    padding-left: 2px;
    
    /* BETTER */
    padding: 1px 2px 1px 2px;
    
    /* BEST */
    padding: 1px 2px;
    
    // Alternate SCSS-style
    padding: {
      top: 1px;
      right: 2px;
      bottom: 1px;
      left: 2px;
    }

#### From 0px to Hero

Assigning a unit type to a property value of zero is redundant. It is not important to know whether an element should be `0px` from the left or `0 elephants` from the left, just that it's flush left.

    /* BAD */
    padding: 0px 10px;
    
    /* GOOD */
    padding: 0 10px;

#### Commenting Blocks

Commenting large blocks of CSS is a great way of keeping track of multiple style areas within the one stylesheet. Obviously it works better with single line CSS styles, but the effect is not entirely lost on multi-line either. The use of dashes versus equals versus underscores are all up the individual, but this is how I like to manage my stylesheets.

    /*
     * Horizontal Nav
     * --------------
     **/
    #horizNav {
      width: 100%;
      display: block;
    }
    
    #horizNav li {
      display: block;
      float: left;
      position: relative;
    }
    
    #horizNav li a {
      display: block;
      height: 30px;
      text-decoration: none;
    }
    
    #horizNav li ul {
      display: none;
      position: absolute;
      top: 30;
      left: 0;
    }
    /*
     * Home Page - Carousel
     * --------------------
     **/
    #carousel {
      width: 960px;
      height: 150px;
      position: relative;
    }
    
    #carousel img {
      display: none;
    }
    
    #carousel .buttons {
      position: absolute;
      right: 10px;
      bottom: 10px;
    }

### Clearing Floats

Clearing a `<div>` used to mean extra DOM, because it involved adding an extra clearer element. The better way is to set a specific width on the parent element ("auto" doesn't work in all browsers and scenarios) and an overflow value of either "auto" or "hidden". "Hidden" obviously degrades better, but there are some IE compatibility versions where "auto" works better.

#### The HTML:

    <div class="parentElement">
      <div class="childElement">
        I'm floated left!
      </div>
      I'm normal text that wraps around the float
    </div>

#### The CSS:

    .parentElement {
      width: 100%;
      overflow: hidden;
    }
    .childElement {
      float: left;
    }

Alternately, the [micro clear-fix](http://nicolasgallagher.com/micro-clearfix-hack/) is considered stable and cross browser compliant enough to make it to the latest HTML5 boiler plate release. I **highly** recommend you check it out. Although I am not a massive fan of browser-specific CSS and pseudo elements such as `:after`, the micro clearfix is definitely more robust. It also prevents top margins from collapsing which is an absolute life saver.

### Vertical & Horizontal Centering

Centering elements horizontally is not exactly rocket science, and I'm sure most of you are familiar with the following snippet:

    .class {
      width: 960px;
      margin: 0 auto;
    }

Front end devs have been using this snippet for a long time, without fully understanding why it didn't work vertically. From my understanding, it's important to remember that the parent element will generally have a `height: auto;` on it, and that there is no 100% height in which to vertically center the element. Applying the `position: absolute;` effectively moves the element out into position mode and responds to the pushing and pulling of auto margins and no specific location. 

    .exactMiddle {
      width: 100px;
      height: 100px;
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      margin: auto;
    }

The downsides of this method include its lack of support in IE6 and IE7, and the absence of a scroll bar if the browser is resized to be smaller than the centered object. There are more methods on [this page](http://blog.themeforest.net/tutorials/vertical-centering-with-css/) (this is Method 4), but this is by far the best.

The vertical centering of text in an element is also straightforward. If the text is on a single line, like a horizontal navigation item, you can set the `line-height` to be that of the element's physical height.

    #horizNav li {
      height: 32px;
      line-height: 32px;
    }

### Feature Sniff, Don't Browser Sniff

In the earlier discusison of JavaScript feature detection, applying properties if a browser is *any version* of IE is increasingly problematic. Man-of-steel Paul Irish pioneered the use of [IE version](http://paulirish.com/2008/conditional-stylesheets-vs-css-hacks-answer-neither/) sniffing to address this problem, but [Modernizr](http://www.modernizr.com/) has since come to the rescue. Modernizr places classes on the root `<html>` element specifying whether features are supported. Bleeding edge styles can then easily cascade from (or be removed from) these classes.

    .my_elem {
      -webkit-box-shadow: 0 1px 2px rgba(0,0,0,0.25);
         -moz-box-shadow: 0 1px 2px rgba(0,0,0,0.25);
              box-shadow: 0 1px 2px rgba(0,0,0,0.25);
    }
    
    /* when box shadow isn't supported, use borders instead */
    .no-boxshadow .my_elem {
     border: 1px solid #666;
     border-bottom-width: 2px;
    }

## You're Not !important

A reliance upon the `!important` tag is a dangerous thing. The cases that warrant its use are rare and specific. They revolve around the necessity to override another stylesheet which you do not have access or permission to edit. Another scenario is hard coding an element's styles to prevent inline JavaScript styles from taking precedence. Instead `!important` is used as a lazy shortcut to set the priority of your style over another, causing headaches further down the line.

The use of the `!important` tag can be mostly avoided via the better understanding of CSS selector precedence, and how to better target elements. The more specific the selector, the more likely it will be accepted as the applicable style. The following example from vanseodesign demonstrates the specificity at work.

    p { font-size: 12px; }
    p.bio { font-size: 14px; }

[Their article](http://www.modernizr.com/) on style precedence does a better job explaining inheritence than I ever could, so please give it a go.

### Aggressive Degradation

It's worth noting that this is a personal opinion, and best suited to very specific situations. The stance of aggressive degradation will not be well received in large commercial projects or enterprise solutions relying upon older browsers.

Aggressive degradation dictates that if a particular (older) browser cannot render a certain effect, it should simply be omitted. A CSS3 button is a good example. Effects such as `border-radius`, `box-shadow`, `text-shadow`, and `gradients` will be displayed in cutting edge browsers. A graceful fallback of a `.PNG` would be provided for slightly older browsers, and the most graceful of all solutions would include a PNG-Fix for IE6 or the use of `filter` arguments to replicate gradients and shadows. However, aggressive degradation in this situation instructs you to neglect the older browsers and present them with a flat, satisfactory object.

Put simply, aggressive degradation boils down to: **if your browser can't render a gradient or a box shadow, tough luck**.

While not ideal for every situation, it ensures the timely delivery of projects and that the root product is still usable and not reliant on (validation breaking) hacks.

## CSS3 & HTML5
### Feature Sniff with Modernizr

I think I've gone on enough about this already. Use [Modernizr](http://www.modernizr.com/) to detect the availability of specific HTML5 and CSS3 features.

### `@font-face` Use and Abuse

Before you consider embedding a custom font, is important that you inspect the EULA and check if web embedding is allowed. Foundries are understandably reluctant to allow designers and developers the ability to place font files directly on a server which can then be copied by a savvy end user. Particular foundries also prohibit the embedding of particular file types, such as `.TTF` and `.OTF`.

If, after careful consideration, you believe the desired font is web embeddable: head on over to the Font Squirrel [@font-face Generator](http://www.fontsquirrel.com/fontface/generator). It utilizes Fontspring's [bulletproof @font-face structure](http://www.fontspring.com/blog/further-hardening-of-the-bulletproof-syntax) and automatically generates all the required file formats.

### Degradation

Thankfully browser handling of unsupported HTML5 and CSS3 is already that of a graceful nature. New additions to the list of `<input />` types such as "email", "search" etc. will generally degrade to normal `<input type="text" />` when not natively supported. Similarly, CSS3 properties that aren't supported will simply not appear. Responsive layouts controlled by height and width media queries are simply not applied.

**Subtle CSS3 effects should be employed as a reward for users who run a modern browser.**

The resources section below includes a few libraries to help normalize HTML5 and CSS3 functionality across a range of older browsers.

## Sass/SCSS (...and Less)
### Comments

Sass supports two comment styles: multiline (`/* */`) and single-line (`//`). With the exception of any licensing statements that need to be applied, the single-line style syntax should be used, as it will be removed when the CSS is compiled, making for smaller file sizes. More can be [read about this in the documentation](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#comments).

#### Debugging comments

While it's normal to leave debugging code enabled during development, it is crucial that this be removed before anything goes to production. There is no need for production code to have these artifacts left in them. Either re-compile using a setting that does not emit the debug code in to the output file, or run the finalized CSS through a minifier that will remove all comments.

### Camel Case Variables

Variable names should be camelCased (much like JavaScript variables), as this helps visually differentiate them from class names (which should be dash separated), and more closely aligns them with HTML IDs (which, like the variables, should reference a single object).

    // Bad
    $some-variable: 0px;
    $some_variable: 0px;
    
    // Good
    $someVariable: 0px;

### Define a Central Variables File

To make maintenance as easy as possible, the variables used in a project should be defined in a single location. This will help prevent inadvertent overwriting, and will make the codebase easier to maintain in the long run.

### All Colors Are Variables

When applying a color to an element (be it text, border, background, or otherwise), store that color in a variable. If it's used once, it's likely that it will be used again.

## Resources
### Useful Resources

The following resources are vital for the standardisation of code and interaction in a modern web page. They ensure that CSS3 and HTML5 features are made accessible across a range of browsers that previously lacked support.

* [jQuery](http://www.jquery.com/) JavaScript helper library
* [jQuery UI](http://www.jqueryui.com/) does for UX/UI what jQuery does for JavaScript
* [Modernizr](http://www.modernizr.com/) feature sniff, don't browser sniff!
* [RespondJS](https://github.com/scottjehl/Respond) brings responsive layouts to older browsers
* [@font-face Generator](http://www.fontsquirrel.com/fontface/generator) font embedding for all!
* [RaphaelJS](http://www.raphaeljs.com/) easy cross browser vector drawing
* [HTML5 Boilerplate](http://html5boilerplate.com/) a good starting point for any project. Even the "stripped" version is still a bit bloated.
* [Twitter Bootstrap](http://twitter.github.com/bootstrap/) allows you to rapidly prototype and style simple web apps.
