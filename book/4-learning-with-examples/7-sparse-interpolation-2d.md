# Image2D interpolation
For the moment, interpolation in Pixpipe is only on sparse point set.

- [2D sparse dataset nearest neighbor](http://pixpipe.github.io/pixpipejs/examples/nearestSparseInterpolation.html) | [source](https://github.com/Pixpipe/pixpipejs/tree/master/examples/nearestSparseInterpolation.html) - basic and not really an interpolation
- [2D sparse dataset triangulation](http://pixpipe.github.io/pixpipejs/examples/TriangleSparseInterpolation.html) | [source](https://github.com/Pixpipe/pixpipejs/tree/master/examples/TriangleSparseInterpolation.html) - linear, not suited for non linear data
- [2D sparse dataset inverse distance weighting](http://pixpipe.github.io/pixpipejs/examples/IDWSparseInterpolation.html) | [source](https://github.com/Pixpipe/pixpipejs/tree/master/examples/IDWSparseInterpolation.html) - a classic in GIS, though kind of flawed by local minimas
- [2D sparse dataset natural neighbors](http://pixpipe.github.io/pixpipejs/examples/NaturalNeighborSparseInterpolation.html) | [source](https://github.com/Pixpipe/pixpipejs/tree/master/examples/NaturalNeighborSparseInterpolation.html) - close to the *real* function but slow to compute
