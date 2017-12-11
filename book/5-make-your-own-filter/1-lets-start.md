# What should my filter inherit from?
All filter should inherit from `Filter`, but some subtypes can also be used. For example, your custom filter can inherit from the class/interface `ImageToImageFilter`, which has the advantage of having some Image2D checking methods already built-in.

The class `ImageToImageFilter` itself inherits from `Filter` and we could imagine creating other *interface* filters that carry some specific logic but no actual data processing.


# What are inputs and outputs?
Input and output data can be of different types but are usually PixpipeContainer. They are stored in a `Filter`'s attribute under the name `this._input` and `this._output`. These two objects are actually *maps* and need a **category**.

A *category* is just a way to give an identifier when adding an input (externally) or an output (internally, to create it). If your filter needs only a single input, adding a category is optional (*"0"* will be given). Same thing for the output, if your filter creates only one, then it's category should be *"0"* because the method `.getOutput()` uses a default value *"0"* for the *category* argument. The concept of *category* becomes very convenient when your filter has to manage multiple I/O.

From the outside of your custom filter, here are the methods you can use that are already part of the `Filter` class:

```javascript
// Create a instance of your custom filter
var myFilter = new pixpipe.MyCustomFilterType();

// we have only one input, no need to specify a category
myFilter.addInput( myImage2D );

// if we have more than one input, we identify them with a "category"
myFilter.addInput( myImage2D_1, 0);
myFilter.addInput( myImage2D_2, 1);

// Note that the category can also be a String
myFilter.addInput( myImage2D_1, "image_one");
myFilter.addInput( myImage2D_2, "image_two");
...

// run the filter
myFilter.update()
...

// If your filter has a single output, no need to
var myOutput = myFilter.getOutput();

// If your filter has more than one output
var myFirstOutput = myFilter.getOutput(0);
var myFirstOutput = myFilter.getOutput(1);

// Note that the category can also be a String
var myFirstOutput = myFilter.getOutput("the_noise_image");
var myFirstOutput = myFilter.getOutput("the_signal_image");

// get the number of input categories (already given)
var numOfInputs = myFilter.getNumberOfInputs();

// get the number of output categories
var numOfOutputs = myFilter.getNumberOfOutputs();

// You can also batch process the outputs
myFilter.forEachOutput( function( category, outputObject ){
  // do something
});

// to know if a filter has an output of a given category
if( myFilter.hasInputOfCategory("the_signal_image") ){
  // do something
}

// get the list of all input categories given to the filter
var allInputCat = myFilter.getInputCategories();

// get the list of all output categories given to the filter
var allOutputCat = myFilter.getOutputCategories();

// tells if after calling update() there is at least one output created
var isReady = myFilter.hasOutputReady();

```

From the inside of your filter, there are a few other (supposedly private) I/O-related methods:

```javascript
// get an input when we know we have only one
var theInput = this._getInput();

// get an input of a given category
var theInput = this._getInput("image_one");

// batch process the inputs, when we have many of them
this._forEachInput( function(category, inputObject){
  // do something with each input
})

// add a new output of a given type that encapsulate the category checking
this._addOutput( Image2D );

// the same but with a given category
this._addOutput( Image2D, "the_first_output" );

// then retrieve the blank output
var outputImg = this.getOutput();

// and initialize it with real values
outputImg.setData(
  bufferCopy,
  inputImage2D.getWidth(),
  inputImage2D.getHeight(),
  inputImage2D.getComponentsPerPixel()
);

```


# What are metadata?
A filter can also accept any kind of metadata so that it can do its job properly. The can be used to store the settings of an algorithm, default values, etc. They can be accessed from the outside as well as from the inside.    
In the case of a filter that performs an algorithm, all the settings must be stored as *metadata* and we strongly discourage to use *class attributes* for that (see serialization). As shown on the [diagram](/#core-description/), the metadata logic is hosted by the interface called `PixpipeObject`. The class `Filter` and every other object that belong to the Pixpipe project inherit from `PixpipeObject`.

Here are the metadata-related methods you can use from the outside:

```javascript
// Create a instance of your custom filter
var myFilter = new pixpipe.MyCustomFilterType();

// Specify a metadata
myFilter.setMetadata("threshold", 100);

// retrieve a metadata
var threshold = getMetadata("threshold");

// check the existance of a given metadata
if( myFilter.hasMetadata( "threshold" ) ){
  // do something
}

// retrieve all the metadata names already specified
var allMetaNames = myFilter.getMetadataKeys();

// Copy all the metadata from another object
myFilter.copyMetadataFrom( anotherFilter );
```


# Can I add class attributes?
Yes, like any other *class*, your custom filter can use attributes to store temporary data, but not for inputs, outputs or metadata of major importance.


# How to make the filter runnable?
As seen earlier, to run a filter, the method `.update()` should be called, though, the method that should be implemented in every new custom filter is `_run()`.  
The `_run()` method is called by `update()` along with some others (this logic is implemented in the mother class `Filter`). It should perform the reading of inputs, the processing and the writing of outputs.

**A FILTER SHOULD NEVER ALTER THE INPUT** <-- *this is an important rule, you must not break it*


# Keep in mind
A filter may need to be compatible with different kind of inputs and it is to the discretion of the developer to develop it properly. For example, a filter that takes an *Image2D* as input has to deal with a variable number of component per pixel so that it behaves in an expected way with a single-band image (b&w) but also with an RGB or RGBA image.  

If a filter can deal only with a certain kind of input, it should be explicitly stated in the documentation.


# Register your filter
To make your custom filter accessible when building Pixpipe, you must register it in the file `src/pixpipe.js`:  
```Javascript
export { MyCustomFilter } from './filter/MyCustomFilter';
```
