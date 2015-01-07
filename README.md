# The Front-End Style Guide

This is a collection of documents describing how to write front-end code (HTML, JS, CSS, et al) the Intridea way.

This document contains universal rules like editor configuration and documentation styles. Language-specific documentation can be found in their respective documents:

## The Guides

* [HTML](./html.md)
* [CSS and Sass](./css_and_sass.md)
* [JavaScript and CoffeeScript](./javascript_and_coffeescript.md)
* Some Specific Libraries:
  * [jQuery](./libraries_and_frameworks/jquery.md)
  * [AngularJS](./libraries_and_frameworks/angularjs.md)
  * [Pre-Fab UI Libraries](./libraries_and_frameworks/prefab_ui_libraries.md)

## Precompilation

### Standard Libraries

These are the Intridea standards, and any deviation from that should be seen as the exception and not the rule.

1. When producing CSS, that code should be created with the use of [Sass](http://sass-lang.com/) (using the SCSS syntax, which is the library's default).
2. When producing JavaScript, that code should be created with the use of CoffeeScript.

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

And for JavaScript, it would look like this:

    /**
     * Module Name
     * A brief description about what the module does.
     * --------------------------------------------------
     **/
    
    var moduleName = function () {
      // ...
    );
    module.exports = moduleName;

Please document as you work through the files. For SCSS, use [KSS](https://github.com/kneath/kss/blob/master/SPEC.md), and for JavaScript follow the [JSDoc](http://usejsdoc.org/about-getting-started.html)/TomDoc pattern.

## CSS3 & HTML5

### Feature Sniff with Modernizr

Use [Modernizr](http://www.modernizr.com/) to detect the availability of specific HTML5 and CSS3 features.

### `@font-face` Use and Abuse

Before you consider embedding a custom font, is important that you inspect the EULA and check if web embedding is allowed. Foundries are understandably reluctant to allow designers and developers the ability to place font files directly on a server which can then be copied by a savvy end user. Particular foundries also prohibit the embedding of particular file types, such as `.TTF` and `.OTF`.

If, after careful consideration, you believe the desired font is web embeddable: head on over to the Font Squirrel [@font-face Generator](http://www.fontsquirrel.com/fontface/generator). It utilizes Fontspring's [bulletproof @font-face structure](http://www.fontspring.com/blog/further-hardening-of-the-bulletproof-syntax) and automatically generates all the required file formats.

### Degradation

Thankfully browser handling of unsupported HTML5 and CSS3 is already that of a graceful nature. New additions to the list of `<input />` types such as "email", "search" etc. will generally degrade to normal `<input type="text" />` when not natively supported. Similarly, CSS3 properties that aren't supported will simply not appear. Responsive layouts controlled by height and width media queries are simply not applied.

**Subtle CSS3 effects should be employed as a reward for users who run a modern browser.**

The resources section below includes a few libraries to help normalize HTML5 and CSS3 functionality across a range of older browsers.

## Resources
### Useful Resources

The following resources are vital for the standardisation of code and interaction in a modern web page. They ensure that CSS3 and HTML5 features are made accessible across a range of browsers that previously lacked support.

* [Tait Brown's Front End Guidelines](http://taitems.github.io/Front-End-Development-Guidelines/) Good additional reading about crafting quality front-end code
* [jQuery](http://www.jquery.com/) JavaScript helper library
* [jQuery UI](http://www.jqueryui.com/) does for UX/UI what jQuery does for JavaScript
* [Modernizr](http://www.modernizr.com/) feature sniff, don't browser sniff!
* [RespondJS](https://github.com/scottjehl/Respond) brings responsive layouts to older browsers
* [@font-face Generator](http://www.fontsquirrel.com/fontface/generator) font embedding for all!
* [RaphaelJS](http://www.raphaeljs.com/) easy cross browser vector drawing
* [HTML5 Boilerplate](http://html5boilerplate.com/) a good starting point for any project. Even the "stripped" version is still a bit bloated.
* [Twitter Bootstrap](http://twitter.github.com/bootstrap/) allows you to rapidly prototype and style simple web apps.
