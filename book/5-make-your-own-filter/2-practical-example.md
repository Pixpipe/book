# A Practical example
In this section, we will go step by step into a simple example: a filter that performs a simple threshold on an image.  
You can find this filter in the repo at `src/filter/SimpleThresholdFilter.js` ([source](https://github.com/Pixpipe/pixpipejs/blob/master/src/filter/SimpleThresholdFilter.js)).  
This filter is simple enough to not have a source cluttered with complicated algorithmic details.  
Along with the filter, an example of its usage will help you to better understand the logic: `examples/imageThresholding.html` ([source](https://github.com/Pixpipe/pixpipejs/blob/master/examples/imageThresholding.html), you can also [run this example](http://www.pixpipe.io/pixpipejs/examples/imageThresholding.html)).


The following is a step-by-step explanation of what happens in this filter.


## First, the structure  
The structure is quite basic and very [ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_2015_support_in_Mozilla)-style.
```javascript
// make Image2D and ImageToImageFilter visible from here because we will use them
import { Image2D } from '../core/Image2D.js';
import { ImageToImageFilter } from '../core/ImageToImageFilter.js';

// Our new class SimpleThresholdFilter extends from the interface ImageToImageFilter
class SimpleThresholdFilter extends ImageToImageFilter {
  constructor(){
    // ... to be implemented
  }

  _run(){
    // ... to be implemented
  }

} /* END of class SimpleThresholdFilter */

// make SimpleThresholdFilter importable
export { SimpleThresholdFilter }
```

Depending on what your own filter will do, you may want to inherit directly from `Filter` rather than from `ImageToImageFilter`. It's up to you and like usually in programming, it's not like there is a single right answer.


## Adding the constructor
Since we inherit from a class, the first thing to do in the constructor is to call the constructor of the parent class using `super()`:

```javascript
// make Image2D and ImageToImageFilter visible from here because we will use them
import { Image2D } from '../core/Image2D.js';
import { ImageToImageFilter } from '../core/ImageToImageFilter.js';

// Our new class SimpleThresholdFilter extends from the interface ImageToImageFilter
class SimpleThresholdFilter extends ImageToImageFilter {

  constructor(){
    super();

    // default values
    this.setMetadata("threshold", 128);
    this.setMetadata("lowValue", 0);
    this.setMetadata("highValue", 255);
    this.setMetadata("preserveAlpha", true);
  }


  _run(){
    // ... to be implemented
  }

} /* END of class SimpleThresholdFilter */

// make SimpleThresholdFilter importable
export { SimpleThresholdFilter }
```
You know how a threshold works, right? under a certain value a pixel is set to black and over, it's set to white. But, we have to take in consideration two more things:
1. Our image may have more than 8bit per pixel (standard [0, 255]) but could be float [0.0, 1.0], 12bit ints [0, 4096], 16bits ints [0, 65535], etc.
2. Our image may have 1, or 3 or 4 or more channels. For the sake of simplicity, we will make this filter compatible with single band (intensity), 3 bands (RGB) and 4 bands (RGBA) images.

Still, we want our filter to have built-in default values for the `threshold`, the value to give when under(`lowValue`), the value to give when over (`highValue`) and also if we should preserve the transparency in the case of RGBA images (`preserveAlpha`).


## The \_run() method
All the following takes place in the scope of the `_run()` method, like here:

```javascript
...

_run(){
  // HERE!
}

...
```

Remember `_run()` is called when you explicitely call `update()`, and you should not call `_run()` directly!  
At this stage, if your filter *runs*, it means it **should** already have an input. Let's make sure of it:

```javascript
// the input checking
// the input checking
if( ! this.hasValidInput())
  return;
...
```

The method `hasValidInput()` is a legacy from the class `Filter`. It checks if a filters input is actually like it should be. Since our filter inherits from `ImageToImageFilter`, it's constructor (when calling `super()`) already register that the filter should expect an input of type `Image2D` and of *category* "0".  

To manually add an expected input, just add an entry to the `inputValidator` by calling the method `this.addInputValidator` from the constructor (after `super()`). For example, if our filter was requiring 2 `Image2D` of category 0 and 1, then we would add that to the constructor:

```javascript
this.addInputValidator(0, Image2D);
this.addInputValidator(1, Image2D);
```

Then, in `_run()`, calling `hasValidInput()` in a *if* statement would make sure our input are actually two `Image2D` of the given categories.  
If we have no input, we just quit with a warning message. There is no exception to launch, we just don't have any input, it's not a big deal and we may want to add one later.

Next, we build some local variables. In Pixpipe, we will often be fetching very large TypedArrays think of it like that: a 1000px by 1000px png (most likely RGBA) is already an array of size 4 million! And nowadays, 1000px by 1000px is not that much so we always have to think about little optimizations because, on a large scale, they will matter **a lot**.  
So, why local variables? Simply because in Javascript they are faster to fetch than complex objects. Here is how:

```javascript
...

var inputImg = this._getInput( 0 );

// Number of bands
var ncpp = inputImg.getComponentsPerPixel();

// having a local value is faster than fetching an object
var threshold = this.getMetadata("threshold");
var lowValue = this.getMetadata("lowValue");
var highValue = this.getMetadata("highValue");

...
```

Then we will start to create the content of our output image, but not the `Image2D` object yet. First we *copy* the data array of the input image - copy in the sense of **duplicate** (and not copy of *reference*):

```javascript
...

// get a copy of the input buffer so that we dont overwrite it!
var outputBuffer = inputImg.getDataCopy();

...
```

If we wanted to get a reference to the original data array of the input image, we would have used `inputImg.getData()` instead of `inputImg.getDataCopy()`. Then modifying the array would change the content of the input image and we don't want that.

Now, we are dealing with checking the number of components per pixel, remember, we want this filter to accept only 1, 3 and 4 components images:

```javascript
...

// if the input image has:
// - a single band, OR
// - three bands (assuming RGB), OR
// - four bands (assuming RGBA)
if(ncpp == 1 || ncpp == 3 || ncpp == 4){

  // later, a few things will go here

}else{
  outputBuffer = null;
  console.warn("The input data must have 1, 3 or 4 components per pixel.");
  return;
}

...
```

As you can see, if the number of components per pixel (aka. `ncpp` or as previously said *channels*) is not like we want, we just quit the function before having mapped any output. Still, we want to rush the *garbage collector* and remain with a potentially large array copied in memory, so we call `outputBuffer = null;`. Though this is not mandatory and the *garbage collector* will come anyway.

Then, let's fill this `if`:

```javascript
...
if(ncpp == 1 || ncpp == 3 || ncpp == 4){


  /**************** PART 1 *********************************************/

  // we want to preserve transparency ( = not affected by thresholding)
  if( this.getMetadata("preserveAlpha") && ncpp == 4){

  /**************** PART 1.1 *********************************************/
    for(var i=0; i<outputBuffer.length; i++){
      // every four band is an alpha band
      if(i%4 == 3){
        continue;
      }
      outputBuffer[i] = outputBuffer[i] < threshold ? lowValue : highValue;
    }

  // transparency is altered by the threshold like any other channel
  }else{
  /**************** PART 1.2 *********************************************/
    for(var i=0; i<outputBuffer.length; i++){
      outputBuffer[i] = outputBuffer[i] < threshold ? lowValue : highValue;
    }
  }

  /**************** PART 2 *********************************************/

  /**************** PART 2.1 *********************************************/
  // creating a blank Image2D output and getting the ref
  var outputImg = this._addOutput( Image2D );

  /**************** PART 2.2 *********************************************/
  // filling it with actual data
  outputImg.setData(
    outputBuffer,
    inputImg.getWidth(),
    inputImg.getHeight(),
    ncpp
  );



}else{
  outputBuffer = null;
  console.warn("The input data must have 1, 3 or 4 components per pixel.");
  return;
}
...
```

For the sake of readability, the content was split into different parts.

**Part 1** is about filling the output array based on its own content. In **Part 1.1** we do care about the alpha channel when we have 4 components, which consists in *skipping* every 4 channel. In **Part 1.2**, we don't care of what channel we are processing, we threshold them all.  

**Part 2** is about creating the output `Image2D`. In **Part 2.1**, we can consider `_addOutput` as a sort of factory that will create a *constructed but blank* instance of `Image2D` and will automatically map it to an output of this filter. We could have given this output a specific *category* by adding another argument: `this._addOutput( Image2D, "myOutputCategory" );`. Since we call it without any *category* argument, the default will be *"0"*.  
*Part 2.2* is about associating the typed array we just modified in the loop to the freshly created output image. For that, we give the array, but also the size and the number of components per pixel.

Then, don't forget to [register your filter](1-lets-start.md#register-your-filter) so that you can actually use it!


## Could we do it another way?
Sure, in many other ways. A simple alternative would have been to:

Make a clone of the input image:

```javascript
...
var inputImg = this._getInput( 0 );
var outputImage = inputImg.clone();
var outputBuffer = outputImage.getData();
...
```

Process `outputBuffer` the very same way, and in the end, creating mapping manually `outputImage` as an output with explicitly giving the category (*"0"* in this case):

```javascript
...
this._output[0] = outputImage;
...
```
And that's all, no need to call `this._addOutput` or `outputImg.setData` because since `outputImg` is a clone, it already carries the information about size and number of components.

This method is actually faster and potentially even easier to understand. The only disadvantage is that it lets the developer dealing with the output *category*, which might be a source of error when running the filter.
