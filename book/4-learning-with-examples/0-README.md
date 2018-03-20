# Learning with examples
The following examples are a nice and easy way to progressively go through using Pixpipe, from the basics to advanced. It is important to understand how to use Pixpipe before trying to develop your own filters in order to respect the coherence that makes it easy to use.  
You can run and explore the source of all the examples, they are simple HTML pages and links are all provided.

Before starting, remember these two things:
- Filters take one or more input using the method `addInput()`, must be ran using `update()` and produce one or more output that can be accessed with `getOutput()`
- When a filter takes multiple inputs, the method `addInput()` takes an additional argument to identify it. Read the [code documentation](pixpipe.github.io/pixpipejs/doc/) to find out what are the *categories* of the expected inputs.
- The method `getOutput()` has the same feature. If multiple are expected, the method will take an argument (*string*) that is the *category* of the output. Though you can alway use the method `.forEachOutput(...)` - [doc](http://www.pixpipe.io/pixpipejs/doc/#Filter.forEachOutput)
- When it comes to containers' metadata (particular setting, overwriting a default algorithm value, etc.), the filter method `.setMetadata(...)` must be called.
- A `Filter` should **NEVER** modify the input data and it's the duty of the developer to respect that.  

```Javascript
// the object myImage comes from a file that you previously opened

var myFilter = new pixpipe.AmazingFilter();

// Some filter have specific setting, if so, they have default values but you can still change them:
myFilter.setMetadata( "someSetting", 42 );
myFilter.setMetadata( "someOtherSetting", "gaussian" );

// If the filter is supposed to take a single input, this is enough:
myFilter.addInput( myImage );
// and this is exactly the same as doing
myFilter.addInput( myImage, 0 );

// But is the filter expect multiple inputs, we have to specify the "category" of each:
myFilter.addInput( myImage, "principalImage" );
myFilter.addInput( myImage2, "secondaryImage" );

// run the filter
myFilter.update();

// Get the output from the filter. Can be null if not successful:
var myOutputImage = myFilter.getOutput();
// which is the same as
var myOutputImage = myFilter.getOutput( 0 );

// but a filter can produce multiple outputs. In this case, they are identified with a "category":
var myOutputImageBlue = myFilter.getOutput( "blueBand" );
var myOutputImageRed = myFilter.getOutput( "redBand" );

// in any case, before going further, ew should test this output:
if( myOutputImage ){
  //...
  // do something here
}

// If a filter produces multiple outputs, you can also access them like that:
myFilter.forEachOutput( function( category, out ){
  // where "category" is a string
  // and "out" is the output object, e.g. an instance of Image2D
});
```

Processing images/volumes usually take a bit of time but there is not always a graphic feedback. Open your javascript console so that you can see the filter feedback in the log.  
The examples from the following pages are in the [example folder](https://github.com/Pixpipe/pixpipejs/tree/master/examples) of Pixpipe's repo.
