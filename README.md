# interactive-examples

[![Build Status](https://travis-ci.org/mdn/interactive-examples.svg?branch=master)](https://travis-ci.org/mdn/interactive-examples)

Home of the [MDN](https://developer.mozilla.org/) live interactive code examples.


## Folder Structure

* [css] - This contains the CSS used by the base templates.
* [js] - This contains the JS used by the base templates.
* [live-examples] - This contains the live example CSS and JS fragments.
* [media] - The contains images used by the live examples and templates.
* [tmpl] - The base templates.

The dynamically generated pages, their dependencies, and assets are generated to the `gh-pages` branch.

## site.json

This describes the pages and bundles that make up the live examples.

### Bundles

Bundles are mainly used by the base templates, and look like so:

```
"cssExamplesBase": {
    "javascript": ["js/libs/prism.js", "js/editable-css.js"],
    "css": ["css/editable-css.css", "css/libs/prism.css"],
    "destFileName": "css-examples-base"
},
```

* `javascript` - This is an array of JS files that will be concatenated and Uglified.
* `css` - This is an array of CSS files that will be concatenated.
* `destFileName` - The file name to use for the generated bundle; `.css` or `.js` will be appended as appropriate.

### Pages

This section is used to describe live example pages that will be generated. The schema is as follows:

```
"arrayFind": {
    "baseTmpl": "tmpl/live-js-tmpl.html",
    ["cssExampleSrc": "../../live-examples/css-examples/css/animation.css",]
    ["jsExampleSrc": "../../live-examples/js-examples/array-find.js",]
    ["exampleCode": "live-examples/css-examples/border-top-color.html",]
    "fileName": "array-find.html",
    "type": "js"
},
```

* `baseTmpl` - The base template to use for this example.
* `cssExampleSrc` - The file location that will be used as the value for the `href` attribute of a `link` tag.
* `jsExampleSrc` - The file location that will be used as the value for the `src` attribute of a `script` tag.
* `exampleCode` - This is currently only used by the CSS examples, and points to the location of the relevant live example HTML file.
* `fileName` - This is the file name that will be used for the generated live example page.
* `type` - The type of example; currently the only available types are js (JavaScript) or css.

## Browser Support Baseline

The following is a list of browser/version combinations that are supported by the interactive editor. In browsers that do not meet the criteria, the editor degrades gracefully to displaying static examples.

* Firefox - Latest three release versions.
* Chrome - Latest three release versions.
* Opera - Latest two release versions.
* Safari - Latest two release versions.
* Internet Explorer - version 11.
* Edge - Latest release version.


## Contributing Live Code Examples

### Get setup to contribute

To contribute live examples all you need is a text editor, git, a GitHub account, and Nodejs.

As far as text/code editors go, there are more editors than you can shake a stick at, so it's down to personal preference. [Atom](https://atom.io/) is a great, open source editor we can definitely recommend.

For more information on setting up Git on your machine, [read this article](https://help.github.com/articles/set-up-git/).

With the above dependencies satisfied, [create your new account on Github](https://github.com/join).

Lastly, [install Nodejs for your operating system](https://nodejs.org/).

### Fork and Clone

Next up, you need to fork and clone the repo to be able to contribute to it. You can [learn about forking on Github](https://help.github.com/articles/fork-a-repo). Once you have you own fork, [clone it to your local machine](https://help.github.com/articles/cloning-a-repository/).

Finally, change into the new directory created by the clone and run the following command:

```
npm install
```

This will ensure that you have all the required development modules installed to build and test your contributions. You are now set-up, and ready to contribute. Thank you o/\o

### Publishing to production (`interactive-examples.mdn.mozilla.net`)

Push to the `prod` branch on GitHub, e.g. `git push origin master:prod`. This will push your local `master` branch to the remote `prod` branch.

### General

There are two types of live examples we currently support: JavaScript and CSS. The process of contributing to either is essentially the same, but there are slight differences you need to be aware of.

### Contributing a CSS example

Here we walk you through adding a new CSS example

You start off by creating a new HTML fragment inside `live-examples\css-examples\`. The name of this file should clearly match the example you are adding. For example, if you are adding samples for `border-radius` you would call the file `border-radius.html`.

Inside this newly created file, copy and paste the following code.

```
<section id="example-choice-list" class="example-choice-list">
<div class="example-choice">
<pre><code class="language-css">/* optional comment */
your CSS goes here</code></pre>
</div>
</section>

<div id="output" class="output">
    <section id="default-example">
        <div id="example-element"></div>
    </section>
</div>
```

This is the base starting point for all CSS examples. Your next step is to fill in the example element. For `border-radius` it makes sense to have a simple `<div>` element with a solid background color. The already present `example-element` `<div>` will do, however we do need to give it some basic styling, and perhaps add the text "Style Me".

First then, inside the `example-element` `<div>` add the text "Style Me", like so:

```
<div id="output" class="output">
    <section id="default-example">
        <div id="example-element">Style Me!</div>
    </section>
</div>
```

Next, create a new CSS file inside `live-examples\css-examples\css\`. Call this CSS file the same as the HTML file i.e. `border-radius.css`. Add the following code to it:

```
#example-element {
    background-color: #74992E;
    width: 250px;
    height: 80px;
}
```

Next we need to add some different examples of using `border-radius`. In the `<pre>` element we first saw above, you will see there is a nested `<div>` with a class of `example-choice`. For each example you wish to add, you will add one of these with the CSS declaration to apply as its content, for example:

```
<section id="example-choice-list" class="example-choice-list">
<div class="example-choice">
<pre><code class="language-css">border-radius: 10px;</code></pre>
</div>

<div class="example-choice">
<pre><code class="language-css">border-radius: 10px 5%;</code></pre>
</div>

<div class="example-choice">
<pre><code class="language-css">border-radius: 10px 5px 2em / 20px 25px 30%;</code></pre>
</div>
</section>
```

#### Comments

You can add comments to the examples you add. However, when adding comments, please follow these guidelines:

* Add comments only when absolutely required.
* Wrap comments in the standard CSS comment delimiters, i.e. `/* */`.
* A comment should never span more than one line.
* Aim to limit the number of characters to around 80.

With this, the example work is complete, and all you need to do is tell the page generator about your new page and its dependencies. To do this, open up the `site.json` file in the root of the project folder. Under `pages`, find an existing entry with a `type` of `css`.

Copy and paste the example then update it to apply to your new example, noting that pages are grouped by `type`, and then alphabetically for each `type`. You entry will look something like this when edited:

```
"borderRadius": {
    "baseTmpl": "tmpl/live-css-tmpl.html",
    "cssExampleSrc": "../../live-examples/css-examples/css/border-radius.css",
    "exampleCode": "live-examples/css-examples/border-radius.html",
    "fileName": "border-radius.html",
    "type": "css"
},
```

You're done. All that remain is to test that your page generates and displays as intended, then open a pull request for review.

From your command line run:

```
npm run build
```

Once this completes run:

```
npm start
```

Now point your browser to [localhost:8080/pages/css/border-radius.html](http://localhost:8000/pages/css/border-radius.html).

Once satisfied with the example, [submit your pull request](https://help.github.com/articles/creating-a-pull-request/).

### Contributing A JavaScript Example

With a JavaScript example you start by creating a new `.html` file in `live-examples/js-examples`. The same naming convention applies here as it does for CSS. In this example we are going to contribute an example demonstrating the use of `Array.from` so, we'll create a new file called `array-from.html`.

Next, you need to paste the following code into this new file (this will be the same for all JavaScript examples you add):

```
<pre>
<code id="static-js">
</code>
</pre>
```

Inside the `code` block is where our example code will go. Change the code to read as follows:

```
<pre>
<code id="static-js">// call from(), passing a string
let result = Array.from('foo');

// log the result
console.log(result);
</code>
</pre>
```

That is all for the live example piece. All that remains is to tell the page generator about our new example. To do this, open up `site.json` at the root of the project folder. Under `pages`, find an existing entry with a `type` of `js`.

Copy and paste the example then update it to your new example, noting that pages are grouped by `type`, and then alphabetically for each `type`.

You entry will look something like the following when edited:

```
"arrayFrom": {
    "baseTmpl": "tmpl/live-js-tmpl.html",
    "exampleCode": "live-examples/js-examples/array-from.html",
    "fileName": "array-from.html",
    "type": "js"
},
```

You're done. All that remains is testing that your page generates and displays as intended, then submitting a pull request.

From your command line run:

```
npm run build
```

Once this completes run:

```
npm start
```

Point your browser to:

[localhost:8080/pages/js/array-from.html](http://localhost:8000/pages/js/array-from.html)

Once satisfied with the example, [submit your pull request](https://help.github.com/articles/creating-a-pull-request/).

## Thank you!

Thank you for your contribution ~ o/\o
