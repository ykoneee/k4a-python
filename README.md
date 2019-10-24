# k4a-python
This library is a wrapper in Python  for Azure-Kinect-Sensor-SDK

# Prereqs
* install [Azure Kinect SDK](https://github.com/microsoft/Azure-Kinect-Sensor-SDK) first
* install pybind11 by pip, ```pip install pybind11```

# Install
## Linux
```
pip install k4a-python
```
pip install only have test on ubuntu 18.04, but it should aslo work well with other linux distributions.

## Windows
pip on windows has not been tested, will test as soon.

# TODO
* IMU data
* Microphone data
* other official c++ API
* other official modules, like k4arecord
* [official example](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples)
# Usage
The Python API mainly refers to the [official C + + API](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group__cppsdk.html)

example code :
```
import pyk4a
import cv2

print(pyk4a.Device.get_installed_count())
device=pyk4a.Device.open(pyk4a.K4A_DEVICE_DEFAULT)

config = pyk4a.Configuration()
config.depth_mode = pyk4a.K4A_DEPTH_MODE_NFOV_2X2BINNED
config.color_resolution = pyk4a.K4A_COLOR_RESOLUTION_720P
config.camera_fps = pyk4a.K4A_FRAMES_PER_SECOND_30
config.color_format = pyk4a.K4A_IMAGE_FORMAT_COLOR_BGRA32
config.synchronized_images_only = True

calibration = device.get_calibration(config.depth_mode, config.color_resolution)
transformation = pyk4a.Transformation(calibration)
device.start_cameras(config)
capture=pyk4a.Capture(transformation,config)
#if you don't want to use image transform and colorize function,you can use
#capture=pyk4a.Capture()
a=200
while a>0:
    a-=1
    print(a)

    if device.get_capture(capture,1000):

        img=capture.get_ir_image()
        #img=capture.get_color_image()
        #img=capture.get_depth_image()
        #we can set flag to get colored and transformed image,like
        #img=capture.get_depth_image(colorize=True,transform=True)
        #type(img)==np.adarray
        cv2.imshow('t',img)
        cv2.waitKey(1)
```
