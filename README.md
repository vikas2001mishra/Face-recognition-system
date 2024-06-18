# Face-recognition-system
Sure, let me explain the code step by step:

# main.py:-

# 1.Imports:

import cv2                    #OpenCV library for image processing.
import numpy as np            #NumPy library for numerical operations.
import face_recognition       #Library for face recognition tasks.



# 2.Loading and Preprocessing Images:

imgowner = face_recognition.load_image_file('ImageAttendace/Vikas.jpg')        # Loads an image ('Vikas.jpg') of the owner's face using 
imgowner = cv2.cvtColor(imgowner, cv2.COLOR_BGR2RGB)                           #converts it from BGR to RGB color format using 'cv2.cvtColor()'.

imgtester = face_recognition.load_image_file('ImageAttendace/image.png')       #Loads another image ('image.png') for testing, performs the same color conversion.
imgtester = cv2.cvtColor(imgtester, cv2.COLOR_BGR2RGB)


# 3.Face Detection and Encoding:

faceLoc = face_recognition.face_locations(imgowner)[0]              #detects the locations of faces in 'the imgowner' image and returns a list of bounding boxes (coordinates) for each 
                                                                     detected face.
encodeowner = face_recognition.face_encodings(imgowner)[0]          #[0] selects the first (and in this case, the only) face location from the list and assigns it to the variable 
                                                                     'faceLoc'.
cv2.rectangle(imgowner, (faceLoc[3], faceLoc[0]), (faceLoc[1], faceLoc[2]), (255, 0, 255), 2)     #Draws a rectangle around the detected face using the coordinates provided by faceLoc.

faceLoctester = face_recognition.face_locations(imgtester)[0]           #Detect the face location in 'imgtester' and store it in 'faceLoctester'.
encodetester = face_recognition.face_encodings(imgtester)[0]            #Compute the face encoding for the detected face in 'imgtester' and store it in 'encodetester'.
cv2.rectangle(imgtester, (faceLoc[3], faceLoc[0]), (faceLoc[1], faceLoc[2]), (255, 0, 255), 2)   #Draw a rectangle around the detected face


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



# 4.Encoding Faces:

def findEncoding(images):                                     #Defines a function to encode faces from images.
    encodeList = []                                           #creates an empty list to store the face encodings.

    for img in images:
        img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)           #converts the color space of the image from BGR (OpenCV's default color order) to RGB.
        encode = face_recognition.face_encodings(img)[0]     #Extracts face encodings for each image and appends them to 'encodeList'.
        encodeList.append(encode)                            #appends the face encoding
        print(len(encodeList))                               #prints the current length of the 'encodeList'.
        # print(classNames[img]) 
        print(encodeList)                                    #prints the contents of the 'encodeList'.
        return encodeList                                    #The function returns 'encodeList', which contains face encodings for all images.

encodeListKnow = findEncoding(images)                        #'findEncoding(images)' calls the findEncoding function with the images list as input and stores the resulting list of face 
                                                               encodings in the variable encodeListKnow.
print('Encoding Done ')                                      #prints a message indicating that the encoding process is complete.
print(len(encodeListKnow))                                   #prints the length of the 'encodeListKnow' list.
print(encodeListKnow)                                        #prints the contents of the 'encodeListKnow' list.



# 5.Face Recognition using Webcam:

cap = cv2.VideoCapture(0)                        #Opens the default webcam for capturing video.
while True:                                      #Runs an infinite loop to continuously process frames from the webcam.
    success, img = cap.read()                    #reads a frame from the video capture and stores it in the img variable.
    imgS = cv2.resize(img, (0, 0), None, 0.25, 0.25)     #resizes the frame to 25% of its original size for faster processing.
    imgS = cv2.cvtColor(imgS, cv2.COLOR_BGR2RGB)         #converts the color space of the resized frame from BGR to RGB.

    faceCurFrame = face_recognition.face_locations(imgS)                     #Detects face locations in the resized frame.
    encodeCurFrame = face_recognition.face_encodings(imgS, faceCurFrame)     #Computes face encodings for the detected faces in the resized frame.

    for encodeFace, faceLoc in zip(encodeCurFrame, faceCurFrame):                 #The loop iterates over each face encoding (encodeFace) and its corresponding face location (faceLoc) 
                                                                                    using the 'zip' function.

        matches = face_recognition.compare_faces(encodeListKnow, encodeFace)       #compares the current face encoding (encodeFace) with the known face encodings (encodeListKnow) and 
                                                                                     stores the boolean results in the matches variable.

        faceDis = face_recognition.face_distance(encodeListKnow, encodeFace)           #If a match is found (matches[matchIndex] is True), it retrieves the corresponding name 
                                                                                        (classNames[matchIndex]) and draws a rectangle around the face with the name displayed.

        print(faceDis)

        matchIndex = np.argmin(faceDis)

        if matches[matchIndex]:
            name = classNames[matchIndex].upper()
            y1, x2, y2, x1 = faceLoc
            y1, x2, y2, x1 = y1 * 4, x2 * 4, y2 * 4, x1 * 4
            cv2.rectangle(img, (x1, y1), (x2, y2), (0, 255, 0), 2)
            cv2.rectangle(img, (x1, y2 - 35), (x2, y2), (0, 255, 0), cv2.FILLED)
            cv2.putText(img, name, (x1 + 6, y2 - 6), cv2.FONT_HERSHEY_COMPLEX, 1, (255, 255, 255), 2)

    cv2.imshow('WebCam', img)               #Displays the webcam feed with recognized faces and names.
    cv2.waitKey(1)                          #Waits for a key press (1 millisecond) to exit the loop and close the window.
