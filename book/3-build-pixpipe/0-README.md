# Building Pixpipe
By default, Pixpipe already comes with a built version in the `dist` directory. Still, if you modify it, you may want to build your own version.  
In this case, clone it or download it from the *master*, then, open a terminal, and:  

```bash
$ cd pixpipejs
$ npm install
$ npm run build
```

This will generate a ES5-compatible `umd` *bundle* file in the `dist` directory: `pixpipe.js`.  
This single source file can be imported in a *html* page like any other js file:

```html
<script src="a/path/to/pixpipe.js"></script>
```
The `build` command will also make a ES5-compatible *commonJS* source that is easier to import in a bigger project: `pixpipe.cjs.js`.


Note that Pixpipejs is developed using ES6 and might not be compatible with older browsers -- Google closure compiler to the rescue! In addition, to provide a nice and easy way to minify the built version, it also transpiles the codebase into ES5. Here is the command:

```bash
$ npm run build
$ npm run min
```

**Note** Pixpipe does not use an npm minifier plugin because it's codebase is too large, thus it uses directly the closure compiler from Google (see `closurecompiler` folder).

# Development mode
In addition to the regular building mode, you can work in development mode:

```bash
$ cd pixpipejs
$ npm install
$ npm run dev
```
This is has two advantages over the regular build mode:
- It lauches a local webserver
- It builds only the `umd` package and keeps it as ES6, which is faster since there is no transpiling involved. (but please don't distribute a ES6 bundle).


# Building the documentation
Pixpipejs uses [DocumentationJS](http://documentation.js.org/) to generate a HTML documentation. It uses *JSDoc* syntax and is generated with the following command:

```bash
$ npm run doc
```

Then, the documentation is accessible in the `doc` folder or can be directly read [here](http://pixpipe.github.io/pixpipejs/doc/).

# Build the whole thing
If you want to build the project, have a minified version and build the doc, then do:
```bash
$ npm run all
```
