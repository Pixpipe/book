<p style="text-align:center;"><img src="images/org_logo_vector_rounded.png" width="128"></p>  
**Hello there**, if you want to learn how to use [Pixpipe](https://github.com/Pixpipe/pixpipejs), you're at the right place! This cookbook was made to cover the majority of use-cases and provide enough material to get you started, though it does not cover 100% of Pixpipe. If you have a question or would like to see an additional section in this book, please raise [an issue on the cookbook's repo](https://github.com/Pixpipe/cookbook/issues/new).

# Install for production
```bash
npm install --save pixpipe
```

The file that will then be imported is `dist/pixpipe.js`. It is a [umd](https://github.com/umdjs/umd) bundle, meaning it's ugly but it's compatible everywhere: Node, Browsers and ES modules. Still, Pixpipe won't work on **old** browsers (i.e. the one that don't support typed arrays).

**In HTML file**
```html
<script src="pixpipe/dist/pixpipe.js"></script>
<!-- or alternatively -->
<script src="pixpipe/dist/pixpipe.min.js"></script>
```

**In Node**
```js
const pixpipe = require("pixpipe");
```

**In ES module**
```js
import pixpipe from 'pixpipe';
```
