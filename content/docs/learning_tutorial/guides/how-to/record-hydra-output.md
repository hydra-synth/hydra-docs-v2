---
title: Record hydra's output
---

# Recording
---

## Saving images from Hydra

You can press `Ctrl+Shift+S` to save a screenshot of your Hydra's canvas, as well as the code that generates the screenshot. You can also do this programmatically calling the function `screencap()`

---

## Hydra's built-in recorder

You can very easily record a video evaluating the following commands:

```javascript
vidRecorder.start() // run this to start recording

vidRecorder.stop()  // run this to stop recording and download video
```

Videos recorded with this method are recorded and downloaded with the vp9 codec and webm filetype. However, they can also be quite low quality.

---

## OBS

[OBS](https://obsproject.com/) is the preferred recording method by most Hydra users. It's a free and open source software for video recording and live streaming available on practically all platforms.

### How to use OBS

1. Download and open OBS
2. In the Controls pane on the right side, click on Settings
3. Go to the Output tab, then to the Recording pane and set your preferences. Then click on OK.
4. On the Sources pane, click on the + icon and select Window Capture. If you want to, give the new source a name and click OK
5. Set the Window to the browser your running Hydra on. Most users also prefer to unselect the Capture cursor setting. Click OK.
6. On the preview at the middle of the screen, select your source, right click and then click on Resize output (source size), select Yes.
7. Set your audio input's volume (or mute it) on the Audio Mixer pane.
8. On the Controls pane, click Start Recording. When you want to stop, click Stop Recording.

#### Trim the browser's UI

If you don't want to record your browser's UI, do the following:
1. Right click your source and click on Filters.
2. Click on the + icon on the lower left corner and select Crop/Pad. Click OK.
3. Set the values until your UI disappears. Sometimes you only need to set the Top value.
4. Click OK

#### Note on window size

If you want to set your browser's window to a specific size, there are various add-ons on different browsers that allow you to do that. Remember you want to set the size of the view pane and not the whole window.

---
by geikha