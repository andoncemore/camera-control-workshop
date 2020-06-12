# Camera Control Workshop

There are multiple approaches to control, modify and switch between camera feeds based on different sensors. The appropriate choice for your project depends on the sensors you want to use, how your project is going to be deployed, and how comfortable you are with code.

1. **[OBS](camera-control-obs.md):** OBS (Open Broadcast System) is an open source software most commonly used for live streaming or desktop recording. However, it can be easily used as a simple, non-code system to control different "scenes" using assigned hotkeys. Using something like the [Makey Makey](https://makeymakey.com/), you could trigger those hotkeys via external sensors.
2. **[python3](camera-control-python.md):** The OpenCV python3 libraries give you a lot of flexibility to control camera feeds. If used on a raspberry pi, then it is also convienient to incorporate a sensor into your system. However, this does require some knowledge of python, and the documentation is a little less accessible to beginners (in comparison to p5js)
3. **p5js:** p5js makes it really easy to play with and control webcams, and there is a lot of beginner friendly documentation on the web. The advantage of using something like p5js is that there is plenty of documentation, and it has much more features for modifying the resulting image. A basic example can be found [here](https://p5js.org/examples/dom-video-capture.html) and for much more detailed instructions to work with video in p5js, check out Alison Parrish's tutorial [here](https://creative-coding.decontextualize.com/video/)
4. **Unity:** Another option for controlling webcams is Unity, which has simple to use plugins to incorporate webcam feeds into your scene. You should use Unity if you are using an VR/AR equipment, need 3D scenes, or are especially comfortable working within Unity. I personally don't have much expertise with Unity, so you'll have to find your own resources for integrating a Webcam into your unity scene.


*Updated 6/10/2020*
