---
title: "sequencing & interactivity"
draft: false
author: "Ritchse"
weight: 4
---

# Sequencing and Interactivity
by [geikha](https://github.com/geikha) and olivia

---
 If you're coding in Hydra, you're constantly trying many values to input to the sources and transforms, and it's just a matter of time until you like how more than one looks, and you want to somehow switch between them. We'll be referring to this idea of arguments whose value change over time as dynamic arguments. And there are two main ways to achieve this in Hydra: Arrays and functions. 

## Sequencing using Arrays

### Sequence your inputs


When you send an Array as an input (indicated in javascript by `[]`), Hydra will automatically switch and jump from each element from the Array to the next one. When there are no more elements, it wraps all the way back to the beginning. Let's see it in action: 

```hydra
shape([3, 4, 5, 6, 7, 8])
	.out()
```
### changing the speed

The arrays in hydra have a default bpm(beats-per-minute) of 30. You can change the speed of a specific array by adding `.fast()` at the end of the array. For example `.fast(4)` will make the above array run four times faster.

```hydra
shape([3, 4, 5, 6, 7, 8].fast(4))
	.out()
```

The speed of all arrays in a sketch can be changed using the `bpm` parameter of hydra synth. 

```javascript
bpm = 60
```

<!-- As you can see, the fact that both these Arrays have a different amount of values doesn't matter, Hydra will take values from each element of any Array for the same amount of time by default. -->

<!-- ### Changing the speed of a specific Array

Hydra adds a couple of methods to all Arrays to be used inside Hydra. `.fast` will control the speed at which Hydra takes elements from the Array. It receives a Number as argument, by which the global speed will be multiplied. So calling `.fast(1)` on an Array is the same as nothing. Higher values will generate faster switching, while lower than 1 values will be slower. 

```hydra
bpm = 45
osc([20,30,50,60],.1,[0,1.5].fast(1.5)) // 50% faster
    //.rotate([-.2,0,.2].fast(1)) // try different speeds for each array
	.out()
``` -->

<!-- #### Offsetting the timing of an Array

Another one of the methods Hydra adds to Arrays, allows you to offset the timing at which Hydra will switch from one element of the Array to the next one. The method `.offset` takes a Number from 0 to 1.

```hydra
bpm = 45
osc([20,30,50,60],.1,[0,1.5].offset(.5)) // try changing the offset
	.out()
``` -->

<!-- #### Fitting the values of an Array within a range

Sometimes you have an Array whose values aren't very useful when used as input for a some Hydra function.
Hydra adds a `.fit` method to Arrays which takes a minimum and a maximum to which fit the values into:

```hydra
bpm = 120
arr = ()=> [1,2,4,8,16,32,64,128,256,512]
osc(50,.1,arr().fit(0,Math.PI))
	.scale(arr().fit(1,2))
	.out()
``` -->

### smooth() interpolation

You can also interpolate between values instead of jumping from one to the other. That is, smoothly transition between values. For this you can use the `.smooth` method. It may take a Number argument (defaulted to 1) which controls the smoothness.

```hydra
shape([3, 4, 5, 6, 7, 8].smooth())
	.out()
```

<!-- ```hydra
bpm = 50
arr = [0,0.8,2]
osc(50,.1,arr.smooth())
	.rotate(arr.fit(-Math.PI/4,Math.PI/4).smooth())
	.out()
```

Try smoothing some of the above examples and see what happens! -->

<!-- ##### Easing functions

The default interpolation used by Hydra on an Array that called `.smooth` is linear interpolation. You can select a different easing function as follows:
```hydra
shape([3, 4, 5, 6, 7, 8].smooth().ease('easeInQuad'))
	.out()
```


```hydra
bpm = 50
arr = [0,0.8,2]
osc(50,.1,arr.ease('easeInQuad'))
	.rotate(arr.fit(-Math.PI/4,Math.PI/4).ease('easeOutQuad'))
	.out() // try other easing functions !
```

The following are the available easing functions:

* linear: no easing, no acceleration
* easeInQuad: accelerating from zero velocity
* easeOutQuad: decelerating to zero velocity
* easeInOutQuad: acceleration until halfway, then deceleration
* easeInCubic
* easeOutCubic
* easeInOutCubic
* easeInQuart
* easeOutQuart
* easeInOutQuart
* easeInQuint
* easeOutQuint
* easeInOutQuint
* sin: sinusoidal shape
--- -->

## Custom Functions

Each numerical parameter in hydra can be defined as a function rather than a static variable. For example,
```javascript
osc(function(){return 100 * Math.sin(time * 0.1)}).out()
```
modifies the oscillator frequency as a function of time. (Time is a global variable that represents the milliseconds that have passed since loading the page). 

The above example can be written more concisely using es6 syntax:

```javascript
osc(() => 100 * Math.sin(time * 0.1)).out()
```
<!-- The other main way of adding dynamic inputs to your sketches is passing functions as arguments. When Hydra takes a function as an argument, what it will do is evaluate it every time it renders a frame. The return of the function will be used as the value for that parameter during that frame render. So you can use a function to simply keep track of a value that you know will change over time, for example, mouse position (which we'll see later). -->

 Custom functions are especially useful for controlling hydra parameters using external inputs, such as the microphone, mouse, or midi control. 


<!-- #### Changing the global speed

You can either slow down or fasten the rate at with `time` increases via changing the `speed` variable:

```javascript
speed = 1  // default
speed = 2  // twice as fast
speed = .5 // half as fast
speed = 0  // freezed 
``` -->



---

### Mouse interactivity

You can have your visuals react to the position of your mouse (or finger, in touch devices). Hydra has an object called `mouse` which stores and keeps track of the position of your mouse on the webpage.

#### mouse.x & mouse.y

```hydra
gradient()
	.hue(()=>mouse.x/3000)
	.scale(1,1,()=>mouse.y/1000)
	.out()
```
|
You can refer to the pixel position of your mouse by calling `mouse.x` and `mouse.y`, each one corresponding to the horizontal and vertical coordinates respectively. When we say 'pixel position', this means that the values you'll find stored in both x and y are represented in pixels. So for `mouse.x`, this means the amount of pixels from the left edge of your window to the position of your mouse. For `mouse.y`, this means the amount of pixels between the top end of your screen and the position of your mouse.

Many times it will be most useful to use values relative to the size of the screen. And also to have values that exist between ranges more reasonable to the hydra functions you're using. For example [-0.5; 0.5] for scrollX and scrollY, [0; 2pi] for rotation, or [0; 1] for general purposes.

#### Note

All of the examples using mouse position to move stuff on the canvas won't work well here, since the canvas doesn't occupy the full size of the screen as in the editor. Take this into account when we use `mouse`, that the positions are relative to the full webpage and not the canvas. This also means that as you scroll down this guide the `y` value will get higher and higher.

#### Control anything with your mouse

On Hydra, most values used are pretty small. So it will be way more useful to have the position of the mouse as values from 0 and 1:

#### Getting values from 0 to 1

```hydra
x = () => mouse.x/width // 0→1
y = () => mouse.y/height // 0→1
osc()
        .scale(()=>1+x()*2)
        .modulate(noise(4),()=>y()/4)
        .out()
```

You can simply multiply by `2*Math.PI` to change the range to [0; 2pi]

#### Make something follow your mouse

On Hydra, things are placed between 0.5 and -0.5 (left to right, top to bottom). In order for anything to follow your mouse, you'll need to get the position of your mouse between that range:

#### Getting values from 0 to ±0.5 from the center

```hydra
x = () => (-mouse.x/width)+.5 // 0.5→-0.5
y = () => (-mouse.y/height)+.5 // 0.5→-0.5
solid(255)
    .diff(
        shape(4,.1)
        .scroll(()=>x(),()=>y())
    )
    .out()
```

Remember you can name these functions however you prefer.

---

## Audio reactivity
FFT functionality is available via an audio object accessed via "a". The editor uses https://github.com/meyda/meyda for audio analysis.
To show the fft bins,
```javascript
a.show()
```
Set number of fft bins:
```javascript
a.setBins(6)
```
Access the value of the leftmost (lowest frequency) bin:
```javascript
a.fft[0]
```
Use the value to control a variable:
```javascript
osc(10, 0, () => a.fft[0]*4)
  .out()
```
It is possible to calibrate the responsiveness by changing the minimum and maximum value detected. (Represented by blur lines over the fft). To set minimum value detected:
```javascript
a.setCutoff(4)
```

Setting the scale changes the range that is detected.
```javascript
a.setScale(2)
```
The fft[<index>] will return a value between 0 and 1, where 0 represents the cutoff and 1 corresponds to the maximum.

You can set smoothing between audio level readings (values between 0 and 1). 0 corresponds to no smoothing (more jumpy, faster reaction time), while 1 means that the value will never change.
```javascript
a.setSmooth(0.8)
```
To hide the audio waveform:
```javascript
a.hide()
```

```javascript
a.setBins(5) // amount of bins (bands) to separate the audio spectrum

noise(2)
	.modulate(o0,()=>a.fft[1]*.5) // listening to the 2nd band
	.out()

a.setSmooth(.8) // audio reactivity smoothness from 0 to 1, uses linear interpolation
a.setScale(8)    // loudness upper limit (maps to 0)
a.setCutoff(0.1)   // loudness from which to start listening to (maps to 0)

a.show() // show what hydra's listening to
// a.hide()

render(o0)
```


```hydra
voronoi(5,.1,()=>Math.sin(time*4))
	.out()
```

The `time` variable seen there is a variable pre-declared by Hydra, that stores how much time passed since Hydra started in seconds.

Functions used in Hydra don't need to be arrow functions, any no-argument function will do! Make sure your function is returning a Number to avoid errors.

### The time variable

When you use functions that can take numerical arguments, `time` will allow you to have their values evolve through... time. If you multiply time by some value it's as if time goes faster, while dividing while act as making time go slower. For example `Math.sin(time*4)` will go 4 times faster than `Math.sin(time)`.

Those users more familiar with mathematics might see this as:

* `y(t) = t` : `()=>time`
* `y(t) = A sin(f t + ph)` : `()=>amplitude*Math.sin(freq*time + phase)`

We recommend getting familiar with some of the methods in the JS built-in `Math` object. Learn more about it [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)


## MIDI 
Hydra can be used with [Web MIDI](https://webaudio.github.io/web-midi-api/) for an extra layer of control to your visuals. 

### Example script: browser console
 At this time this requires some running of code on the
browser console (Press F12 in Chrome to access).  This page only considers MIDI Continuous Controllers (CC) but other types of data may be accessible.

This is a generic script that doesn't care what Midi Channel you're broadcasting on and maps a normalized value 0.0-1.0 into an array named cc. 

This portion should be ran in the console & will register Web MIDI & map the incoming CC data to a set of parameters.  For simplicity, these
parameters are named to match the CC number.  The CC values are normally in a range from 0-127, but we've also normalized them to be in a range of 0.0-1.0.

```javascript
// register WebMIDI
navigator.requestMIDIAccess()
    .then(onMIDISuccess, onMIDIFailure);

function onMIDISuccess(midiAccess) {
    console.log(midiAccess);
    var inputs = midiAccess.inputs;
    var outputs = midiAccess.outputs;
    for (var input of midiAccess.inputs.values()){
        input.onmidimessage = getMIDIMessage;
    }
}

function onMIDIFailure() {
    console.log('Could not access your MIDI devices.');
}

//create an array to hold our cc values and init to a normalized value
var cc=Array(128).fill(0.5)

getMIDIMessage = function(midiMessage) {
    var arr = midiMessage.data    
    var index = arr[1]
    //console.log('Midi received on cc#' + index + ' value:' + arr[2])    // uncomment to monitor incoming Midi
    var val = (arr[2]+1)/128.0  // normalize CC values to 0.0 - 1.0
    cc[index]=val
}
```

#### Hydra script
Now that these controls have been assigned to the cc[] array, we can start using them in Hydra.  As we've normalized the values 0-1 we can use
as-is with most functions or quickly remap them with various math.  
```javascript
// example midi mappings - Korg NanoKontrol2 CCs

// color controls with first three knobs
noise(4).color( ()=>cc[16], ()=>cc[17], ()=>cc[18] ).out()

// rotate & scale with first two faders
osc(10,0.2,0.5).rotate( ()=>(cc[0]*6.28)-3.14 ).scale( ()=>(cc[1]) ).out()

```

### MIDI extension
