# Sass

> Courtesy of [sass-lang.com](https://sass-lang.com/guide)

[Install Instructions](https://sass-lang.com/install)

## SCSS or SASS?

There are two syntaxes available for Sass. The first, known as ```SCSS (Sassy CSS)``` and used throughout this reference, is an extension of the syntax of CSS. This means that **every valid CSS stylesheet is a valid SCSS file** with the same meaning. This syntax is enhanced with the Sass features described below. Files using this syntax have the ```.scss``` extension.

The second and *older syntax*, known as the indented syntax (or sometimes just “```Sass```”), provides a more concise way of writing CSS. It uses indentation rather than brackets to indicate nesting of selectors, and newlines rather than semicolons to separate properties. Files using this syntax have the ```.sass``` extension.


## Preprocessing
CSS on its own can be fun, but stylesheets are getting larger, more complex, and harder to maintain. This is where a preprocessor can help. Sass lets you use features that don't exist in CSS yet like variables, nesting, mixins, inheritance and other nifty goodies that make writing CSS fun again.

Once you start tinkering with Sass, it will take your preprocessed Sass file and save it as a normal CSS file that you can use in your website.

The most direct way to make this happen is in your terminal. Once Sass is installed, you can compile your Sass to CSS using the ```sass``` command. You'll need to tell Sass which file to build from, and where to output CSS to. For example, running ```sass input.scss output.css``` from your terminal would take a single Sass file, ```input.scss```, and compile that file to ```output.css```.

You can also watch individual files or directories with the ```--watch``` flag. The watch flag tells Sass to watch your source files for changes, and re-compile CSS each time you save your Sass. If you wanted to watch (instead of manually build) your ```input.scss``` file, you'd just add the watch flag to your command, like so:

```sass --watch input.scss output.css```

You can watch and output to directories by using folder paths as your input and output, and separating them with a colon. In this example:

```sass --watch app/sass:public/stylesheets```

Sass would watch all files in the ```app/sass``` folder for changes, and compile CSS to the ```public/stylesheets``` folder.

## More...

There is more, but it is so well documented on sass-lang, that it would be redundant to put it here.