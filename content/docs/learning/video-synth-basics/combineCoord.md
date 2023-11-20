---
title: Modulate
date: 2023-11-16
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


## Available modulate functions

### modulateRepeat
modulateRepeat( texture, repeatX = 3, repeatY = 3, offsetX = 0.5, offsetY = 0.5 )


The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
```hydra
// default
shape(4,0.9)
  .mult(osc(3,0.5,1))
  .modulateRepeat(osc(10), 3.0, 3.0, 0.5, 0.5)
  .out(o0)
```

### modulateRepeatX
modulateRepeatX( texture, reps = 3, offset = 0.5 )


The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
```hydra
// straight lines illusion
shape(4,0.9)
  .mult(osc(4,0.25,1))
  .modulateRepeatX(osc(10), 5.0, ({time}) => Math.sin(time) * 5)
  .scale(1,0.5,0.05)
  .out(o0)
```

### modulateRepeatY
modulateRepeatY( texture, reps = 3, offset = 0.5 )


The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
```hydra
// morphing grid
shape(4,0.9)
  .mult(osc(4,0.25,1))
  .modulateRepeatY(osc(10), 5.0, ({time}) => Math.sin(time) * 5)
  .scale(1,0.5,0.05)
  .out(o0)
```

### modulateKaleid
modulateKaleid( texture, nSides = 4 )


The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
See also: [`kaleid`](#kaleid).
```hydra
// default
osc(9,-0.1,0.1)
  .modulateKaleid(osc(11,0.5,0),50)
  .scale(0.1,0.3)
  .modulate(noise(5,0.1))
  .mult(solid(1,1,0.3))
  .out(o0)
```

### modulateScrollX
modulateScrollX( texture, scrollX = 0.5, speed )


The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
See also: [`scrollX`](#scrollX)
```hydra
// default
voronoi(25,0,0)
  .modulateScrollX(osc(10),0.5,0)
  .out(o0)
```

### modulateScrollY
modulateScrollY( texture, scrollY = 0.5, speed )


The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
See also: [`scrollY`](#scrollY)
```hydra
// default
voronoi(25,0,0)
  .modulateScrollY(osc(10),0.5,0)
  .out(o0)
```

### modulate
modulate( texture, amount = 0.1 )


Modulate texture.
More about modulation at: <https://lumen-app.com/guide/modulation/>
The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
```hydra
// chocolate whirlpool
voronoi()
  .color(0.9,0.25,0.15)
  .rotate(({time})=>(time%360)/2)
  .modulate(osc(25,0.1,0.5)
              .kaleid(50)
              .scale(({time})=>Math.sin(time*1)*0.5+1)
              .modulate(noise(0.6,0.5)),
              0.5)
  .out(o0)
```

### modulateScale
modulateScale( texture, multiple = 1, offset = 1 )


The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
See also: [`scale`](#scale).
```hydra
// cosmic radiation
gradient(5).repeat(50,50).kaleid([3,5,7,9].fast(0.5))
  .modulateScale(osc(4,-0.5,0).kaleid(50).scale(0.5),15,0)
  .out(o0)
```

### modulatePixelate
modulatePixelate( texture, multiple = 10, offset = 3 )


The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
See also: [`pixelate`](#pixelate)
```hydra
// what lies beneath
voronoi(10,1,5).brightness(()=>Math.random()*0.15)
  .modulatePixelate(noise(25,0.5),100)
  .out(o0)
```

### modulateRotate
modulateRotate( texture, multiple = 1, offset )


The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
See also: [`rotate`](#rotate)
```hydra
// wormhole
voronoi(100,3,5)
  .modulateRotate(osc(1,0.5,0).kaleid(50).scale(0.5),15,0)
  .mult(osc(50,-0.1,8).kaleid(9))
  .out(o0)
```

### modulateHue
modulateHue( texture, amount = 1 )


Changes coordinates based on hue of second input. Based on: https://www.shadertoy.com/view/XtcSWM
The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
```hydra
src(o0)
  .modulateHue(src(o0).scale(1.01),1)
  .layer(osc(4,0.5,2).mask(shape(4,0.5,0.001)))
  .out(o0)
```

