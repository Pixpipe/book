<p style="text-align:center;"><img src="images/org_logo_vector_rounded.png" width="128"></p>  
**Hello there**, if you want to learn how to use [Pixpipe](https://github.com/Pixpipe/pixpipejs), you're at the right place! This cookbook was made to cover the majority of use-cases and provide enough material to get you started, though it does not cover 100% of Pixpipe. If you have a question or would like to see an additional section in this book, please raise [an issue on the cookbook's repo](https://github.com/Pixpipe/cookbook/issues/new).

# Install for production
```bash
npm install --save pixpipe
```

The `dist` folder of Pixpipe contains several pre-built bundles:
1. `pixpipe.js` is the default *umd* bundle you can import from an HTML document, from an ES module or *require* from node. It is transpiled into ES5 for broader compatibility.
2. `pixpipe.min.js` is the same as `pixpipe.js` but minified, which makes it about 3 times smaller
3. `pixpipe.es6.js` is also a *umd* bundle that goes more or less anywhere but its syntax remain ES6, which is the original Pixpipe syntax
4. `pixpipe.esmodule.js` is a *es module*, which is specialy baked to be imported (using `import`) from another *es module*. You cannot use this version directly in a HTML page with the `<script>` markups or `require` it from Node.

The file that will then be imported is `dist/pixpipe.js` **[1]**, because it is defined as `main` in the `package.json` file. The **[2]** and **[3]** are mostly alternative to this main bundle, to be use in more of less the same conditions.  
While they can all be imported in a *es module*, only the **[4]** provides an optimal way to do it, by selecting only the class you need and thus make a smaller final bundle.

**In HTML file**

```html
<!-- Import the ES5 UMD bundle -->
<script src="pixpipe/dist/pixpipe.js"></script>

<!-- or alternatively, the ES6 UMD bundle -->
<script src="pixpipe/dist/pixpipe.min.js"></script>

<!-- or alternatively, the minified ES5 UMD bundle -->
<script src="pixpipe/dist/pixpipe.min.js"></script>
```

**In Node**
```js
const pixpipe = require("pixpipe");
```

**In ES module**
This imports **ALL** Pixpipe:
```js
import pixpipe from 'pixpipe'
```

To have a finer granularity on what part of Pixpipe you want to import (and thus have a smaller final bundle), you can also use the *esmodule* version of Pixpipe:
```js
import { Image2D, Image3D } from 'pixpipe/dist/pixpipe.esmodule.js'
```
