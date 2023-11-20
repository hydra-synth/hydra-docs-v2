---
title: Synth Settings
date: 2023-11-16
---
# Synth Settings
Functions and settings that affect overall hydra behavor and rendering. 

### render
render( texture = all )

```javascript
// default
osc(30,0.1,1.5).out(o0)
noise().out(o1)
solid(1).out(o2)
gradient().out(o3)
render()
```

### update
update(  )

```hydra
// update is called every frame
b = 0
update = () => b += 0.01 * Math.sin(time)
shape().scrollX(()=>b).out(o0)
```

### setResolution
setResolution( width, height )

```javascript
// make the canvas small (100 pixel x 100 pixel)
setResolution(100,100)
osc().out(o0)
```

### hush
hush(  )

```javascript
// clear the buffers
osc().out(o0)
hush()
```

### setFunction
setFunction( options )

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

### speed
speed = 1

```hydra
// change overall speed
speed = 3
osc(60,0.1,[0,1.5]).out(o0)
```

### bpm
bpm = 30

```hydra
// change array speed
bpm = 60
osc(60,0.1,[0,1.5]).out(o0)
```

### width
width

```hydra
shape(99).scrollX(() => -mouse.x / width).out(o0)
```

### height
height

```hydra
shape(99).scrollY(() => -mouse.y / height).out(o0)
```

### time
time

```hydra
// default
shape(2,0.8).kaleid(()=>6+Math.sin(time)*4).out(o0)
```

### mouse
mouse = { x, y }

```hydra
shape(99).scroll(
  () => -mouse.x / width,
  () => -mouse.y / height)
  .out(o0)
```

