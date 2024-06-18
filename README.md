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


# 3.This loop iterates over each filename (cl) in myList:

for cl in myList:
    curImg = cv2.imread(f'{path}/{cl}')             #reads an image file from the 'ImageAttendace' directory using its filename (cl) and stores it in the variable 'curImg'.
    images.append(curImg)                           #appends the current image (curImg) to the 'images' list.
    print(len(images))                              #prints the current length of the images list, which represents the number of 'images' processed so far.
    classNames.append(os.path.splitext(cl)[0])      #removes the file extension from the filename (cl) using 'os.path.splitext' and appends the resulting name to the 'classNames' list. 
                                                     This list will contain the class names or labels for each image.
print(classNames)                                   #This line prints the contents of the 'classNames' list
