---
title: Combining visuals using blending and modulation
weight: 6
---

## Multiple outputs 

By default, hydra contains four separate virtual outputs that can each render different visuals, and can be mixed with each other to create more complex visuals. The variables `o0`, `o1`, `o2`, and `o3` correspond to the different outputs. 

To see all four of the outputs at once, use the `render()` function. This will divide the screen into four, showing each output in a different section of the screen. 

![](https://i.imgur.com/m5Q0Na6.jpg)

Using a different variable inside the `.out()` function renders the chain to a different output. For example, `.out(o1)` will render a function chain to graphics buffer `o1`. 


```hydra
gradient(1).out(o0) // render a gradient to output o0
osc().out(o1) // render voronoi to output o1
voronoi().out(o2) // render voronoi to output o2
noise().out(o3)  // render noise to output o3

render()  // show all outputs
```

By default, only output `o0` is rendered to the screen, while the `render()` command divides the screen in four. Show a specific output on the screen by adding it inside of `render()`, for example `render(o2)` to show buffer `o2`.


```hydra
gradient(1).out(o0) // render a gradient to output o0
osc().out(o1) // render voronoi to output o1
voronoi().out(o2) // render voronoi to output o2
noise().out(o3)  // render noise to output o3

render(o2)  // show only output o2
```


*Trick: try to create different sketches and switch them in your live performance or even combine them.*


```hydra
gradient(1).out(o0)
osc().out(o1)
render(o0) //switch render output
// render(o1) 
```

## Blending multiple visual sources together
You can use ***blend*** functions to combine multiple visual sources. `.blend()` combines the colors from two sources to create a third source. 

```hydra
s0.initCam()

src(s0).out(o0) // render the webcam to output o0

osc(10).out(o1) // render an oscillator to output o1

src(o0).blend(o1).out(o2) // start with o0, mix it with o1, and send it out to o2

render() // render all four outputs at once
```

Try adding transformations to the above sources (such as `osc(10).rotate(0, 0.1).out(o1)`) to see how it affects the combined image. You can also specify the amount of blending by adding a separate parameter to `.blend()`, for example `.blend(o1, 0.9)`. 

There are multiple [blend modes](https://en.wikipedia.org/wiki/Blend_modes) in hydra, similar to blend modes you might find in a graphics program such as photoshop or gimp. See [the function reference](https://hydra.ojack.xyz/api/) for more possibilities. 

```hydra
s0.initCam()

src(s0).out(o0) // render the webcam to output o0

osc(10).out(o1) // render an oscillator to output o1

src(o0).diff(o1).out(o2) // combine different signals by color difference (dark portions become inverted).

render() // render all four outputs at once
```

---
title: modulation

---

## Modulation
While ***blend*** functions combine the colors from two visual sources, ***modulate*** functions use the colors from one source to affect the ***geometry*** of the second source. This creates a sort of warping or distorting effect. An analogy in the real world would be looking through a texture glass window.
`modulate()` does not change color or luminosity but distorts one visual source using another visual source.

Using the same sources from above, we can use an oscillator to modulate or warp the camera image: 

```hydra
s0.initCam()

src(s0).out(o0) // render the webcam to output o0
osc(10).out(o1) // render an oscillator to output o1

src(o0).modulate(o1).out(o2) // use source o1 to distort source o0, lighter areas are distorted more

render() // render all four outputs at once
```

You can add a second parameter to the `modulate()` function to control the amount of warping:  `modulate(o1, 0.9)`. In this case, the red and green channels of the oscillator are being converted to x and y displacement of the camera image. 

All ***geometry*** transformations have corresponding ***modulate*** functions that allow you to use one source to warp another source. For example, `.modulateRotate()` is similar to `.rotate()`, but it allows you to apply different amounts of rotation to different parts of the visual source. See [the function reference](https://hydra.ojack.xyz/api/) for more examples. 

```hydra
s0.initCam()

src(s0).out(o0) // render the webcam to output o0
osc(10).out(o1) // render an oscillator to output o1

src(o0).modulateRotate(o1, 2).out(o2) // 

render() // render all four outputs at once
```

## More blending and modulating

In addition to using multiple outputs to combine visuals, you can also combine multiple sources within the same function chain, without rendering them to separate outputs.

```hydra
osc(10, 0.1, 1.2).blend(noise(3)).out(o0)

render(o0) // render output o0
```

This allows you to use many sources, blend modes, and modulation, all from within the same chain of functions. 

```hydra
osc(10, 0.1, 1.2).blend(noise(3)).diff(shape(4, 0.6).rotate(0, 0.1)).out()
```

*Trick: use `ctrl + shift + f` from the web editor to auto-format your code*

#### Modulating with the camera
```hydra
s0.initCam() //loads a camera

shape().modulate(src(s0)).out() //shape modulated by a camera
```
```hydra
s0.initCam() //loads a camera

src(s0).modulate(shape()).out() //camera modulated by a shape
```






```hydra

noise().out(o1)
shape().out(o3)

src(o1).add(src(o3)).out(o2) //additive light. Color only gets brighter

render() 
```

```hydra
osc(10).out(o0)

shape().out(o1)

src(o0).diff(o1).out(o2) // combines different signals by color difference (color negative/inverted/opposite).  

render()
```
```hydra
osc().mult(src(o1)).out() // multiplies the sources together, 
shape(5).out(o1)

```


We have now covered all of the basic types of functions within hydra: ***source***, ***geometry***, ***color***, ***blending***, and ***modulation***! See what you can come up with by mixing these together. 


#### Have fun!

