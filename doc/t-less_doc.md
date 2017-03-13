## T-LESS dataset (v2) documentation

Author: Tomas Hodan (hodantom@cmp.felk.cvut.cz), Center for Machine Perception,
Czech Technical University in Prague


### Directory structure

The T-LESS dataset is organized as follows (when downloaded using
t-less\_download.py):

* **models\_{cad,reconst}** - 3D object models.
* **train\_{primesense,kinect,canon}/YY** - Training images of object YY.
* **test\_{primesense,kinect,canon}/ZZ** - Test images of scene ZZ.


### Sensors

The training and test images were captured with three sensors:

* **primesense** - Primesense CARMINE 1.09.
* **kinect** - Microsoft Kinect v2.
* **canon** - Canon IXUS 950 IS.

The sensors were synchronized, i.e. the following images were captured at the
same time from almost the same viewpoint:

* {train,test}\_primesense/YY/rgb/XXXX.png
* {train,test}\_primesense/YY/depth/XXXX.png
* {train,test}\_kinect/YY/rgb/XXXX.png
* {train,test}\_kinect/YY/depth/XXXX.png
* {train,test}\_canon/YY/rgb/XXXX.jpg


### 3D object models

The 3D object models are provided in two variants:

* **cad** - Manually created CAD models (no color).
* **reconst** - Models (with color) automatically reconstructed from the
            training RGB-D images (from Primesense) using fastfusion:
            http://github.com/tum-vision/fastfusion/

The models are provided with vertex normals that were calculated using the
MeshLab implementation (http://meshlab.sourceforge.net/) of the following
method: G. Thurrner and C. A. Wuthrich, Computing vertex normals from polygonal
facets, Journal of Graphics Tools 3.1 (1998).


### Training and test images

The training images depict an object from a systematically sampled full view
sphere with 10 deg step in elevation (from 85 deg to -85 deg) and 5 deg in
azimuth. There are 1296 training images per object from each sensor (only 648
training images for objects 19 and 20 because they look the same from the upper
and the lower view hemisphere).

The test images depict a scene from a systematically sampled view hemisphere
with 10 deg step in elevation (from 75 deg to 15 deg) and 5 deg in azimuth.
There are 504 images per scene from each sensor.

Each set of training and test images is accompanied with file info.yml that
contains for each image the following information:

* **cam\_K** - 3x3 intrinsic camera matrix K, saved row-wise.
* **cam\_R\_w2c** - 3x3 rotation matrix R\_w2c, saved row-wise.
* **cam\_t\_w2c** - 3x1 translation vector t\_w2c.
* **elev** - Approximate elevation at which the image was captured.
* **mode** - Capturing mode (for training images: 0 = the object was standing
    upright, 1 = the object was standing upside down, for test images:
    always 0).

The matrix K is different for each image -- the principal point is not constant
because the provided images were obtained by cropping the region around the
origin of the world coordinate system (i.e. the center of the turntable)
in the captured images.

P\_w2c = K * [R\_w2c, t\_w2c] is the camera matrix which transforms 3D point
x\_w in the world coordinate system to 3D point x\_c in the camera coordinate
system: x\_c = P * x\_w. The world coordinate system is defined by the fiducial
markers (some of them are visible in the test images). R\_w2c and t\_w2c are
provided only for the test images.

The ground truth object poses are provided in files gt.yml that contain for each
object in each image the following information:

* **obj\_id** - Object ID.
* **cam\_R\_m2c** - 3x3 rotation matrix R\_m2c, saved row-wise.
* **cam\_t\_m2c** - 3x1 translation vector t\_m2c.
* **obj\_bb** - 2D bounding box of projection of the 3D CAD model at the ground
    truth pose. It is given by (x, y, width, height), where (x, y) is the
    top-left corner of the bounding box. 

P\_m2c = K * [R\_m2c, t\_m2c] is the camera matrix which transforms 3D point
x\_m in the model coordinate system to 3D point x\_c in the camera coordinate
system: x\_c = P * x\_m.


### Camera parameters

The intrinsic camera parameters differ for each image (see above).

Parameters for the original images (before cropping) can be found here:
https://github.com/thodan/t-less_toolkit/tree/master/cam

Note that for the Canon camera there are two sets of parameters, each for
different zoom level:

* **Zoom level 3** - Used to capture all training images and to capture test
    images from scene 14 for elevation <= 45 degrees.
* **Zoom level 1** - Used to capture the rest of test images.


### Units

* Depth images: 0.1 mm
* 3D object models: 1 mm
* Translation vectors: 1 mm


### More information

More information about the dataset can be found on its websites:

http://cmp.felk.cvut.cz/t-less/

or in this publication:

T. Hodan, P. Haluza, S. Obdrzalek, J. Matas, M. Lourakis, X. Zabulis,
T-LESS: An RGB-D Dataset for 6D Pose Estimation of Texture-less Objects,
IEEE Winter Conference on Applications of Computer Vision (WACV), 2017
