# Audio Guide
---

## Reacting to audio

In order to achieve audio reactivity, Hydra makes use of a JavaScript library called Meyda and has a pre-defined object called `a` to access many of its features. Audio reactivity in Hydra is mainly achieved using an algorithm called Fast Fourier transform. You definitely don't need to know what it is or how it achieves what it does to use it, but you need to understand the following:

### The audio spectrum

Sound travels through air as a wave, that's fairly common knowledge. This basically means that sound is nothing more than air pressure going up and down through time very fast in weird ways. But we don't experience sound simply as something that goes on and off like a light flickering, many of the sounds we are used to have some sort of frequency or repetition that we interpret as higher or lower pitch. 
When we listen to a song we can easily differentiate the bass guitar from the singer even if both are playing at the same time. If there are two vocals being sung at the same time, even if by the same person, we can differentiate them because of how high or low they are (also because of timbre, but that doesn't matter at all right now).
When we talk about the audio spectrum, we are talking about the many frequencies a sound can cover and we humans can hear.
What a fast fourier transform does is basically hear some ongoing sound and interpret how present the sound is on different parts of this spectrum. For example, if we separate the audio spectrum in 3 equal parts and play a drum-kit, the bass drum will have more presence on the lower side of the spectrum, while a hi-hat will surely have most of its presence on the higher third part of the spectrum.

### a.show() & a.hide()

We can see what Hydra hears by calling the function `a.show()`. This will show a small graphic on the lower right corner of the screen. If we want to hide it we can call `a.hide()`

### a.setBins() & a.fft 

We can decide into how many parts we want to separate the audio spectrum using the function `a.setBins()`. As you might've already guessed, a bin is just a part of the audio spectrum. Try now to use very high values since more bins means more processing and you can go overkill easily. But also, you'll see there usually isn't much need to separate the spectrum into that many parts.
To read the current value of a bin we use an Array (a list of variables per se) called `a.fft`.

See the following example:

```javascript
a.setBins(5)
osc(20,.1,2)
	.saturate(()=>1-a.fft[4])
	.rotate(()=>a.fft[0])
	.kaleid()
	.out()
```

See how if you make a deep "O" sound into the mic, the rotation will be strong and the saturation won't be affected as much. Also try to make a high "S" sound, you'll see the exact opposite.

Note how we use brackets to call an element in an Array, and that we start counting from 0.

#### a.bins & a.prevBins

If you want the raw values from each bin without the mapping from 0 to 1, you can access them via  `a.bins`. You can also use `a.prevBins` to get the bins from the previous frame.

### a.setSmooth()

Sometimes the audio reactive elements react... too much. You can easily get into strobe territory if you are not careful. You can smooth out the interpretation of the sound using the `a.setSmooth()` function. A value of 0 will be no smoothing at all, a raw input, while a value of 1 will be so smooth nothing will happen at all. Try evaluating `a.setSmooth(.85)` above and see how different it looks.

### a.setScale() & a.setCutoff()

Each microphone, each sound input, etc, can be quite different in volume and dynamics. If you're on a noisy room, your visuals could react to the noise and that's quite annoying. Or if your mic is too low on volume, your visuals may barely react.
If you ran `a.show()` and look at the graph, you may have noticed there are 2 horizontal lines going across the bins. The lowest one represents the cutoff (guitar players and other musicians out here might know this as a noise gate), this means that the value of the bin will be 0 unless the sound goes above that cutoff. The higher line is the scale, that's where the maximum value of 1 is (again, musicians may want to see this as a limiter with auto-gain). If a given bin goes above the scale value, its value won't go past 1.

### a.vol

`a.vol` will give you the overall volume of the audio input.

### a.onBeat() & a.beat

Hydra also has a simple beat detection algorithm. You can change this function to anything you like and it will be executed whenever Hydra detects a beat. The beat detection algorithm uses values from `a.vol` and compares them with a threshold set at `a.beat`.

`a.beat` stores the configuration for the beat detection Hydra uses. It's an Object which most useful parameter is `threshold`. `a.beat.threshold` represents the volume to which `a.vol` will be compared to detect a beat. There's also `a.beat.decay` which sets the decay after a beat.

### Reacting to music

As we've seen, Hydra takes your microphone as an input, not your desktop audio. Those interested in using a music player or a DAW's output as an audio input will have to delve into virtual audio routing. Users with physical sound interfaces with multiple inputs and outputs might prefer physically routing an output to an input and set that input as the default microphone on Chrome.

---

## Virtually routing audio to Hydra

TODO