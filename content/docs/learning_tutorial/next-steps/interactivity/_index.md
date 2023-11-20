---
title: "Interactivity: mouse, audio, and MIDI"
draft: false
weight: 4
bookCollapseSection: true
---

# Interactivity: mouse, audio, and MIDI


 Custom functions are especially useful for controlling hydra parameters using external inputs, such as the microphone, mouse, or midi control. 

## Mouse interactivity

Hydra visuals can react to the position of your mouse (or finger, in touch devices). Hydra has an object called `mouse` which stores and keeps track of the position of your mouse on the webpage.

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


