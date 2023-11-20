---
title: Geometry and color
weight: 3
---
## Adding transformations
We can add another transformation to the oscillator from above, by adding the function `rotate()` after the oscillator:
```hydra
osc(5,-0.126,0.514).rotate().out()
```

As you can see, you have first an input source `osc()` and things that come after (`rotate()` and `out()`) are connected with a dot ‘.’
In this sense, Hydra is inspired by [modular synthesis](https://en.wikipedia.org/wiki/Modular_synthesizer).
Instead of connecting cables you connect different kinds of javascript functions.  
![](https://i.imgur.com/RBRxeiL.jpg)
###### source [Sandin Image Processor](https://en.wikipedia.org/wiki/Sandin_Image_Processor)

You can continue adding transformations to this chain of functions. For example:  
```hydra
osc(5,-0.126,0.514).rotate(0, 0.2).kaleid().out()
```

Repeat: 
```hydra
osc(5,-0.126,0.514).rotate(0, 0.2).kaleid().repeat().out()
```


For more available sources and transformations, see the [interactive function reference](./../../../reference). 
The logic is to start with a [***source***](./../../../reference/api/src) (such as `osc()`, `shape()`, or `noise()`), and then add transformations to [***geometry***](./../../../reference/api/coord) and [***color***](./../../../reference/api/color) (such as `.rotate()`, `.kaleid()`, `.pixelate()` ), and in the end always connect the chain of transformations to the output screen `.out()` .


```hydra
noise(4).color(-2, 1).colorama().out()
```

```hydra
shape(3).repeat(3, 2).scrollX(0, 0.1).out()
```

## What is an error? 
Sometimes, you will try to run a line of code, and nothing will happen. If you have an error you’ll notice text in red at the left-bottom on your screen. Something like ‘Unexpected token ‘.’ (in red) will appear. This doesn’t affect your code, but you won’t be able to continue coding until you fix the error. Usually it is a typing error or something related to the syntax. 

## What is a comment?

```javascript
// Hello I’m a comment line. I’m a text that won’t change your code. You can write notations, your name or even a poem here.
```

## Save your sketch on the internet


When you evaluate the entire code with the ***run button*** or with `shift + ctrl + enter`, Hydra automatically generates a URL that contains the last changes of your sketch. You can copy and paste the url from the URL bar to save it or share it with other people. You can also use the browser `back` and `forward` arrows to navigate to earlier versions of your sketch. 
![](https://i.imgur.com/lV0rmoh.png)
