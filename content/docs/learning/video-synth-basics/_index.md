---
weight: 3
bookFlatSection: false
title: "video synth basics"
bookCollapseSection: true
draft: false
---

# video synth basics

## Modularity

Hydra is inspired by [modular synthesis](https://en.wikipedia.org/wiki/Modular_synthesizer).
Instead of connecting modules with cables you connect different kinds of JavaScript functions using dots and calls. Numeric arguments inside functions are analogue to the position of knobs in modules.

![](../../images/RBRxeiL.jpg)
{.center}
###### source [Sandin Image Processor](https://en.wikipedia.org/wiki/Sandin_Image_Processor)

The logic is to start with a ***source*** (such as `osc()`, `shape()`, or `noise()`), and then add transformations to ***geometry*** and ***color*** (such as `.rotate()`, `.kaleid()`, `.pixelate()` ), and in the end always connect the chain of transformations to the output screen `.out()` .

For example, the following code renders an oscillator with parameters frequency, sync, and RGB offset:
```hydra
osc(5, -0.126, 0.514).out()
```

We can add another transformation to the oscillator from above, by adding the function `rotate()` after the oscillator:
```hydra
osc(5,-0.126,0.514).rotate().out()
```

We can expand the patch, pixelating the output of the above function:
```hydra
osc(5,-0.126,0.514).rotate().pixelate().out()
```

---

## Function glossary

See the [interactive function reference](../../reference) for a glossary of all the functions.
