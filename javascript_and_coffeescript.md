# JavaScript and CoffeeScript

It should be noted that CoffeeScript is the preferred tool for writing out JavaScript. These guidelines are listed first for a few reasons: first, CoffeeScript is eventually output as JavaScript, so many of the rules still apply, and second, there are often times where JavaScript needs to be written rather than CoffeeScript.

## Documentation

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

## Whitespacing & Formatting

Any discussion about formatting, whitespacing and the placement of braces is going to be hotly debated. I guess the simplest rule is that, unless you're willing to completely format a whole document, **respect and maintain the formatting of an existing document**. That means: see same-line braces throughout a JS file, continue to write code with same-line braces. Your code should fail the code review process if it doesn't maintain consistency with the rest of the document.

Consistent formatting makes code more readable, and also means the code can be easily modified with find and replace commands. The coding habits we have picked up are thankfully very similar to what jQuery officially encourages. There are a few minor discrepencies, but again, these are personal issues or things that we think cannot be maintained. [Further Reading](http://docs.jquery.com/JQuery_Core_Style_Guidelines).

### Character Spacing

    // Bad
    if(blah=="foo"){
      foo("bar");
    }
    
    // Good
    if (blah == "foo") {
      foo("bar");
    }

### Same Line Braces

    // Bad
    if (foo)
    {
      bar();
    }
    
    // Good
    if (foo) {
      bar();
    }

### Always Using Braces

    // Bad
    if (foo)
      bar();

    // Good
    if (foo) {
      bar();
    }

### String Handling

**Strings should always use double quotes**. Some people are very fond of their C style strings (single quotes), but this leads to conflicting styles within a script. C style string handling dictates that empty and single character strings should be wrapped in single quotations, while phrases and words should be wrapped in double quotations.

### Variable Declarations

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

## Always Specify the `radix` Parameter When Using `.parseInt()`

When parsing a string to an integer, it is considered good practice to specify the `radix` parameter - which determines to what base the string should be converted to. The default setting will trigger a radix of 16 whenever the string is lead by a 0. Most beginner and intermediate users are only ever going to be using a radix of `10`.

    alert( parseInt("08") ); // alerts: 2
    alert( parseInt("08", 10) ); // alerts: 8

## Avoid Comparing to true and false

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

## Avoid Polluting the Global Namespace

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

## Camel Case Variables

The camel casing (or *camelCasing*) of JavaScript variables is accepted as the standard in most coding environments. The only exception is the use of uppercase and underscores to denote constants.

    var X_Position = obj.scrollLeft;
    var xPosition = obj.scrollLeft; // tidier
    SCENE_GRAVITY = 1; // constant

## Loop Performance - Cache Array Length

Looping is arguably the most important part of JavaScript performance to get right. Shave a millisecond or two off inside of a loop, potentially gain seconds overall. One such way is to cache the length of an array so it doesnt have to be calculated every time the loop is iterated through.

    var toLoop = new Array(1000);
    
    for (var i = 0; i < toLoop.length; i++) {
      // BAD - the length has to be evaluated 1000 times
    }
    
    for (var i = 0, len = toLoop.length; i < len; i++) {
      // GOOD - the length is only looked up once and then cached
    }

### The Exception

If you're looping through an array to find and remove a particular item, this will alter the array length. Any time you change the array length by either adding or removing items from inside the loop, you will get yourself into trouble. Consider either re-setting the length or avoid caching it for this particular situation.

## Loop Performance - Use 'break;' & 'continue;'

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

## Don't Send Too Many Function Parameters

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

## Remap 'this' to 'self'

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

## All Bound Up

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

## Storing Booleans

Booleans should be easily identifiable by the way they are named. Use prefixes like `is`, `can` or `has` to propose a question.

    isEditing = true;
    obj.canEdit = true;
    user.hasPermission = true;

## Minimizing Repaints & Reflows

Repaints and reflows relate to the process of re-rendering the DOM when particular properties or elements are altered. Repaints are triggered when an element's look is changed without altering its layout. Nicole Sullivan describes these changes in a thorough [blog post](http://www.stubbornella.org/content/2009/03/27/reflows-repaints-css-performance-making-your-javascript-slow/) as style changes such as visibility or background-color. Reflows are the more costly alternative, caused by changes that alter the layout of the page. Examples include the addition or removal of elements, changes to an element's width or height, and even resizing the browser window. Worst yet is the domino effect of reflows that cause ancestor, sibling and child elements to reflow.

There is no doubt that both reflows and repaints should be avoided if possible, but how?

### A Reflow Example

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

## Don't Use Milliseconds to Generate Unique IDs

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

## Feature Sniff, Don't Browser Sniff

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

## Readable Milliseconds

A handy way of writing milliseconds in a readable format. Great for beginners, but mostly a gimmick.

    // is this 3, 30 or 300 seconds?
    var timeout = 30000;
    // an extra calculation, but easier to read and modify
    var timeout = 30 * 1000;
