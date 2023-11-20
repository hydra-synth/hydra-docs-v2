---
title: Change values over time using arrays and custom functions
weight: 1
---
## Sequencing using Arrays

If you're coding in Hydra, you're constantly trying many values to input to the sources and transforms, and it's just a matter of time until you like how more than one looks, and you want to somehow switch between them. We'll be referring to this idea of arguments whose value change over time as dynamic arguments. And there are two main ways to achieve this in Hydra: Arrays and functions. 


### Sequence your inputs


When you send an Array as an input (indicated in javascript by `[]`), Hydra will automatically switch and jump from each element from the Array to the next one. When there are no more elements, it wraps all the way back to the beginning. Let's see it in action: 

```hydra
shape([3, 4, 5, 6, 7, 8])
	.out()
```
### changing the speed

The arrays in hydra have a default bpm(beats-per-minute) of 30. You can change the speed of a specific array by adding `.fast()` at the end of the array. For example `.fast(4)` will make the above array run four times faster.

```hydra
shape([3, 4, 5, 6, 7, 8].fast(4))
	.out()
```

The speed of all arrays in a sketch can be changed using the `bpm` parameter of hydra synth. 

```javascript
bpm = 60
```

<!-- As you can see, the fact that both these Arrays have a different amount of values doesn't matter, Hydra will take values from each element of any Array for the same amount of time by default. -->

<!-- ### Changing the speed of a specific Array

Hydra adds a couple of methods to all Arrays to be used inside Hydra. `.fast` will control the speed at which Hydra takes elements from the Array. It receives a Number as argument, by which the global speed will be multiplied. So calling `.fast(1)` on an Array is the same as nothing. Higher values will generate faster switching, while lower than 1 values will be slower. 

```hydra
bpm = 45
osc([20,30,50,60],.1,[0,1.5].fast(1.5)) // 50% faster
    //.rotate([-.2,0,.2].fast(1)) // try different speeds for each array
	.out()
``` -->

<!-- #### Offsetting the timing of an Array

Another one of the methods Hydra adds to Arrays, allows you to offset the timing at which Hydra will switch from one element of the Array to the next one. The method `.offset` takes a Number from 0 to 1.

```hydra
bpm = 45
osc([20,30,50,60],.1,[0,1.5].offset(.5)) // try changing the offset
	.out()
``` -->

<!-- #### Fitting the values of an Array within a range

Sometimes you have an Array whose values aren't very useful when used as input for a some Hydra function.
Hydra adds a `.fit` method to Arrays which takes a minimum and a maximum to which fit the values into:

```hydra
bpm = 120
arr = ()=> [1,2,4,8,16,32,64,128,256,512]
osc(50,.1,arr().fit(0,Math.PI))
	.scale(arr().fit(1,2))
	.out()
``` -->

### smooth() interpolation

You can also interpolate between values instead of jumping from one to the other. That is, smoothly transition between values. For this you can use the `.smooth` method. It may take a Number argument (defaulted to 1) which controls the smoothness.

```hydra
shape([3, 4, 5, 6, 7, 8].smooth())
	.out()
```


See [reference: arrays](./../../reference/api/array) for a more detailed explanation and example usage of arrays and sequencing in Hydra. 

## Custom Functions
 An even more flexible way of creating dynamic inputs to a hydra sketch is using custom funcitons. This can be useful for creating parameters that change over time, as well as creating sketches that respond to external inputs, such as the mouse, audio input, or MIDI controllers.

Each numerical parameter in hydra can be defined as a function rather than a static variable.  When Hydra takes a function as an argument,it will evaluate it every time that function every time it renders a frame. The return of the function will be used as the value for that parameter during that frame render. For example,
```javascript
osc(function(){return 100 * Math.sin(time * 0.1)}).out()
```
modifies the oscillator frequency as a function of time. (Time is a global variable that represents the milliseconds that have passed since loading the page). 

The above example can be written more concisely using es6 syntax:

```javascript
osc(() => 100 * Math.sin(time * 0.1)).out()
```

```hydra
voronoi(5,.1,()=>Math.sin(time*4))
	.out()
```

The `time` variable seen there is a variable pre-declared by Hydra, that stores how much time passed since Hydra started in seconds.

Functions used in Hydra don't need to be arrow functions, any no-argument function will do! Make sure your function is returning a Number to avoid errors.

### The time variable

When you use functions that can take numerical arguments, `time` will allow you to have their values evolve through... time. If you multiply time by some value it's as if time goes faster, while dividing while act as making time go slower. For example `Math.sin(time*4)` will go 4 times faster than `Math.sin(time)`.

Those users more familiar with mathematics might see this as:

* `y(t) = t` : `()=>time`
* `y(t) = A sin(f t + ph)` : `()=>amplitude*Math.sin(freq*time + phase)`

We recommend getting familiar with some of the methods in the JS built-in `Math` object. Learn more about it [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)

