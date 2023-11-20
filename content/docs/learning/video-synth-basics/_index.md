---
weight: 3
---

Hydra is inspired by [modular synthesis](https://en.wikipedia.org/wiki/Modular_synthesizer).
Instead of connecting cables you connect different kinds of javascript functions.  
![](https://i.imgur.com/RBRxeiL.jpg)
###### source [Sandin Image Processor](https://en.wikipedia.org/wiki/Sandin_Image_Processor)


The logic is to start with a ***source*** (such as `osc()`, `shape()`, or `noise()`), and then add transformations to ***geometry*** and ***color*** (such as `.rotate()`, `.kaleid()`, `.pixelate()` ), and in the end always connect the chain of transformations to the output screen `.out()` .

for example, the following code renders an oscillator with parameters frequency, sync, and rgb offset:
```hydra
osc(5, -0.126, 0.514).out()
```

We can add another transformation to the oscillator from above, by adding the function `rotate()` after the oscillator:
```hydra
osc(5,-0.126,0.514).rotate().out()
```

Pixelate the output of the above function: 

```hydra
osc(5,-0.126,0.514).rotate().pixelate().out()
```