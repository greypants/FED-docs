
These are the recommended best practices for Viget front-end development.

- [General](#general)
- [HTML](#html)
- [CSS](#css)
- [JavaScript](#javascript)

<hr>

# General

### Readability vs Compression
Prefer readability over file-size concerns when it comes to writing and maintaining code. Compression can be automated, clear code can’t.

<hr>

### Modularity
When possible, write features or content blocks in a modular way. Features should be portable (possible to move between pages or containers) and repeatable (you can run more than one on a page, or stack several without causing layout problems).

This means:
- Create repeatable, configurable JS features using jQuery extensions or OO patterns
- Using classes over IDs wherever possible
- Clearly define modules and their components
- Modify module behavior and style with classes, not cascades (example: “fullsize-gallery” vs “body.homepage .gallery”, “.hero-hd” vs “.aboutpage h1”)
- When necessary, use “js-[featurename]” classes to separate behavior from presentation

<hr>

### Parts Kit
When possible, reproduce your modular styles and behaviors in a Parts Kit, to document them for future use, test them easily, and maintain a working copy. The Parts Kit also serves as a test of code modularity.

<hr>

### Organization
Prefer folder structures over filenames. Example: images/desktop/icons/arrow.png is preferable to images/icn_arrow.png

<hr>

### Source Files
Maintain your source files (PSDs, EPSes) outside of the repo to prevent filesize bloat, and link to them in your README.

<hr>

### Automation
On non-Rails projects, use Bundler with a Gemfile to manage automation requirements. Use Guard to automate minification, sprite generation, file compression, etc. Document its installation in the README.

<hr>

# HTML

### Doctype
The HTML5 Doctype is used for all pages:

```
<!doctype html>
```

While HTML5 offers the flexibility of more free-form style coding (akin to HTML 4.01), we will be adhering to XHTML 1.0’s sense of strictness. This includes closing open tags and quoting attributes.

<hr>

### Tags
Use parts of HTML5 when appropriate, while ensuring older browsers are handled fairly. When using new structural tags (like header or article), either provide a JavaScript shim or avoid styling the tags. For other new tags, always provide a fallback for older browsers. The simple HTML5 input types can be used without concern.

### Accepted HTML5 input types:
- email
- tel (telephone number)
- number (as in security code or credit card number)
- url
- datetime
- date
- month
- week
- time

<hr>

### Character Encoding
Use the utf-8 charset unless otherwise dictated by the project.

```
<meta charset="utf-8">
```

<hr>

### Template

While there is no official template, we mostly use variations of the HTML5 Boilerplate. See also: Jason's fork of the HTML5 Boilerplate and Blake's Front End Formulation project.

<hr>

### Semantics

Almost all HTML elements carry an inherent meaning and should be used appropriately based on context. This includes:
- Making use of DL (definition lists) and BLOCKQUOTE, when appropriate.
- Items in list form should always be housed in a UL, OL, or DL, never a set of DIVs or Ps.
- Use label fields to label each form field, the for attribute should associate itself with the input field, so users can click the labels. cursor:pointer; on the label is wise, as well.
- Do not use the size attribute on your inputs The size attribute is relative to the font-size of the text inside the input. Instead use css width.
- Don’t use tables for layout.
- Use microdata where appropriate, specifically hCard and adr.
- Make use of THEAD, TBODY, and TH tags (and Scope attribute) when appropriate.
- Be sparing when using extra tags for visual effects (icons, wrappers, etc). Ideally, every tag on your page should have a specific, non-visual meaning.
- Prefer to use JS to add any non-semantic tags needed for a JS feature.

<hr>

### SEO
Avoid making the site’s logo an H1 element, particularly on every page of a given project. The repetition of H1 content across all pages is seen as duplicative by some search engines.

<hr>

### Accessibility
Include skipnav links at the top of your page. When using these, you might also want to include the JS fix for Webkit browsers.

<ul class="screen-reader">
  <li><a href="#nav">Skip to Navigation</a></li>
  <li><a href="#content">Skip to Content</a></li>
</ul>

Use ARIA landmark roles on your document structure. Roles:
- application
- banner
- complementary
- contentinfo
- form
- navigation
- search
- main

### Validation
We will test our markup against the W3C validator, to ensure that the markup is well formed. 100% valid code is not a goal, but validation certainly helps to write more maintainable sites as well as debugging code. It’s suggested that FEDs auto-validate their code with a browser or text editor plugin.

We do not guarantee code is 100% valid, but instead assures the cross-browser experience is fairly consistent.


### IDs & Classes
IDs and Classes should be specific and meaningful. IDs should be avoided where possible, due to their difficulty to override or reuse.

Use terse CSS statements (‘.header-btn’, ‘.product-list li’) when possible to make code more reusable and easier to override. When you use the cascade, avoid long-distance cascading  (example: using classes on body to set font-sizes on a paragraph).

When creating IDs and Classes, try to avoid visual identifiers such as directions and colors like top, left, red and blue unless absolutely necessary on a system of large scale and complexity. This keeps visual meaning out of markup and leaves it up to CSS to define layout. When naming multiple identical regions differentiate by nothing other than position or color, use a numbering sequence such as “section-1” and “group-2”.

<hr>

# CSS

### Inline Styles
We strive to maintain proper separation of content and design, and therefore highly discourage the use of inline style=”…” attributes. CMS-controlled styles (background images, user-specified colors) are a common and valid exception.

<hr>
### Property order
CSS properties should, generally, by alphabetically ordered, after any @extends. You can place @includes where they work best.

```
p {
  @extend .text;
  @include border-radius(5px);
  @include rotate(5);
  border: 1px solid red;
  -moz-box-shadow: 0 0 1px #000;
  -webkit-box-shadow: 0 0 1px #000;
  box-shadow: 0 0 1px #000;
  display: block;
  width: 200px;
}
```
<hr>


### Validation
Don’t validate your CSS.

<hr>

### Font-Size Measurements
Use px to define font size, because it offers absolute control over text. Unit-less line-height is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the font-size.

<hr>

### Hacks
Hacks are not used. When you need to target a specific browser, use a class on the HTML element.

```
RIGHT
.ie6 .box {
  margin-right: 10px;
}

WRONG
.box {
  *margin-right: 10px;
}
```

### Browser Specific Styles
We encourage troubleshooting and building code that will work in all browsers without special modifications, but sometimes it is necessary to use conditional IE comments for CSS hooks we can use in our stylesheets.

Class the html tag with the appropriate version of IE to be used directly in the master stylesheet that contains the selector being re-written. There are several versions of this idea; here is a common one from the HMTL5 boilerplate.

```
<!--[if lt IE 7]> <html class="no-js lt-ie9 lt-ie8 lt-ie7" lang="en"> <![endif]-->
<!--[if IE 7]>    <html class="no-js lt-ie9 lt-ie8" lang="en"> <![endif]-->
<!--[if IE 8]>    <html class="no-js lt-ie9" lang="en"> <![endif]-->
<!--[if gt IE 8]><!--> <html class="no-js" lang="en"> <!--<![endif]-->

   .box { ... }
   .lt-ie7 .box { ... }

```

<hr>

### Images

Use CSS sprites generously. They make hover states easy, improve page load time, and reduce carbon dioxide emissions. If you can’t automatically generate your sprite, for the love of god save the original PSD somewhere.
If you can’t automatically compress your images, manually compress them with ImageAlpha and ImageOptim

<hr>

### General Principles
- Add CSS through external files, minimizing the number of files, if possible. It should always be in the HEAD of the document.
- Use the LINK tag to include, never the @import.
- Don’t include styles inline in the document, either in a style tag or on the elements. It’s harder to track down style rules.
- Use a reset CSS file (like Eric Meyer's reset) to zero our cross-browser weirdness.
- Understand cascading and selector specificity so you can write very terse and effective code.
- Write selectors that are optimized for speed. Where possible, avoid expensive CSS selectors. Avoid the * wildcard selector. Don’t qualify ID selectors (e.g. #header div#myid) or class selectors (e.g. table.results)

<hr>

### SCSS
Define all your mixins/variables/defaults in a single file when possible.
Sample structure for a screen.scss file:

```
    @import ‘mixins’
    @import ‘reset’
    @import ‘base’
    @import ‘structure’
    @import ‘modules’
    @import ‘pages/*’
    @import ‘utility’
```

Use @extend, not mixins, to repeat large blocks of style in a number of rules.

<hr>

# JavaScript

### Framework
We primarily develop new applications in jQuery; however, the JavaScript framework used is chosen on a per-project basis and is up to the discretion of the front-end development team for that project.

The preferred loading method for jQuery is to load from the google CDN, then fallback with a local copy. Example:

```
<script src=’//ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js’></script>
<script>window.jQuery || document.write(‘<script src=\’js/vendor/jquery-1.7.2.min.js\’><\/script>’)</script>
```

<hr>

### Functions
Avoid anonymous functions when named ones will better describe behavior. For example,

```
$(‘.row’).each(function(){
  var $cols = $(this).find(‘.col’);
var maxHeight = 0;
  $cols.each(function(){
    maxHeight = Math.max($(this).height(), maxHeight);
  });
  $cols.height(maxHeight);
});
```

Is less clear than this:

```
var equalizeHeight = function(){
  var $cols = $(this).find(‘.col’),
  var maxHeight = 0;
  var setMaxHeight = function(){
maxHeight = Math.max($(this).height(), max_height);
  };
  $cols.each(setMaxHeight).height(maxHeight);
};

$(‘.row’).each(equalizeHeight);
```

<hr>

### Validation
All JavaScript written should be run through a simple JSLint or JSHint validation before being committed. There is a TextMate "Javascript Tools" bundle that will automatically run JS through JSLint when saving a file.

Additionally, “use strict”; should be prepended inside a closed scope around your code.

<hr>

### Minimizing Globar Vars
Namespace all code to a clientname, and wrap your code in a self-executing function to prevent scope leak. Example:

```
window.CLIENTNAME = window.CLIENTNAME || {};

(function(C, $){

  ‘use strict’;

  ...code...

})(window.CLIENTNAME, jQuery);
```

<hr>

### Coding Practices

- 99% of code should be housed in external Javascript files. They should be included at the END of the BODY tag for maximum page performance.
- Nesting code in document.ready and window.load are not required, but encouraged when working with image sizes or writing to the body.
- Don’t rely on the user-agent string if you don’t have to. Do proper feature detection. (More at jQuery.support)
- Don’t use document.write().
- Strive to create functions which can be generalized, take parameters, and return values. This allows for substantial code reuse and, when combined with includes or external scripts, can reduce the overhead when scripts need to change.
- Comment your code, especially non-human-readable code like RegExp and external interfaces. It helps reduce time spent troubleshooting JavaScript functions.

<hr>

### .prototype
Don’t alter the prototype of basic JS objects (Object, String, Array, Function). If you feel like it’s the best solution for a large project document the changes extremely well (example: Sugar.js’s documentation)

<hr>

### Documentation
Don’t litter your code with JSdoc or NaturalDoc comments. If you’re not building an API with many potential users, you don’t need to generate docs.

<hr>

### Classes
When you define a class (or singleton), use “init” for the initialization method. Set any static vars first, set init next, then define the remaining methods.

<hr>

### Booleans
Boolean variable names should try be worded as a present-tense statement. This typically means phrasing them with “to be” verbs such as “is” (e.g. isVisible, isActive), “does” (e.g. doesMove, doesFillView), “has” (e.g. hasAttributes), etc.

<hr>

### Third Party Code
All third-party code should be clearly segregated from code written in-house. For projects where many JS assets are being used, third-party code should be relegated to its own “vendor” directory within the main javascript folder.

Don’t modify third-party scripts. If you have to, fork it, and use your fork. If you can’t do that, move it out of /vendor/ and very clearly comment to describe what you changed. Don’t remove attribution from third-party scripts.

<hr>

### Testing
Testing is difficult and unproductive on DOM-heavy JS. Reserve unit/int testing for JS modules with a lot of logic.

<hr>

### Chained Methods
Long chains of methods should appear on separate lines after the selector, indented with one tab.

```
$(‘.something’)
  .show()
  .css(‘height’, ‘100px’)
  .attr(‘title’, ‘Some Title’)
  .data(‘parent’, $myObject)
  .css({‘height’ : ‘100px’})
  .fadeOut();
```

<hr>

### Conditionals
Use variables to simplify conditionals. This improves readability and cuts down on the need to write multiline conditional statements.

```
RIGHT
var isNearOffset = pos >= windowPos && pos <= windowPos + this.windowOffset;
var isInView = pos >= windowPos && this.positions[i+1] <= windowThreshold;
var doesFillView = pos <= windowPos && this.positions[i+1] >= windowThreshold;
var isAtBottom = windowThreshold === this.documentHeight && i === length - 1;

if(isNearOffset || isInView || doesFillView || isAtBottom) {
indices.push(i);
 }

WRONG
if(
(pos >= windowPos && pos <= windowPos + this.windowOffset) ||
(pos >= windowPos && this.positions[i+1] <= windowThreshold) ||
(pos <= windowPos && this.positions[i+1] >= windowThreshold)
(windowThreshold === this.documentHeight && i === length - 1)
) {
indices.push(i);
}
```
