# Camera Control with **Python**

## Pros and Cons
Here are some reasons why you might use python to switch between cameras:

* You are using a raspberry pi - python is your best option when using a raspberry pi since running Unity or OBS would be quite challenging, and raspberry pi isn't as performant for displaying websites, so p5js is a worse option
* You are doing something like Machine Learning to control your cameras
* You want to use physical sensors (using the Makey-Makey or Raspberry Pi)

And here are some reasons why you might not want to use python:

* You aren't that comfortable with code
* The tools to build graphic elements aren't as easy to use as with p5js

---

## Installation

 The first step is to install a few of the necessary packages to be able to grab your webcam feed, and to manipulate those video feeds:

 1. opencv (this is to get the webcam feeds)
 2. numpy (to do any kind of math with the image pixels)
 3. [Pillow](https://pillow.readthedocs.io/en/stable/handbook/tutorial.html) (python imaging library which makes it easier to manipulate images in Python)

*Use the following commands to install these three necessary packages*

```bash
pip3 install opencv-python
pip3 install numpy
pip3 install Pillow
```
---

## Import Libraries

```python
import numpy as np
import cv2
from PIL import ImageDraw, Image

```

## Basic Webcam Example


```python
cap = cv2.VideoCapture(0)

while(True):
    ret,frame = cap.read()
    cv2.imshow('window name',frame)
    if (cv2.waitKey(1) & 0xFF) == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()

```
The first line `cap = cv2.VideoCapture(0)` creates a variable `cap` that stores a VideoCapture object. The number `0` is a reference to which webcam device to use. If you wanted to use a different device, you would use a different number.

 To show the video on your screen, you need to grab the frames from the VideoCapture object `cap`, and then display that in a window. `cap.read()` is a function ([documentation](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_gui/py_video_display/py_video_display.html)) that "returns a bool (True/False). If frame is read correctly, it will be true." The second variable returned will be the next frame of the video.

 To display the static frame we've just grabbed, we are using the `imshow('window name',frame)` line of code. The first parameter is the name of the window, and the second is the image we want to show.

 The final if-statement within the code allows us to get out of the while loop if the letter q is pressed. `cv2.waitKey(1)` waits for 1 millisecond, returning the code of the pressed key. It returns the keycode as a 32-bit integer, so by doing an AND operation with `0xFF`, we convert it to an 8-bit number that can be compared to `ord('q')`. For more on this, read this [StackOverflow post](https://stackoverflow.com/questions/35372700/whats-0xff-for-in-cv2-waitkey1#:~:text=waitKey(0)%20is%20bitwise%20AND,eqality%20with%20ord(c).&text=cv2.,-waitKey()%20is&text=output%20from%20waitKey%20is%20then,8%20bits%20can%20be%20accessed).

Finally, to finish off closing the window, we release the VideoCapture object and then use `cv2.destroyAllWindows()` to close all open OpenCV windows.


## Multiple Cameras

Using multiple cameras is pretty simple. Just create a new VideoCapture object for each camera you would like to use.

```python
webcam1 = cv2.VideoCapture(0)
webcam2 = cv2.VideoCapture(1)
...

```
You can choose which camera feed to display by just choosing to read from a different VideoCapture object.

```python
cameras = [webcam1,webcam2]
cameraIndex = 0

while(True):
    ret,frame = cameras[cameraIndex].read()

    cv2.imshow('window name',frame)

    key = cv2.waitKey(1) & 0xFF
    if key == ord('q'):
        break
    elif key == ord('1'):
        cameraIndex = 1
    elif key == ord('0'):
        cameraIndex = 0

```
In this example, I've stored each of the webcams in an array. I then have a second variable, `cameraIndex = 0` to track the currently selected webcam. To switch cameras, I modified the if-statement for closing to also check if you've pressed 1 or 0. By updating the `cameraIndex` appropriately, you can now change the camera we are using.

```python
webcam1.release()
webcam2.release()
cv2.destroyAllWindows()

```
Make sure to release all the cameras you've created before closing.

## Frame/Image Manipulation

To show the video feed to our screens, we are grabbing static images and displaying it to a window. That means that you can modify the image in any way you want before displaying it to the screen.

For example, to display a grayscale video feed:

```python
cap = cv2.VideoCapture(0)

while(True):
    ret,frame = cap.read()

    # Do some image manipulation here
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    cv2.imshow('gray window',gray)
    if (cv2.waitKey(1) & 0xFF) == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()

```
In this case, we are using OpenCV's built-in function to [change the colorspace](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_colorspaces/py_colorspaces.html#converting-colorspaces) of the image. The default colorspace is BGR (Blue, Green, Red), so we use the `cvtColor()` function to convert the image, using the openCV constant `COLOR_BGR2GRAY` to convert the image to grayscale.

While OpenCV has many functions to do image processing (see documentation here), its often simpler to use the PIL/Pillow library to do image manipulation.

Start by converting the frame to a PIL object. To do this, you have to convert the colorspace from BGR to RGB to be compatible with PIL

```python
cv2_im_rgb = cv2.cvtColor(frame,cv2.COLOR_BGR2RGB)
pil_im = Image.fromarray(cv2_im_rgb)
```

### Drawing Shapes and Text

Once you have the PIL image, you can pretty easily do some basic image operations, which you can find more details about in the [documentation](https://pillow.readthedocs.io/en/3.1.x/reference/ImageDraw.html).

```python

# Resizing an image (doesn't maintain aspect ratio)
resized_image = pil_im.resize((960,720))

# Draw shapes ontop of an image
draw = ImageDraw.Draw(pil_im)
draw.rectangle([0,0,400,400],fill=(0,0,255),outline=(0,0,0))
draw.line([0,0,100,100],fill=(0,0,255),width=3)

# Draw text ontop of an image
font = ImageFont.truetype("SpaceMono-Regular.ttf",24)
draw.text((100,100),"TEXT",font=font,fill=(0,0,255))

```
### Combining static graphics
Another useful thing to do with PIL is to "[paste](https://pillow.readthedocs.io/en/stable/reference/Image.html#PIL.Image.Image.paste)" images together. You can create static graphics and overlay it ontop of your webcam feed.

```python
graphic =  Image.open("graphic.png")

# Coordinates define the upper left corner
pil_im.paste(graphic,(60,0))

```


## Fullscreen Window

To get the window to take up the whole screen use the following code before the line `cv2.imshow('frame',frame)`.

```python
cap = cv2.VideoCapture(0)
cv2.namedWindow("frame", cv2.WND_PROP_FULLSCREEN)
cv2.setWindowProperty("frame",cv2.WND_PROP_FULLSCREEN,cv2.WINDOW_FULLSCREEN)
```
Make sure to be consistent with your frame name (in this case it's "frame").

 *Note that while this makes the window take up the full screen, if the image is not big enough, it will not be scaled to fit the window. You'll have to resize your frames itself if you want it to take up the whole screen*

 ## Basic Multi-Threading

 So far, the only way we've talked about triggering a camera change is by using `cv2.waitKey(1)`, which constrains you to using keyboard keys. Using something like the Makey-Makey, which acts like an external keyboard, can allow you to be creative under those constraints.

 However, if you want the camera to be triggered by something else, like a machine learning algorithm, your going to need to run that machine learning algorithm or the camera code on a seperate thread. We'll be using the default python `threading` library.

 ```python
import threading
...
cameraIndex = 0

 def main():
    while True:
        # looping code that updates
        # cameraIndex variable

# Threading code
thread = threading.Thread(target=main)
thread.daemon = True
thread.start()

while True:
    ret,frame = cap.read()
    ...

```
In the first line of the threading code we set `target=main`, which is a reference to the function that we want to run in a seperate thread. For more detailed information on threads in python, take a look at this [guide](https://realpython.com/intro-to-python-threading/).


## Simulating Keystrokes with Python
Here is a [guide](https://nitratine.net/blog/post/simulate-keypresses-in-python/) for more detailed code on this topic

An easy way to integrate some more complex logic into your opencv controlled webcams or even your OBS based camera control system is to use python to simulate keystrokes. For example, you could setup Hotkeys in OBS and then trigger those Hotkeys from a python script automatically.

First install the library pynput `pip3 install pynput`

Basic usage:

```python
from pynput.keyboard import Key, Controller

keyboard = Controller()
keyboard.press('q')
keyboard.release('q')

```
Now, you can setup simple conditions in python to control scenes you've setup in OBS or just trigger scene changes in another python script you're running
