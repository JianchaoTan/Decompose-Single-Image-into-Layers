# Decomposing Images into Layers via RGB-space Geometry

This code implements the pipeline described in the paper ["Decomposing Images into Layers via RGB-space Geometry"](http://cs.gmu.edu/~ygingold/singleimage/) by Jianchao Tan, Jyh-Ming Lien, and Yotam Gingold in [ACM Transactions on Graphics (TOG)](http://tog.acm.org).

The pipeline is divided into two steps.

### 1. Convex Hull Simplification

Input:

* Source image

Output:

* Simplified Hull Vertices (color palettes) for multiple simplification levels (4 to 10 vertices)

Users can then choose what simplification level (number of vertices) is reasonable. They can visualize the output by dragging-and-dropping it onto [our web GUI](http://yig.github.io/image-rgb-in-3D/).


### 2. Layer Extraction

Input: 

* Source image
* Vertex order (users choose which they like)
* Simplified convex hull vertices (the color palettes)
* Optimization weights


Output:

* Translucent layers (PNG's)
* Barycentric Coordinate weights (a JSON file and also PNG's of each weight map for visualization)

Users can perform global recoloring in [our web GUI](http://yig.github.io/image-rgb-in-3D/). First load the original image, then drag-and-drop the convex hull `.js` file, and finally drag-and-drop the Barycentric Coordinates weights `.js` file. For a more detailed usage guide to the web GUI, please see the [supplemental materials](http://cs.gmu.edu/~ygingold/singleimage/) of our paper.

We provide two choices for this layer extraction step:

* Our main global optimization-based approach, including RGB and RGBA versions. The RGB optimization assumes that the first layer is opaque, which means its color is the background color. The RGBA version is appropriate when there is no obvious background color, such as a photograph.
* Our As-Sparse-As-Possible (ASAP) approach, which is independent per-pixel and therefore much faster but generates noisier results.

## Example usage

Convex Hull Simplification:

    cd results
    python ../ConvexHull_Simplification/SILD_convexhull_simplification.py apple

Layer extraction (RGB):

    cd results
    python ../Layer_Extraction/SILD_RGB.py apple.png apple-06-vertex_order.js apple-final_simplified_hull_clip-06.js apple-06-layers-RGB --weights weights.js

Layer extraction (RGBA): (Note that `apple` has an opaque background, so it is not a good example for RGBA layer extraction.)

    cd results
    python ../Layer_Extraction/SILD_RGBA.py apple.png apple-06-vertex_order.js apple-final_simplified_hull_clip-06.js apple-06-layers-RGBA --weights weights.js

Layer extraction (ASAP) (all arguments are in the JSON parameter file):

    cd results
    python ../Layer_Extraction/SILD_ASAP.py apple-06-ASAP.js

## Dependencies
* NumPy
* SciPy
* cvxopt (as an interface to the [GLPK](https://www.gnu.org/software/glpk/) linear programming solver)
* [GLPK](https://www.gnu.org/software/glpk/) (`brew install glpk`)
* PIL or Pillow (Python Image Library): `pip install Pillow`
