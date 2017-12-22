# PixpipeObject
PixpipeObject is the common interface to every object in Pixpipe. They implement the logic of *metadata*, among others. [documentation](http://www.pixpipe.io/pixpipejs/doc/#PixpipeObject).


# PixpipeContainer
Directly inheriting from PixpipeObject, the container has a `_data` attribute but this one is still generic. Any new container must inherit from `PixpipeContainer`. Read more about them in the [documentation](http://www.pixpipe.io/pixpipejs/doc/#PixpipeContainer).


## Signal1D
Pixpipe container for mono dimensional data, such as a time series (e.g. electroencephalogram). They can also contain as many metadata fields as needed. Read more about them in the [documentation](http://www.pixpipe.io/pixpipejs/doc/#Signal1D).


## Image2D
Pixpipe container for two dimensional dataset such as image. They can be of any size and have as many band as necessary. They can also contain as many metadata fields as needed. Read more about them in the [documentation](http://www.pixpipe.io/pixpipejs/doc/#Image2D).


## Image3D
[soon to be replaced by Image3DAlt] Pixpipe container for 3D and 3D+time dataset such as MRI or fMRI. They can be of any size and have as many band as necessary. They can also contain as many metadata fields as needed. Read more about them in the [documentation](http://www.pixpipe.io/pixpipejs/doc/#Image3D).


## Image3DAlt
Pixpipe container for 3D and 3D+time dataset such as MRI or fMRI. They can be of any size and have as many band as necessary and handle multiple affine transformations. They can also contain as many metadata fields as needed. Read more about them in the [documentation](http://www.pixpipe.io/pixpipejs/doc/#Image3DAlt).


## Mesh3D
Pixpipe container for surface dataset. They can be of any size and have as many band as necessary. They can also contain as many metadata fields as needed. Read more about them in the [documentation](http://www.pixpipe.io/pixpipejs/doc/#Mesh3D).


## LineString
Pixpipe container for vectorial multipoint lines. They can also represent a closed polygon or a single point in a 3D space. They can contain as many metadata fields as needed. Read more about them in the [documentation](http://www.pixpipe.io/pixpipejs/doc/#LineString).


## Category
The category is an identifier internally used by filter. They qualify an instance of a data container (see PixpipeContainer) and help Filters to know what input is what.


## Categories
The category is an identifier internally used by filter. They qualify an instance of a data container (see PixpipeContainer) and help Filters to know what input is what.


## Metadata
Metadata are information stored by a data container (any PixpipeContainer) alongside the data. Metadata can be anything except large typed arrays. They are sometimes used to store  informations that are not essential (description, date, etc.), but they can also store data that are mandatory to use the container properly (width, height, affine transformations, etc.).  
Metadata are usually considered *public* because they can be accessed from the outside using the method `.getMetadata(string)`.


## Serialization
The serialization is the process to turn a data container (PixpipeContainer) into a file. This file can then be read by Pixpipe to be "de-serialized" and produce a PixpipeContainer similar to the original. The serialization handles only the attributes `_data` and `_metadata`, for this reason it is unsafe to store information in other attributes. They would be lost in the serialization process.

## BVH
Bounding Volume Hierarchy. Commonly used in video games to perform hit tests in real time between triangles and rays, a BVH is a way to arrange the triangles as a binary tree to perform fast lookups. As a comparison, octrees are usually used for sigle points spatial lookup while BVH is for shapes (triangles or more complex)
