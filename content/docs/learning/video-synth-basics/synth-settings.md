---
title: synth settings
---

## synth settings 
Functions and settings that affect overall hydra behavor and rendering. 
### render()
`render( texture )`

```hydra
	// default
	osc(30,0.1,1.5).out(o0)
	noise().out(o1)
	solid(1).out(o2)
	gradient().out(o3)
	render()
```
```hydra
// specify display buffer
voronoi().out(o1)
render(o1)
```

### update
User-definable function that is called every frame. 

```hydra
// update is called every frame
b = 0
update = () => b += 0.01 * Math.sin(time)
shape().scrollX(()=>b).out(o0)
```

### setResolution()
```javascript
// make the canvas small (100 pixel x 100 pixel)
setResolution(100,100)
osc().out(o0)
```
note: in hydra web editor, the canvas is styled to fill the screen regardless of resolution, so it will appear stretched. 

### bpm
To change how rapidly Hydra switches from element to element of all Arrays, you can change the `bpm` variable (meaning beats per minute) to any value you desire:

```hydra
bpm = 138 // change me !

gradient()
	.hue([0, 0.1, 0.3, 0.9])
	.out()
```

The default value for `bpm` is 30.

When livecoding visuals at the same time that music is playing, it can be useful to have a tapping metronome opened to keep track of the BPM being played and set this variable as such.

### speed
Change overall speed of all hydra functions
```hydra
speed = 3
osc(60,0.1,[0,1.5]).out(o0)
```
### hush() 
```hydra
// clear all buffers
osc().out(o0)
hush()
```
hush resets hydra. fps, bpm and speed won't be reset to their defaults by calling hush
// their defaults are: fps = undefined, bpm = 30, speed = 1

## setFunction

generate custom glsl
```hydra
// from https://www.shadertoy.com/view/XsfGzn
setFunction({
  name: 'chroma',
  type: 'color',
  inputs: [
    ],
  glsl: `
   float maxrb = max( _c0.r, _c0.b );
   float k = clamp( (_c0.g-maxrb)*5.0, 0.0, 1.0 );
   float dg = _c0.g; 
   _c0.g = min( _c0.g, maxrb*0.8 ); 
   _c0 += vec4(dg - _c0.g);
   return vec4(_c0.rgb, 1.0 - k);
`})
osc(60,0.1,1.5).chroma().out(o0)
```

see guides/customGlsl for more detailed info

## synth variables
Read-only variables defined by hydra-synth that can be used in calculations. 

### width
The current pixel width of the canvas
```hydra
shape(99).scrollX(() => -mouse.x / width).out(o0)
```

### height
The current pixel height of the canvas
```hydra
shape(99).scrollY(() => -mouse.y / height).out(o0)
```

### time
Time in seconds since the page was loaded.
```hydra
shape(2,0.8).kaleid(()=>6+Math.sin(time)*4).out(o0)
```

### mouse
object containing the mouse x and y position in pixel coordinates
```hydra
shape(99).scroll(
  () => -mouse.x / width,
  () => -mouse.y / height)
  .out(o0)
```