# Events in filters
The class/interface `Filter` has a built-in event manager so that you can specify callbacks for specific events from the outside. In Javascript, the most common example is in the context of asynchronous processes, like AJAX requests, or with device interaction like mouse/keyboard events.

For example, in the class `FileImageReader`, the *\_run()* method triggers the custom event `ready` when the desired image is loaded. Just like that:

```javascript
...
img.onload = function(){

  // in the "onload" method, the image is already loaded
  // but we still have to convert it into an Image2D
  ...

  // the "ready" can now be called
  that.triggerEvent("ready");
}
...
```

From the outside (see **examples/urlToImage2D.html**) the user must define the callback relative to the `ready` event. Most of the time, it will be an anonymous function, just like that:

```javascript
// The filter to read image from URL
var url2ImgFilter = new pixpipe.UrlImageReader();

// the input of this reader is a simple string url. Can be local of distant, but if distant, the server must allow CORS.
url2ImgFilter.addInput( "images/sd.jpg" );

// the image is loaded...
// here, this = url2ImgFilter
url2ImgFilter.on("ready", function(){
  // get the output image
  myImage = this.getOutput();

  ...
})

// Ask to fetch the image from URL
url2ImgFilter.update();
```

In an asynchronous context like this one, it does not really matter if we `update` the filter before or after having defined the `ready` event. In the time necessary to load the data, the main *thread* will continue and will have plenty of time to define the events declared just after.  
On the contrary, in a synchronous context, `update` should always be called **after** having defined all the event of this filter.

Let's take another example: **examples/forEachPixel.html**. This short test loads local images and calls an anonymous function on each and every pixel of it using the filter `ForEachPixelImageFilter`.  
Let's see what happens:

```javascript
...
// first event, the file is finally loaded
url2ImgFilter.on( "ready", function(){

  // this filter needs a `pixel` event, a function to be applied on each pixel
  var forEachPixelFilter = new pixpipe.ForEachPixelImageFilter();

  // add the input input
  forEachPixelFilter.addInput( this.getOutput() );

  // Apply this treatment to each pixel of the image.
  // args:
  //   position: 2D position of current pixel as an Object {x, y}
  //   color: array of colors of the current pixel.
  //
  // Must return, or null to not modify the current color,
  // or an array of the same size of color.
  forEachPixelFilter.on( "pixel", function(position, color){
    return [
      color[1], // red (takes the values from green)
      color[0], // green (takes the values from red)
      color[2] * 0.5, // blue get 50% darker
      255 // alpha, at max
    ]
  });

  // run the filter
  forEachPixelFilter.update();

  ...
});
```

Once we are inside the `ready` event's callback, there is nothing asynchronous anymore, and `update()` will call `_run()` that will call the `pixel` event on each pixel of the image. This means the `pixel` event **must** be defined before calling `update()`.


## Events methods available
As seen on the previous part, when using a filter (from the outside), an event should be defined using the method `.on(...)` as follow:

```javascript
myFilter.on( 'myEventName', function( /* possibly some args */ ){
  ...

  // returning is not mandatory
  return 20;
});
```

Depending on how you call it from your filter, you may or may not have arguments.  
The same goes with returned values, if your custom event internally is expected to return something, so be it (see the previous part, with the `pixel` event).  
Keep in mind that it's not because you define an event with `.on(...)` that it will necessary be called, your filter needs to explicitly call it at some point.

Internally, triggering an event is fairly simple. Here is an example with no arguments and no returned values:

```javascript
// here, this is a filter, from the inside
this.triggerEvent("ready");
```

The case of `ForEachPixelImageFilter` is a bit more complete since it triggers the event `pixel` with few arguments and handles a returned value:

```javascript
var newColor = this.triggerEvent("pixel", position2D, currentColor);
```
As shown, if an event needs to be triggered with some arguments, just put them after the name of the event itself.  About the returned value, the one of your callback function is just relayed by the method `triggerEvent`.

If you happen to trigger an event that was not defined (using `triggerEvent`), a warning will be printed in the console but no exception is raised.  
In addition to `triggerEvent`, the `Filter` class comes with `hasEvent` so that you can check in advance the existance of an event:

```javascript
// internally from a custom filter
if( this.hasEvent("dataProcessed") ){
  this.triggerEvent( "dataProcessed" );
}else{
  // do something else, maybe call a default built-in event...
}
```


# Measuring time
The class/interface `PixpipeObject` has a built-in system to measure time that any other Pixpipe object can use. It can be useful to track performance and know where to look for further optimization. Here is how to use it, internally from your custom filter:

```javascript
...
this.addTimeRecord('step1');
// do things ...
this.addTimeRecord('step2');
// do things ...
this.addTimeRecord('step3');

this.getTime('step2', 'step3', true);
var t = this.getTime('step1', 'step3');

console.log("The whole thing took " + t + " millisec.");
...
```

The method `.addTimeRecord` creates a new entry and the method `.getTime` returns the time elapsed from a step (arg 1) to another (arg 2) in milliseconds. In addition to return, it can also print in the JS console when a third argument is set to `true`.


# Various optimizations
As said the [step-by-step](#simple-example) part, processing an image means processing very large typed arrays, and saving a single millisecond per pixel really matters!

For some algorithm, you may need to use information from metadata extensively (ie. in a loop). If such metadata is initialized in the constructor or above in your code, using it like that:

```javascript
this._metadata.width
```

is faster than that:

```javascript
this._metadata["width"]
```

which is faster than that:

```javascript
this.getMetadata("width")
```

Using the getter is nice because it provides a layer of control and returns null if the metadata does not exist but it's slower, so as long as you are using a metadata internally (from `this`) and that you know what you are doing (you are **sure** this metadata actually exists), then it's ok to call it like a regular attributes, with the *dot* operator.
