---
title: Sources
date: 2023-11-16
weight: 1
---
# Source

### noise
noise( scale = 10, offset = 0.1 )

Generate [Perlin noise](https://en.wikipedia.org/wiki/Perlin_noise).
```hydra
// default
noise(10, 0.1).out(o0)
```

### voronoi
voronoi( scale = 5, speed = 0.3, blending = 0.3 )

Generate [voronoi shapes](https://en.wikipedia.org/wiki/Voronoi_diagram).
```hydra
// default
voronoi(5,0.3,0.3).out(o0)
```

### osc
osc( frequency = 60, sync = 0.1, offset )

```hydra
// frequency
osc( [1,10,50,100,250,500].fast(2) ).out(o0)
```

### shape
shape( sides = 3, radius = 0.3, smoothing = 0.01 )

```hydra
// triangle
shape(3,0.5,0.001).out(o0)
```

### gradient
gradient( speed )

```hydra
// gradient sequence at speeds of 1, 2 & 4
gradient([1,2,4]).out(o0)
```

### src
src( tex )

See `hydra-examples` repository
```hydra
// feedback
src(o0).modulate(noise(3),0.005).blend(shape(4),0.01).out(o0)
```

### solid
solid( r, g, b, a = 1 )

```hydra
// cycling through red, green and blue
solid([1,0,0],[0,1,0],[0,0,1],1).out(o0)
```

### prev
prev(  )



### out
out( texture = all )

```hydra
// default
osc().out(o1)
src(o1).out(o0)
```

