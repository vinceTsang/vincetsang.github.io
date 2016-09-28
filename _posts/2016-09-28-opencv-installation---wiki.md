---
layout: post
title:  "opencv installation on Ubuntu 16.04/14.04"
date:   2016-09-28 12:11:06
isWiki: yes
---
(py version is 2.7.12)

**Step 1: install the dependencies**

sudo apt-get -y install libopencv-dev build-essential cmake git libgtk2.0-dev pkg-config python-dev python-numpy libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libtiff4-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libxine-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libv4l-dev libtbb-dev libqt4-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 v4l-utils unzip
---

**Step 2: Download and extract OpenCV**

choose version 2.4.9 rather than 3.0.0

wget -O OpenCV-2.4.9.zip http://fossies.org/linux/misc/opencv-2.4.9.zip

unzip OpenCV-2.4.9.zip

cd opencv-2.4.9
---

**Step 3: Install**

mkdir build

cd build

cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D WITH_V4L=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_QT=ON -D WITH_OPENGL=ON ..

make -j2

sudo make install

sudo sh -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf'

sudo ldconfig

**Step 4: reboot and test**

## test.py

import cv2
import numpy as np

img = cv2.imread('/home/vincent/wks/myPic.png',cv2.IMREAD_COLOR)
gray = cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
cv2.imshow('origin',img);

# SIFT
detector= cv2.SIFT()
keypoints = detector.detect(gray, None)
img = cv2.drawKeypoints(gray,keypoints)
cv2.imshow('test',img);
cv2.waitKey(0)
cv2.destroyAllWindows()

