# HTML

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

**Which Do You Think Looks Cleaner, This?**

    <span class="sectionHeading">A Heading</span>
    <br /> <br />
    Lorem ipsum dolor sit amet. ...
    <br /> <br />

**Or This?**

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

## Use jQuery & jQuery UI Widgets

jQuery and jQuery UI are constructed to look and behave as close to identical as possible on different browsers. jQuery UI is designed to be WAI WCAG 2.0 and WAI ARIA compliant, so using the framework removes any uncertainty about plugins or scripts running on your site.

_**Note**: See the [jQuery Documentation](./libraries_and_frameworks/jquery.md) about when (and when not) to use jQuery._