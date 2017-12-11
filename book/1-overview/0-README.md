# Pixpipe Overview
The point of Pixpipe is to be easy to use and easy to contribute to. This goal led to take some decisions:  
- using a source bundler ([Rollup](http://rollupjs.org/))
- properly define a folder hierarchy within `src`:
  - `core` for the most low level *interfaces* and *classes*
  - `decoder` for specific file decoding and encoding
  - `filter` for all the filters (this could be arranged in a better way)
  - `helper` are like filters but the processing they provide is to help visualize data, rather that applying a scientifically relevant treatment. (ie. apply a colormap)
  - `io` for downloading/reading/writing files from the filesystem or AJAX
  - `utils` for hosting static functions (algebra, geometry, etc.)
  - `pixpipe.js` the main entry point where are listed all the modules
- A modular approach and a clear separation of objects.
- Once built, Pixpipe is only a single file, located in `dist`, so that's it's easy to import

If at any moment you need to know more about a specific function or class, you can read the [code documentation](pixpipe.github.io/pixpipejs/doc/).

# Projects used in Pixpipe
Sometimes, it's just not worth reinventing the wheel. Here are the libraries Pixpipe uses and includes at build time:
- [Pako](https://github.com/nodeca/pako), for high speed file compression/decompression in JS.
- [FileSaver.js](https://github.com/eligrey/FileSaver.js), to easily trigger file downloading to the user side
- [expr-eval](https://github.com/silentmatt/expr-eval), to evaluate math expression and create quick filter prototype
- [js-md5](https://github.com/emn178/js-md5), to generate a unique checksum for each loaded files
- [geotiff](https://github.com/constantinius/geotiff.js) to decode Tiff and BigTiff images
- [delaunay](https://github.com/ironwallaby/delaunay) Delaunay triangulation
- [jpeg-js](https://github.com/eugeneware/jpeg-js) a pure JS jpeg decoder
- [UPNG.js](https://github.com/photopea/UPNG.js) a pure JS png decoder
- [gl-matrix](http://glmatrix.net/) a great linear algebra lib. Its name comes from that it's compatible with WebGL vector/Matrix format (column major typed arrays)
- [joi-browser](https://github.com/jeffbski/joi-browser) a Javascript object schema validation lib that is easy to use
- [ndarray](https://github.com/scijs/ndarray) multidim numerical arrays, used for some computations only (FFT), but not as a native data storage in Pixpipe
- [nifti-reader-js](https://github.com/rii-mango/NIFTI-Reader-JS) an efficient NIfTI file parser

In order to make Pixpipejs the way it is, we have developped a few modules that are integrated at bundling time but that can also be used as stand-alone, without Pixpipe. They are all hosted on the [Pixpipe Github organization](https://github.com/Pixpipe):
- [codecutils](https://github.com/Pixpipe/codecutils) provides some static functions to encode or decode data, especially extract numerical array from `ArrayBuffer` without constraint, convert to unicode strings, etc.
- [pixbincodec](https://github.com/Pixpipe/pixbincodec) to read and write into the native Pixpipe format: PixBin files. Although this lib create or reads from `ArrayBuffer`, the exampled provided show how to read from or write to disk depending if you are using JS in a browser of in Node.
- [mniobjparser](https://github.com/Pixpipe/mniobjparser) to parse the 3D mesh/surface files output of CIVET, called MNI OBJ
- [qeegmodfile](https://github.com/Pixpipe/qeegmodfile) a EEG file parser for the MOD format used in the Canada-China-Cuba consortium
- [edfdecoder](https://github.com/Pixpipe/edfdecoder) a EDF (European Data Format) file parser, especially used in EEG


# Sample data
[HERE](https://github.com/Pixpipe/pixpipeData) is the repo where some sample data are stored (mainly to avoid this repos to be too fat).
