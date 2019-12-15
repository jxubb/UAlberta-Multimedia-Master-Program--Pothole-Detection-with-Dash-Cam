# UAlberta-Multimedia-Master-Program---Pothole-Detection-with-Dash-Cam
This is a course project for UAlberta Multimedia Master Program. The main purpose of this project is to achieve pothole detection with tiny YOLOv3. 

### [YOLO: Real-Time Object Detection](https://pjreddie.com/darknet/yolo/)

This project is modofied based on [AlexeyDB's fork](https://github.com/AlexeyAB/darknet) of Darknet tiny Yolov3 that runs on Windows and Linux.
Before implementing pothole detection with this repository, please follow the instruction of [AlexeyDB's fork](https://github.com/AlexeyAB/darknet). Custom object detection tutorial can also be found in [How to train](https://github.com/AlexeyAB/darknet#how-to-train-to-detect-your-custom-objects) in AlexeyDB's README file. 
The modified files can be found in [this Github repository](https://github.com/jxubb/UAlberta-Multimedia-Master-Program--Pothole-Detection-with-Dash-Cam).

# Contents
* [Data set](#Data-set)
* [Set up](#Set-up)
* [Running the project](#Running-the-project)
* [Results](#Results)

# Data set

Our  ”Edmonton  pothole”  data  set  is  composed  of  325labelled  images.  We  took  25  Edmonton  pothole  videos  withdashcam around the University of Alberta during daylight. Thevideos add up to about 25 minutes and can be decomposed intoapproximately  45000  images. You can download the dataset [here](https://drive.google.com/drive/folders/1wWwiGBUo0C_ElA8B_Z0iha_uVJyWKgQX).


# Set up
* Environment: Windows10, CUDA10.0, Cudnn7.6, OpenCV3.4.1 .

* Other requirementts can be find [here](https://github.com/AlexeyAB/darknet#requirements).

* Follow [AlexeyDB's fork](https://github.com/AlexeyAB/darknet) to compile darknet.

* Then, implement the following steps to modify tiny YOLOv3 to  detect custom object:

 1. Create file `yolov3-tiny-obj.cfg` with the same content as in `yolov3-tiny.cfg`

  * change line batch to [`batch=64`](https://github.com/AlexeyAB/darknet/blob/0039fd26786ab5f71d5af725fc18b3f521e7acfd/cfg/yolov3.cfg#L3)
  * change line subdivisions to [`subdivisions=8`](https://github.com/AlexeyAB/darknet/blob/0039fd26786ab5f71d5af725fc18b3f521e7acfd/cfg/yolov3.cfg#L4)
  * change line max_batches to `4000`, f.e. [`max_batches=4000`](https://github.com/AlexeyAB/darknet/blob/0039fd26786ab5f71d5af725fc18b3f521e7acfd/cfg/yolov3.cfg#L20) if you train for 3 classes
  * change line steps to 80% and 90% of max_batches, f.e. [`steps=3200,3600`](https://github.com/AlexeyAB/darknet/blob/0039fd26786ab5f71d5af725fc18b3f521e7acfd/cfg/yolov3.cfg#L22)
  * change line `classes=80` to `1` in each of 2 `[yolo]`-layers:
      * https://github.com/AlexeyAB/darknet/blob/master/cfg/yolov3-tiny.cfg#L135  
      * https://github.com/AlexeyAB/darknet/blob/master/cfg/yolov3-tiny.cfg#L177
  * change [`filters=255`] to filters=18 in the 2 `[convolutional]` before each `[yolo]` layer
      * https://github.com/AlexeyAB/darknet/blob/master/cfg/yolov3-tiny.cfg#L127
      * https://github.com/AlexeyAB/darknet/blob/master/cfg/yolov3-tiny.cfg#L171
      
2. Create file `obj.names` in the directory `build\darknet\x64\data\`, with objects names "pothole" 

3. Create file `obj.data` in the directory `build\darknet\x64\data\`, containing

  ```
  classes= 1
  train  = data/train.txt
  valid  = data/test.txt
  names = data/obj.names
  backup = backup/
  ```

4. Put image-files (.jpg) of your objects in the directory `build\darknet\x64\data\obj\`
      
# Running the project

Train with this command line: `darknet.exe detector train data/obj.data yolov3-tiny-obj.cfg yolov3-tiny.conv.15`

Or just train with `-map` flag: `darknet.exe detector train data/obj.data yolov3-tiny-obj.cfg yolov3-tiny.conv.15 -map`

Test with this command line:   `darknet.exe detector test data/obj.data yolov3-tiny-obj.cfg <training_config>_last.weights`


# Results

## Images
|||
|-|-|
|[![Image_1](/test_im_n_vid/229.jpg)](https://github.com/jxubb/UAlberta-Multimedia-Master-Program--Pothole-Detection-with-Dash-Cam/blob/master/test_im_n_vid/229.jpg)
|[![Image_2](/test_im_n_vid/229_result.png)](https://github.com/jxubb/UAlberta-Multimedia-Master-Program--Pothole-Detection-with-Dash-Cam/blob/master/test_im_n_vid/229_result.png)
|[![Image_3](/test_im_n_vid/229_result.png)](https://github.com/jxubb/UAlberta-Multimedia-Master-Program--Pothole-Detection-with-Dash-Cam/blob/master/test_im_n_vid/235.jpg)
|[![Image_4](/test_im_n_vid/235_result.jpg)](https://github.com/jxubb/UAlberta-Multimedia-Master-Program--Pothole-Detection-with-Dash-Cam/blob/master/test_im_n_vid/235_result.png)
## Videos

[![tiny Yolo v3](http://img.youtube.com/vi/JAY3CWlcmcM/0.jpg)](https://www.youtube.com/watch?v=JAY3CWlcmcM "tiny Yolo v3")
