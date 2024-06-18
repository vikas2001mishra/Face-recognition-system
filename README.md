# Face-recognition-system
Sure, let me explain the code step by step:

# Attendance.py:-

# 1.Imports:

import cv2                 #OpenCV library for image and video processing.
import numpy as np         #NumPy library for numerical operations.
import face_recognition    #Library for face recognition tasks.
import os                  #'os' is a built-in Python module for interacting with the operating system.


# 2.Loading Images and Preparing Class Names:

path = 'ImageAttendace'         #Directory path where images are stored.
images = []                     #creates an empty list to store the images.
classNames = []                 #creates an empty list to store the class names (or labels) of the images.
myList = os.listdir(path)       #lists all the files and directories in the specified path and stores them in the 'myList' variable.
print(myList)                   #prints the contents of 'myList'.


