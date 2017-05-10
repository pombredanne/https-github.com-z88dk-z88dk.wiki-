# 3D Libraries

The base for this tool (and for this document as well) comes from  OZ Developement kit.
Many new functions coming from the GFX library have recently been added.
See section III for Credits.




## I. Introduction

The 3D Libraries include a standard set of functions for making your own 3D functions. This document is a reference for these functions.



## II. Reference

The following is a reference for the available functions in lib3d.lib. Please note that you will have to include "lib3d.h" in order to use these functions in your code.


### Rotate a vector


	void vector_rotate(vector_t* p, int angle_x, int angle_y, int angle_z, vector_t* r)

Rotate vector p by the given angles, result in r.


	void ozrotatepointx(Vector_t *v, int rot)
	void ozrotatepointy(Vector_t *v, int rot)
	void ozrotatepointz(Vector_t *v, int rot)


Use these functions to rotate a vector around the origin. The variable 'rot' is the rotation factor in degrees (?), not radians (? × p ÷ 180). 

Note: Do not rotate the original coordinates! This will distort an object after a few times of use. Use ozcopyvector() to first copy the coordinates into a temporary variable and rotate them there.

Example:
```
...
Vector_t v;
...
v.x = 10;
v.y = 10;
v.z = 10;
ozrotatepointx(&v, 90);
ozrotatepointy(&v, 90);
ozrotatepointz(&v, 90);
...
```


### Copy a vector


	void ozcopyvector(Vector_t *dest, Vector_t *src)

This function copies a vector's X, Y, and Z coordinates from *src to *dest.

Example:

```
...
Vector_t v1;
Vector_t v2;
...
ozcopyvector(&v2, &v1);
...
```


### Translate a vector


	void vector_add(vector_t *v1, vector_t *v2, vector_t *r)

Add vector v1 with v2, result in r.


	void vector_subtract(vector_t *v1, vector_t *v2, vector_t *r)

Subtract vector v1 by v2, result in r.


	void oztranslatevector(Vector_t *v, Vector_t *offset)

Offset a vector by using this function. It will add the X, Y and Z coordinates from *offset to *v.

Example:
```
...
Vector_t v1;
Vector_t v2;
...
oztranslatevector(&v2, &v1);
...
```


### Scale vector


	void vector_scalar(vector_t *v, int s, vector_t* r)

Scale vector v by s, result in r.


### Vector normalize


	void vector_normalize(vector_t *p, vector_t *r)

Normalize vector p, result in r.


### Vector distance


	int vector_distance (vector_t *v1, vector_t *v2)

Distance between vectors v1 and v2.


### Vector length


	int vector_length(vector_t *v)

Pure length of vector v (it involves sqrt calculus, so it is expensive).


### Vector cross product


	void vector_cross_product (vector_t* v1, vector_t* v2, vector_t* r)

Cross product of v1 by v2, result into r.


	int vector_cross_product_z (vector_t* v1, vector_t* v2)

Cross product of the z axis of v1 by v2.


### Vector dot product


	int vector_dot_product (vector_t* v1, vector_t* v2)

Dot product of v1 by v2.


### 3D to 2D vector conversion


	void ozplotpointcam(Vector_t *v, Cam_t *c, Point_t *p)


This function will convert 3D vectors (X, Y, Z) into 2D Points (X, Y). This will even compensate for camera's position and angle. If you do not wish to use a camera in your program, use ozplotpoint() instead (see next function.)

Example:
```
...
Vector_t v;
Point_t p;
Cam_t mycam;
...
ozplotpointcam(&v, &mycam, &p);
...
```


### Fast 3D to 2D vector conversion


	void ozplotpoint(Vector_t *v, Point_t *p)

This function converts 3D vectors to 2D points without compensating for camera's position, thus it is much faster.

Example:

```
...
Vector_t v;
Point_t p;
...
ozplotpoint(&v, &p);
...
```








## III. Credits/Acknowledgements

3D Libraries Copyright© 2002, Mark Hamilton Jr. 

New functions coming from GFX - a small graphics library 
Copyright© 2004  Rafael de Oliveira Jannone

Vector rotation optimizations performed by Lawrence Chitty. 

Wonderful Sine and Cosine enhancements done by Alexander Pruss.

Many thanks to on-line tutorials, Alexander Pruss and Benjamin Green for helping me with 3D concepts. I certainly learned a lot from this experience.


Rearranged for z88dk by Stefano Bodrato.

