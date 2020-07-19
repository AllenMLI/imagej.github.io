{{ImgLib1_Deprecation_Notice}}
{{ImgLibMenu}}
The purpose of ImgLib, a Generic Java Image Processing Library, is to provide an abstract framework enabling Java developers to design and implement data processing algorithms without having to consider dimensionality, type of data (e.g. byte, float, complex float), or strategies for data access (e.g. linear arrays, cells, paged cells).

This kind of programming has significant advantages over the classical way. An algorithm written once for a certain class of Type will potentially run on any compatible Type, even if it does not exist yet. Same applies for data access strategies and the number of dimensions.

We achieve this abstraction by accessing data through Iterators and Type interfaces. Iterators guarantee efficient traversal through pixels depending on whether random coordinate access is required or just all pixels have to be visited once, whether real or integer coordinates are accessed, whether coordinates outside of image boundaries are accessed or not. Type interfaces define the supported operators on pixel values (like basic algebra) and hide the underlying basic type from the algorithmic implementation.

Imglib provides interfaces to ImageJ for importing, exporting and visualizing data.

As a consequence of its generic aims, the library allows and requires a different way of designing and implementing algorithms. We will first give an overview of ImgLib and its basic design elements and will then hands-on implement a selection of simple algorithms to mediate the way of programming.

==Workshop Material==

#[https://fiji.sc/~preibisch/ImgLibPresentation.pdf An overview of the workshop]
#[http://fly.mpi-cbg.de/~preibisch/pubs/imagejpaper2010.pdf The corresponding ImgLib publication in the Proceedings of the ImageJ Conference 2010]
#[https://fiji.sc/~preibisch/workshop.zip Code examples and images including all libraries (16MB, for non-Fiji users)]
#[https://fiji.sc/~preibisch/workshop2.zip Code examples and images without libraries (4MB, for Fiji users)]
#[http://fly.mpi-cbg.de/~saalfeld/workshop3.zip Code examples without images and libraries (15KB, for Fiji users)]

==References==

#[http://imagejconf.tudor.lu/program/doku.php?id=:program:workshops:preibisch:start ImageJ Conference 2010] programme entry
#[mailto:fiji@fiji.sc Fiji-devel mailing list] discusses imglib development and questions
#[[Imglib|More about ImgLib on the Fiji wiki]]
[[Category:ImgLib]]
