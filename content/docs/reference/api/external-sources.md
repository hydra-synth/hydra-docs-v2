---
title: "External sources"
draft: false
weight: 4
---

# External Sources
<!-- by [geikha](https://github.com/geikha) and [olivia](https://ojack.xyz) -->
## Using the webcam
In addition to using sources from within hydra (such as `osc()` and `shape()`), you can use hydra to process external video sources such as a webcam. External sources in hydra are referenced using predefined objects `s0`, `s1`, `s2`, and `s3`.  To initialize the webcam in `s0`, run the following code:
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



---
## Available source functions

### initCam()

You can use a webcam's video as such:

```javascript
s0.initCam()

s0.initCam(2) // if you have many cameras, you can select one specifically
```



---

### initImage()

Load an image into a source object:

```javascript
// load an image into a source object
s0.initImage("https://upload.wikimedia.org/wikipedia/commons/2/25/Hydra-Foto.jpg")

// show the image on the screen
src(s0).out()
```

When running Hydra in Atom, or any other local manner, you can load local files referring to them by URI:

```javascript
s0.initImage("file:///home/user/Images/image.png")
```

### Supported image formats

You can load `.jpeg`, `.png`, and `.bmp` as well as `.gif` and `.webp` (although animation won't work).

### initVideo()

The syntax for loading video is the same as for loading image, only changing the function to `loadVideo`:

```javascript
s0.initVideo("https://media.giphy.com/media/AS9LIFttYzkc0/giphy.mp4")
src(s0).out()
```

### Supported video formats

You can load `.mp4`, `.ogg` and `.webm` videos.

{{< youtube bavUH1cv_v0 >}}

### Useful HTML Video properties

You can access all of the [HTML Video](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video) functions when a video is loaded to a Source via `s0.src`. Some useful properties are:

```javascript
s0.src.playbackRate = 2 // double the speed at which the video plays
s0.src.currentTime = 10 // seek to the 10th second
s0.src.loop = false // don't loop the video
```

#### initScreen()

You can capture your screen or specific windows or tabs to use as a video source:

```javascript
s0.initScreen()
src(s0).out()
```
---

{{< youtube mPH8hpbEs3o >}}

### init()
`init()` is a more generic function for loading any external source into hydra. This can be especially useful when you are using an [HTML canvas element](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) as an input, or loading an existing resource as a source into hydra. Valid input types are documented in the [regl texture documentation](http://regl.party/api#textures).

For example, the following code creates a canvas element and draws text to it, and then uses that canvas as a source in hydra:
```hydra
myCanvas = document.createElement('canvas')
ctx = myCanvas.getContext('2d')
ctx.font = "30px Arial"
ctx.fillStyle = "red";
ctx.fillText("Hello World", 10, 50)   

s0.init({ src: myCanvas, dynamic: false })

src(s0).diff(osc(2, 0.1, 1.2)).out()
```
use the `dynamic` parameter to indicate whether the source will be updated, or remain the same. 

### initStream()
{{< hint danger >}}
note: initStream() is currently broken in hydra editor due to server issues
{{< /hint >}}

streaming between Hydra sessions

Hydra (the editor) also has built-in streaming. You can stream the output of your Hydra to someone else and vice-versa. This is done in a similar fashion to using images and videos, using external sources. But there are some extra steps for streaming:

####  The pb object

On your Hydra editor, you can find a pre-defined object called `pb` (as in patch-bay). This object basically represents the connection of your Hydra editor instance to all others hosted on the same server. When you want to share your stream to someone else you'll have to give your Hydra session a name. Do this using the `pb.setName()` function and by passing in some string as the name. For example: `pb.setName('myverycoolsession')`. If you want someone else to stream to you, ask them to set a name as such and share it with you.

You can see online sessions using the function `pb.list()`, which will return an Array of names.

#### Starting to stream

Streaming is as simple as initiating the source as a stream and passing the name of the session you want to stream. For example:

```javascript
s0.initStream('myfriendsverycoolsession')
src(s0)
    .out()
```

---

### Extra parameters

Any external sources loaded into Hydra are using [regl's texture constructor](https://github.com/regl-project/regl/blob/master/API.md#textures) in the background. There are many properties you can set when loading a texture and Hydra and regl handle the important ones for you. But to set any of these properties you can pass an object containing them to any of the init functions. For example:

```javascript
s0.initCam(0,{mag: 'linear'})
```

`mag` & `min` are the most used, since using `linear` interpolation will resize textures in a smooth way. The default for both is `nearest`. 

---

## Common problems

### CORS policy

If you try to load images (or videos) from some websites (most of them, really), sometimes nothing shows up on the screen. Opening the browser's console might reveal a message similar to this one:

```
Access to image at '...' from origin 'https://hydra.ojack.xyz' 
has been blocked by CORS policy: 
No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

The CORS in CORS policy stands for 'Cross-origin resource sharing'. This refers to the action of calling resources (such as images) from one website to another. For example, asking for an image hosted on other website from inside the Hydra editor. This error message is basically telling us "hey, the website you're trying to ask for an image doesn't allow other websites to use their resources, so i can't let you have that picture".
In order to circumvent this error, you can try re-uploading the images you want to use to some image hosting service that allows cross-origin sharing such as imgur, where you can also load short videos. You can also try to use websites which you know will allow cross-origin resource sharing such as [Wikimedia Commons](https://commons.wikimedia.org/), which is great for video.

### Loading video from YouTube, Vimeo, etc

Some users may be tempted to try and load some video they liked on YouTube, for example, and run something suchlike:

```javascript
s0.initVideo('https://www.youtube.com/watch?v=dQw4w9WgXcQ') // doesn't work
```

This will not work. The same goes for Vimeo and other video streaming services. When you use such an URL, it is not returning a video, it is returning the website where you can watch the video! The URL you pass to `initVideo` has to go directly to a video file. In other words, the URL should (usually) end in `.mp4`, `.webm` or `.ogg`. And, even if you did get a URL directly to the video with a tool such as youtube-dl, you'll run into CORS problems.

#### Workaround

The most common workarounds are:

* Run Hydra locally (on Atom for example) and load local video files
* Have the video run on its own window and use `initScreen` to capture it
