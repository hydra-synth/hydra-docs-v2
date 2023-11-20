---
title: Color
date: 2023-11-16
weight: 3
---
# Color

### posterize
posterize( bins = 3, gamma = 0.6 )

```hydra
// static gradient posterized, varying bins
gradient(0).posterize( [1, 5, 15, 30] , 0.5 ).out(o0)
```

### shift
shift( r = 0.5, g, b, a )

```hydra
// default
osc().shift(0.1,0.9,0.3).out()
```

### invert
invert( amount = 1 )

Invert color.
```hydra
// default
solid(1,1,1).invert([0,1]).out(o0)
```

### contrast
contrast( amount = 1.6 )

Larger amount value makes higher contrast.
```hydra
// 20Hz oscillator with contrast interpolating between 0.0-5.0
osc(20).contrast( () => Math.sin(time) * 5 ).out(o0)
```

### brightness
brightness( amount = 0.4 )

```hydra
// default
osc(20,0,2)
  .brightness( () => Math.sin(time) )
  .out(o0)
```

### luma
luma( threshold = 0.5, tolerance = 0.1 )

```hydra
// default
osc(10,0,1).luma(0.5,0.1).out(o0)
```

### thresh
thresh( threshold = 0.5, tolerance = 0.04 )

```hydra
// default
noise(3,0.1).thresh(0.5,0.04).out(o0)
```

### color
color( r = 1, g = 1, b = 1, a = 1 )

```hydra
// default
osc().color(1,0,3).out(o0)
```

### saturate
saturate( amount = 2 )

```hydra
// default
osc(10,0,1).saturate( () => Math.sin(time) * 10 ).out(o0)
```

### hue
hue( hue = 0.4 )

```hydra
// default
osc(30,0.1,1).hue(() => Math.sin(time)).out(o0)
```

### colorama
colorama( amount = 0.005 )

Shift HSV values.
```hydra
// // 20Hz oscillator source
// // color sequence of Red, Green, Blue, White, Black
// // colorama sequence of 0.005, 0.5, 1.0 at 1/8 speed
// // output to buffer o0
osc(20)
  .color([1,0,0,1,0],[0,1,0,1,0],[0,0,1,1,0])
  .colorama([0.005,0.33,0.66,1.0].fast(0.125))
  .out(o0)
```

### sum
sum( scale = 1 )



### r
r( scale = 1, offset )

```hydra
// default
osc(60,0.1,1.5).layer(gradient().r()).out(o0)
```

### g
g( scale = 1, offset )

```hydra
// default
osc(60,0.1,1.5).layer(gradient().g()).out(o0)
```

### b
b( scale = 1, offset )

```hydra
// default
osc(60,0.1,1.5).layer(gradient().colorama(1).b()).out(o0)
```

### a
a( scale = 1, offset )



