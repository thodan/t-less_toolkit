# T-LESS DATASET (v2) DOCUMENTATION

Author: Tomas Hodan (hodantom@cmp.felk.cvut.cz)
Center for Machine Perception, Czech Technical University in Prague


## DIRECTORY STRUCTURE
--------------------------------------------------------------------------------
The T-LESS dataset is organized as follows (when downloaded using the script
provided at: http://ptak.felk.cvut.cz/darwin/t-less/t-less_download.py):

* models_{cad,reconst} - 3D object models
* train_{primesense,kinect,canon} - Training images
* test_{primesense,kinect,canon} - Test images


## SENSORS
--------------------------------------------------------------------------------
The training and test images were captured with these sensors:

* primesense - Primesense CARMINE 1.09
* kinect - Microsoft Kinect v2
* canon - Canon IXUS 950 IS

The sensors were synchronized, i.e. the following images were captured at the
same time from almost the same viewpoint:

* {train,test}_primesense/rgb/XXXX.png
* {train,test}_primesense/depth/XXXX.png
* {train,test}_kinect/rgb/XXXX.png
* {train,test}_kinect/depth/XXXX.png
* {train,test}_canon/rgb/XXXX.jpg

Note: Relative poses of the sensors can be calculated from the information
provided in the {obj,scene}_info.yml files (see below).


## 3D OBJECT MODELS
--------------------------------------------------------------------------------
The 3D object models are provided in two variants:

* cad - Manually created CAD models (no color)
* reconst - Models (with color) automatically reconstructed from the training
            RGB-D images (from Primesense) using fastfusion:
            github.com/tum-vision/fastfusion

The models are provided with vertex normals that were calculated using the
MeshLab implementation (meshlab.sourceforge.net) of the following method:
G. Thurrner and C. A. Wuthrich, Computing vertex normals from polygonal facets,
Journal of Graphics Tools 3.1 (1998)


## TRAINING IMAGES
--------------------------------------------------------------------------------
The training images depict an object from a uniformly sampled full view sphere
-- 10 deg step in elevation (from 85 deg to -85 deg) and 5 deg in azimuth.

The training images of each object are accompanied with file obj_info.yml, that
contains for each image the following information:

* cam_K - 3x3 intrinsic camera matrix K (saved row-wise)
* cam_R_m2c - 3x3 rotation matrix R_m2c (saved row-wise)
* cam_t_m2c - 3x1 translation vector t_m2c
* elev - an approximate elevation at which the image was captured
* mode - capturing mode (0 = the object was standing upright, 1 = the object was
         standing upside down)
* obj_bb - 2D bounding box [x, y, width, height], where [x, y] is the top-left
           corner of the bounding box

P_m2c = K * [R_m2c, t_m2c] is the camera matrix which transforms a 3D point x_m
in the model coordinate system to a 3D point x_c in the camera coordinate
system: x_c = P * x_m.

Note: The matrix K is different for each image because the provided images were
obtained by cropping of the captured images (i.e. the principal point is not
constant).


## TEST IMAGES
--------------------------------------------------------------------------------
The test images depict a scene from a uniformly sampled view hemisphere
-- 10 deg step in elevation (from 75 deg to 15 deg) and 5 deg in azimuth.

The test images from each scene are accompanied with file scene_info.yml, that
contains for each image the following information:

* cam_K - 3x3 intrinsic camera matrix K (saved row-wise)
* cam_R_w2c - 3x3 rotation matrix R_w2c (saved row-wise)
* cam_t_w2c - 3x1 translation vector t_w2c
* elev - an approximate elevation at which the image was captured
* mode - capturing mode (always 0 for the test scenes)

P_w2c = K * [R_w2c, t_w2c] is the camera matrix, which transforms a 3D point x_w
in the world coordinate system to a 3D point x_c in the camera coordinate
system: x_c = P * x_w. The world coordinate system is defined by the fiducial
markers (some of them are visible in the provided test images).

Note: The matrix K is different for each image for the same reason as in the
case of training images.

The ground truth poses of the objects which are present in the scene are
provided in file scene_gt.yml, that contains for each image and object the
following information:

* obj_id - Object ID
* cam_R_m2c - 3x3 rotation matrix R_m2c (saved row-wise)
* cam_t_m2c - 3x1 translation vector t_m2c
* obj_bb - 2D bounding box [x, y, width, height], where [x, y] is the top-left
           corner of the bounding box

P_m2c = K * [R_m2c, t_m2c] is the camera matrix which transforms a 3D point x_m
in the model coordinate system to a 3D point x_c in the camera coordinate
system: x_c = P * x_m.


## UNITS
--------------------------------------------------------------------------------
* Depth images: 0.1 mm
* 3D object models: 1 mm
* Translation vectors: 1 mm
