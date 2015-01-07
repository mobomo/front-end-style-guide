# jQuery

First and formost it is absolutely important to note that unless necessary (it's a dependency, for example, of a core JS library) jQuery should *not* be used in our projects. This is particularly true when building applications with AngularJS, as Angular ships with a jQuery-style replacement library. That, paired with the fact that newer versions of native JS contain most of the utility methods that jQuery provided. If there's something missing (`select` or `findWhere` for example), use a library like [UnderscoreJS](http://underscorejs.org/) or [Lo-Dash](https://lodash.com/).

With that out of the way, if you're using jQuery, you should use it the best way possible. This document outlines our best practices.

## Chain, Chain, Chain

One of the best parts of jQuery is its function chaining. You've probably used it a bit, maybe a few simple calls one after another&hellip; but have you ever traversed the DOM like a mad dog? Take some time to familiarize yourself with the [`.end()`](http://api.jquery.com/end/) function. It is critical for when you begin stepping up and down the DOM tree from your original selector.

    $(".quote")
      .hide()
      .find("a").text("Click here").bind("click", doStuff).end()
      .parent().removeClass().addClass("testimonial").draggable().end()
      .fadeIn("slow");

In the example above, the [.end()](http://api.jquery.com/end/) function is used once we have finished doing things with a particular DOM object and want to traverse back up the DOM to the original object we called. We then load back up and dive back into the DOM.

## Using `data-*` Attributes

Those of you who have been writing JavaScript (and not jQuery) for a good length of time are most likely familiar with attributes. Setting them. Getting them. Abusing `rel` and `title` instead&hellip;

So when *isn't* HTML5 or jQuery coming the rescue? New specs allow the use of `data-` prefixes on HTML elements to indicate attributes which can hold data, and jQuery does an awesome job of converting the designated string into the correct JavaScript type. It's a beautiful partnership. Let's create a `DIV` with some data attributes.

    <div id="test" data-is-bool="true" data-some-number="123"></div>

Now even though our values are wrapped in quotation marks, they won't be handled as strings:

    typeof $("#test").data("isBool"); // boolean
    
    typeof $("#test").data("someNumber"); // number

### Special Casing

It's also important to notice the lower casing required to get these snippets to work. But if you're a great front end developer, you will still want to camel case your data variables. Like many places in JavaScript, a preceding hyphen signifies camel casing of the next letter. The following camel casing of the HTML attribute **does not work** and the same JavaScript used above will return `undefined`.

**Does not work**

    <div id="test" data-isBool="true" data-someNumber="123"></div>

**Does work**

    <div id="test" data-is-bool="true" data-some-number="123"></div>

### '`.stop()`' Collaborate & Listen

Binding jQuery animations to mouse events is a key part of modern web-based user interaction. It's also something that you see done poorly on even the most famous of web sites. [This article](http://www.learningjquery.com/2009/01/quick-tip-prevent-animation-queue-buildup) provides a straight forward example of built up animations and demonstrates how visually jarring they can be. Thankfully it's easily fixed with a single function prefix or a parameter added to `$.animate` calls.

When using `$.animate`, `queue: false` can be added to the parameters to prevent chaining. Animation shortcuts such as `$.fadeIn` or `$.slideDown` do not take `queue` settings. Instead you have to pre-empt these animations with the `$.stop` method of pausing currently executing animations. Certain scenarios require the animation to stop dead in its tracks, or to jump to the end of the transition. It is recommended you familiarize yourself with the [documentation](http://api.jquery.com/stop/) of the parameters `clearQueue` and `jumpToEnd`, because god knows I can't help you there.

    $("selector").stop(true, true).fadeOut();
    $("selector").animate({
      property: value
    }, {
      duration: 1000,
      queue: false
    }

## Selectors and You

### Optimize Your Selectors

jQuery is pretty laid back. It can do pretty much everything but make you coffee, and I hear that's in the roadmap for 2.0. One thing you have to be careful about is abusing the power that is the [sizzleJS](http://sizzlejs.com/) selector engine. There are two strategies to overcome this: *caching the selector results* and *using efficient selectors*.

#### Caching Selector Results

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

### Using Efficient Selectors

So jQuery/sizzleJS can use CSS3 selectors well, but what's the real cost? Behind the scenes the browser is hopefully using `document.querySelector()`, but there's also a fair chance it will be breaking down your selector string and querying the DOM manually.

    // an ID search is the quickest possible query, then it just takes a list of the childNodes and matches the class
    $("#quoteList").children(".quotes");
    // looks for the "foo" class only in the pre-defined bar element
    $(".foo", bar);

## A 'for' Loop is Always Quicker Than an 'each()' Loop

No matter what happens in the next few years of browser development, a native `for` loop will always be quicker than a jQuery `$.each()` loop. When you think of what jQuery really is (a library wrapped around native JS functions) you begin to realize that the native underlying JavaScript is always going to be quicker. It's a tradeoff of run speed versus authoring speed.

It is vital that a native `for` loop is always used for performance critical functions that could fire potentially hundreds of times per second. Examples include:

* Mouse movement
* Timer intervals
* Loops within loops