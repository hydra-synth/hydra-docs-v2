---
title: Webcam and video inputs
weight: 5
---

## Using the webcam
In addition to using sources from within hydra (such as `osc()` and `shape()`), you can use hydra to process external video sources such as a webcam, video, or screen capture. To initialize the webcam, run the following code:
```javascript
s0.initCam()
```

This activates the webcam source inside a variable called `s0`, and you should see the light on your webcam light up. However, you will still not see the webcam image on the screen. In order to use the camera within a hydra sketch, you need to use it within the `src()` function. 

```hydra
s0.initCam() //initialize webcam as external source 's0'
src(s0).out() // use external source 's0' inside Hydra
```

Similar to adding transformations above, you can add transformations of color and geometry to the camera output, by adding functions to the chain:

```hydra
s0.initCam()
src(s0).color(-1, 1).out()
```

```hydra
s0.initCam()
src(s0).color(-1, 1).kaleid().out()
```

If you have multiple webcams, you can access separate cameras by adding a number inside `initCam`, for example `s0.initCam(1)` or `s0.initCam(2)`. 


Similar to using the camera as an input, you can also use window or desktop capture, video sources, images, or HTML canvas elements as sources within Hydra. For a complete list of sources, see [reference: external sources.](./../../../reference/api/external-sources/#available-source-functions#available-source-functions)
