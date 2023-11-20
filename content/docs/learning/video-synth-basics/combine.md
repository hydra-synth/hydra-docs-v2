---
title: Blend
date: 2023-11-16
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

## Available blend funcitons

### add
add( texture, amount = 1 )


Add textures.
The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
```hydra
// default
shape().scale(0.5).add(shape(4),[0,0.25,0.5,0.75,1]).out(o0)
```

### sub
sub( texture, amount = 1 )

```hydra
// default
osc().sub(osc(6)).out(o0)
```

### layer
layer( texture )

Overlay texture based on alpha value.
The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
```hydra
// default
solid(1,0,0,1).layer(shape(4).color(0,1,0,()=>Math.sin(time*2))).out(o0)
```

### blend
blend( texture, amount = 0.5 )


Blend textures.
The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
```hydra
// default
shape().scale(0.5).blend(shape(4),[0,0.25,0.5,0.75,1]).out(o0)
```

### mult
mult( texture, amount = 1 )


Multiply images and blend with the texture by `amount`.
The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
```hydra
// default
osc(9,0.1,2).mult(osc(13,0.5,5)).out()
```

### diff
diff( texture )


Return difference of textures.
The `texture` parameter can be any kind of [source](#sources), for
example a [`color`](#color), [`src`](#src), or [`shape`](#shape).
```hydra
// default
osc(9,0.1,1).diff(osc(13,0.5,5)).out(o0)
```

### mask
mask( texture )

```hydra
// default
gradient(5).mask(voronoi(),3,0.5).invert([0,1]).out(o0)
```

