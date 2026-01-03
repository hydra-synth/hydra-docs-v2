---
title: outputs
date: 2025-06-06
weight: 31
---

# outputs

By default, hydra contains four separate virtual outputs that can each render different visuals, and can be mixed with each other to create more complex visuals. The variables `o0`, `o1`, `o2`, and `o3` correspond to the different outputs.

## The **out** function
The special Hydra function that allows us to output our visuals is called `out`.

`out( output = o0 )`
{.center}

```hydra
osc().out(o1)
src(o1).out(o0)
```

### The **src** function
In the example above you can also see the `src` function, which allows use to access the video signals in framebuffers such as outputs or external sources. This is how take video from outputs and route them mixing them with each other.

`src( texture )`
<br>default available textures are: `o0, o1, o2, o3, s0, s1, s2, s3`
{.center}

## Using multiple outputs

### Rendering all outputs at once

To see all four of the outputs at once, use the `render()` function. This will divide the screen into four, showing each output in a different section of the screen.

![](../../../images/m5Q0Na6.jpg)
{.center}

Using a different variable inside the `.out()` function renders the patch to a different output. For example, `.out(o1)` will render a function chain to the framebuffer `o1`.

```hydra
gradient(1).out(o0) // render a gradient to output o0
osc().out(o1) // render voronoi to output o1
voronoi().out(o2) // render voronoi to output o2
noise().out(o3)  // render noise to output o3

render()  // show all outputs
```

### Rendering an output on screen

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
