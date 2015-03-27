WWF-UK CSS style guide
======================

This style guide sets out the internal practices for CSS. The objective for this is to make our stylesheets look as if they were written by one person, with consistent naming, formatting, commenting etc.

Credit where credit is due; this guide draws heavily on and mashes up:

- [Nicolas Hallagher’s Idiomatic CSS Style guide](https://github.com/necolas/idiomatic-css)
- [WordPress’s coding handbook](http://make.wordpress.org/core/handbook/coding-standards/css/)
- [Github’s coding style](https://github.com/styleguide/css)
- [Harry Robert’s coding style](http://csswizardry.com/2012/04/my-html-css-coding-style/)
- [Google’s coding style guide](http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml#CSS_Style_Rules)
- [SMACSS guide](http://smacss.com)


General principles
------------------

*"Part of being a good steward to a successful project is realizing that writing code for yourself is a Bad Idea™. If thousands of people are using your code, then write your code for maximum clarity, not your personal preference of how to get clever within the spec." - Idan Gazit*

- Don't try to prematurely optimize your code - keep it readable and understandable.
- Make sure it’s explained, commented and well named.
- All code in any code-base should look like a single person typed it, even when many people are contributing to it.
- Strictly adhere to the style syntax.
- If in doubt when deciding upon a style - take a look at existing code and use common patterns.

- clarity over compactness
- names-use-hyphens
- no more than 3 multiple selectors
- never use #id
- each selector and property on its own line
- tabs
- ems and %


Structure
---------

This may seem picky, but it really does help make sure that the code remains legible - and that people in the far distant future can tell what we were doing.

- Tabs, not spaces, are used to indent
- One blank line between blocks of code within a section
- Two blank lines between different sections
- One selector per line, followed by a comma or an opening curly brace
- The opening curly brace should have a space before it
- Properties and their values should be
- indented with one tab
- on the same line as each other
- always closed with a semicolon - including the last one
- The closing curly brace is indented at the same level as the opening selector

For example:

    .selector-one,
    .selector-two,
    .selector-three {
        background: lawngreen;
        color: lightyellow;
    }

But not:

    .selector-one, .selector-two, .selector-three{
        background: lawngreen;
        color: lightyellow
    }

or
    
    .selector-one,
    .selector-two,
    .selector-three{background: lawngreen;
        color: lightyellow
        }


Modular coding
--------------
Reusing code is great, since it minimises file size - but further down the road can end up with a spaghetti. So, even if it means repeating things, keep styles specific to a module or a shared style. We can run the larger code through a minifier that combines selectors which have shared properties at a later date for production.

**Correct:**

    .image {
        border: 1px solid grey;
        padding: 1px;
    }
    
    .full-width-image {
        /* use with .image */
        width: 100%;
    }
    
    .half-width-image {
        /* use with .image */
        width: 50%;
    }

**or**

    .full-width-image {
        border: 1px solid grey;
        padding: 1px;
        width: 100%;
    }
    
    .half-width-image {
        border: 1px solid grey;
        padding: 1px;
        width: 50%;
    }


**Incorrect:** too broad

    img {
            border: 1px solid grey;
            padding: 1px;
        }

**Incorrect:** this path leads to convoluted code

    .full-width-image,
    .half-width-image {
        border: 1px solid grey;
        padding: 1px;
    }
    
    .full-width-image {
        width: 100%;
    }
    
    .half-width-image {
        width: 50%;
    }


Selectors
---------

*‘With great power there must also come great responsibility’ - Uncle Ben*

Broad selectors give efficient and easily understandable code, but can lead to unseen consequences. Very specific selectors can target accurately, but tend to lead to a cluttered and large stylesheet.

**Don't use `#id` for styling.** They're too specific and give long term pain. They lack reusability, tend to lean towards use-once code, and the only way to override them is with the dreaded `!important`. So we don't use `#id` for styling.

**Broad selectors - such as `figure`, `header`, `footer` - are broadly fine**. They should be used as foundation styles, but be careful when using broader selectors such as section and article.

`p`, `a`, `form`, `small` etc should also be thought of as foundation styles to be built upon. For example:

    /* foundation paragraph style */
    p {
        font-size: 1em;
        padding: 0 0 1em 0;
    }
    
    /* style specific to paragraphs within a figure */
    figure p {
        padding: 0.25em;
    }

`div` and `span` are used in too many different circumstances and shouldn't be used as selectors.

**Classes are used for specific styling.** They’re flexible enough to be broad and specific, allow reusing without creating a spaghetti of CSS code.

- When naming classes, try and make them human readable that describes the element(s) that they style or the function they carry out.
- Always use lowercase and separate words with hyphens - no `.CamelCase` and `.under_scores`
- Attribute selectors should use single quotes - increases legibility for humans.
- Target only the class, not the element and class - for example: `.comment-form`, not `div.comment-form`
- Be careful of selector supporter in older browsers - for example, `input[type='password']` or `input:not([type='submit'])not:([type='password'])`. Use them, but use with caution.

**Correct:**

    header,
    footer {
    }
    
    .visually-hidden {
    }
    
    input[type='submit'] {
    }
    
    input[type='number'] {
    }


**Not:**

    div#logo {
    }
    
    #logo {
    }
    
    h1.logo {
    }


Properties
----------

- 0.5em, not .5em - as makes it easier to distinguish between 0.5em and 5em
- Single quotation marks for attribute selectors, property values, and URI values.

    header {
        font-family: 'Open Sans', 'Helvetica Neue', Helvetica, Arial, sans-serif;
        background-image: url('dancing-cat.gif');
    }


Property ordering
-----------------

Alphabetic - nice and simple.

    p {
        background: #fff url('background.jpg') no-repeat 50%;
        line-height: 2;
        margin: 0;
        padding: 0 1em 1em 0;
        position: relative;
        width: 50%;
        z-index: 1;
    }

A special mention for vendor prefixes; they're alphabetacised as if they don't have the vendor prefix. The W3C property comes last so that it isn't don’t overriden. They're indented with an extra tab and then alphabetised based on the vendor prefix.
    
        -moz-transition: all 300ms linear;
        -ms-transition: all 300ms linear;
        -o-transition: all 300ms linear;
        -webkit-transition: all linear;
    transition: all 300ms linear;

If you’re using Sass or Less, then use a variables and mixins file as a repository for vendor prefixed styles. You can then just use `.transition(all 300ms linear);` or `@include transition(all 300ms linear);` to keep the code clean.


Property values - Pixels vs. Ems
--------------------------------

The property preference hiererchy goes:

1. `em` or `%`
2. Nothing else

Whether `em` or `%` is used is dependant on the situation, but the important thing about these is that they're flexible and don't block the stylesheet's cascade.

Pixels aren't needed - preprocessors can do the division for you to convert `px` into the `em` equivalent.

For example, in Less or Sass:

    /* method: */
    width: ( [pixel number] / [parent element em size]em );
    
    /* for example: */
    width: ( 20 / 16em );
    
    /* would output */
    width: 1.25em;

If you don't use preprocessors, then still use `em` - the numbers will just neeed to be calculated by hand.

*The reasoning:* using either pixels, points, or root ems block the cascade of inheritance. Using `em` or `%` we allow elements to inherit characteristics - which makes designing responsively much lighter, as we're not rewriting the CSS for every viewport size. 

This can seem frustrating if you're unfamiliar with the C of CSS, but the killer advantage is that you can just change the root font size and everything else will change in proportion.


How to comment
--------------

Comments within the code should be easily visible, but not create muddle. With this in mind, these are the following rules for commenting within CSS.

For consistancy, the CSS style comments - `/* */` - are always used, even when using preprocessors.

**To do lists and noticed bugs that aren't yet fixed are incredible useful comments** - especially when using a repository such as Git or SVN. Please make notes within the code to help the rest of the organisation keep up with you. As with JavaScript, use the prefixes `/* FIXME */` and `/* TODO */` where appropriate.

At the top of the style.css (see *File structure*), this description should go as a minimum. Note the exclamation mark, which (should) preserve the comment after minification.

    /*! ============================================================================
    
    Project:        Name
    Version:        [major].[minor].[bugfixes] 
    Last change:    Date (quick comment)
    Assigned to:    Person, person, person
    
    ============================================================================= */
    
The versioning follows semantic versioning[^!semanticversioning]

Any other descriptors are welcome - contact details, to do list etc.

**Headings** for sections of code should be highlighted with single lines:

    /* -----------------------------------------------------------------------------
        Heading
        - small description of what's going on here
        - another point, maybe a bugfix with a FIXME
        - TODO if needed
    ----------------------------------------------------------------------------- */
    
**Individual comments in the code** don't have the single or double lines.

If referring to the whole selector, then the comment should sit inside it.
    
    .standfirst p {
        /* this is to make the first paragraph into a standfirst */
        font-weight: bold;
    }
    
If referring to an individual property, it should sit on the same line after the semicolon:
    
    font-size: 1.25em; /* equivalent to 20px */
    margin-top: 1.48em; /* FIXME magic number */


File structure
--------------

The SMACSS file structure[^!smacssguide], with some tweaks of our own. We use a concatenator - which can be included with either SASS or LESS - to produce a single, minifed stylesheet from several different files.

- style, which imports:
     - variables-and-mixins (only if Sass or Less are used)
     - normalize.css[^!normalise]
     - base
     - layout
     - modules
     - theme

Output is a minified, single file:

- style.css


Media Queries
-------------

If Internet Explorer 8 (or older) support is needed, then go desktop first. Any desktop styles then won't have be duplicated in an Internet Explorer only stylesheet.

Otherwise, if Internet Explorer 8 (or older) support isn't required, then go mobile first.

The breakpoints should be in `em`, as this provides the widest level of consistent, bug-free support. Using `ems` in media queries doesn't use the root setting - it uses the browsers default font size, usually 16px.

Any styles that are triggered by media queries should be placed within the stylesheets - not in their own stylesheet.

For example, within the <i>modules</i> stylesheet:

    [ Small screen rules for specific module here ]
    
    @media (min-width: 37.5em) {
        /* break point is equivalent to 600px (37.5 x 16) */
        [ Medium screen rules for specific module here ]
    }
    
    @media (min-width: 48em) {
        /* Break point equivalent to 768px (48 x 16) */
        [ Medium screen rules for specific module here ]
    }
    
    @media (min-width: 80em) {
        /* Break point equivalent to 1280px (80 x 16) */
        [ Large screen rules for specific module here ]
    }

If edits are required to a specific module, then everything for that module is in one place - rather than spread over several different stylesheets.

Best Practices
--------------

Stylesheets tend to get long in length. Focus slowly gets lost whilst intended goals start repeating and overlapping. Writing smart code from the outset helps us retain the overview whilst remaining flexible throughout change.

- If you are attempting to fix an issue, try to remove code before adding more.
- Magic Numbers are unlucky. These are numbers that are used as quick fixes on a one-off basis. Example:

```
.box {
margin-top: 37px;
}
```
- Know when to use the height property. It should be used when you are including outside elements (such as images). Otherwise use line-height for more flexibility.
- Do not restate default property and value combinations (for instance `display: block;` on block-level elements [^!HTML5block].

---------------------------------------

Footnotes
---------

[^!semanticversioning]: [Semantic versioning](http://semver.org/)

[^!smacssguide]: [SMACSS module guide](http://smacss.com/book/categorizing)

[^!normalise]: [normalize.css](https://github.com/necolas/normalize.css) is kept separate to allow normalize.css to be updated easily.

[^!HTML5block]: An exception is for HTML 5 elements, such as `section`, `header` etc. These should be display: block at the start of the stylesheet for older versions of Firefox. But then, you probably knew that - or are using something nifty like *normalize.css* which does that anyway.
