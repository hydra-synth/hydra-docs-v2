---
title: Blend
date: 2023-11-16
---
# Blend

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

