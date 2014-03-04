This is to help define the standards to be followed when developing the UI for
this project.

For guidelines that aren't covered here, refer to
http://taitems.github.io/Front-End-Development-Guidelines/

If this project uses
[Bootstrap](https://github.com/thomas-mcdonald/bootstrap-sass), then the
variables and mixins included are available for use in the SCSS files. Core
bootstrap functionality should be overridden, never modified. SCSS variables
should be kept in `sass/_variables.scss`.


### Spacing/Indentation

We follow the standard rails convention of using soft-tabs, and
2-spaces-per-tab for all JS/CS/SCSS files. No hard-tabs please.

Eliminate any and all trailing white-space. It's messy, and it causes Git to
complain. Most editors support doing this automatically.


### Documentation

When creating new files, please add a mast-head and brief description of the
module. It should look something like:

    // in '_module.scss'

    //
    // Module Name
    // A brief description about what the module does.
    // --------------------------------------------------


    // First module component
    .module {
      // ...
    }

Please document as you work through the files. For SCSS, use
[KSS](https://github.com/kneath/kss/blob/master/SPEC.md), and for JavaScript,
follow the [JSDoc](http://usejsdoc.org/about-getting-started.html)/TomDoc
pattern.


### Naming conventions and File Organization

#### Files

##### CSS / SCSS

SCSS files that are created for the project should live in
`sass`. Files that are provided by a vendor library (for
example, SymbolSet) should be in `sass/vendor`.

Files should be small and concise, focused on building modules. A good example
of what should be contained in a file would be adding all type rules in
`_type.scss`. Alternately, a file for forms is appropriate. Most files will
live under the `sass/modules` directory.

Partials should be named using the `_{{filename}}.scss` pattern. Top-level
files should be named `{{filename}}.css.scss`.

When writing SCSS, please use the following conventions:

* Use box-model impact as a sorting method for CSS properties. See [Andy Ford's](http://web.archive.org/web/20130227044124/http://fordinteractive.com/2009/02/order-of-the-day-css-properties) post on the reasons why.
  * An easy way to do this is to use the [CSSComb](http://csscomb.com) tool (there are plugins for most editors; just make sure it's not set to use 'alphabetical').
* Put `@extends` declarations at the top of the property list (followed by an empty space)
* Put `@include` declarations at the bottom of the property list (preceded - and followed - by an empty line).
* Destroy all trailing whitespace.

For more details, read [this article on CSS Tricks](http://css-tricks.com/sass-style-guide)

Finally, do not check in compiled CSS.


##### JavaScript / CoffeeScript

JavaScript/CoffeeScript files that are created for the project should live in
`scripts`. Files that are provided by a vendor library(for
example, SymbolSet) should be in `scripts/vendor`.

The directory should be kept as shallow as possible. Files should be modularly
built, each file containing one class at a time.

If [Backbone](http://backbonejs.org) is to be used, then
[Marionette](http://marionettejs.com) should be
used as well. As such, the module files should be kept in
`scripts/features`, with a directory for each module. The
structrue should look like this:

    scripts
    |-  app.js.coffee
    `-  features
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

#### Variables/Classes/IDs

IDs should be camelCase

    // Good
    #someElement
    <span id="someElement"></span>

    // Bad
    #some-element
    <span id="some-element"></span>

Additionally, IDs should not be used when styling elements with CSS; they
should primarily be used for targeting elements with JavaScript. Not to say
they should never be used, but given that most elements will likely be part of
a re-usable pattern, they should only be used as a last resort.

Classes should be dash-separated

    // Good
    .some-element
    <span class="some-element"></span>

    // Bad
    .someElement
    .someelement
    <span class="someElement"></span>

Additionally, naming should follow the [SMACSS](http://smacss.com)
methodologies / [OOCSS](https://github.com/stubbornella/oocss/wiki) principles. Focus on creating re-usable components,
and not crafting hyper-specific styles.

Both CoffeeScript/JavaScript and SCSS variables should be camelCase

    // Good
    $someVariable // SCSS
    someVariable  // JavaScript/CoffeeScript

    // Bad
    $some-variable // SCSS
    some-variable  // JavaScript/CoffeeScript
