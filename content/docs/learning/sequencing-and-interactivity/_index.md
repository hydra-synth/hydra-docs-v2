---
title: "Sequencing & interactivity"
draft: false
weight: 4
# bookCollapseSection: true
---

# Sequencing and Interactivity

 If you're coding in Hydra, you're constantly trying many values to input to the sources and transforms, and it's just a matter of time until you like how more than one looks, and you want to somehow switch between them. We'll be referring to this idea of arguments whose value change over time as dynamic arguments. And there are two main ways to achieve this in Hydra: Arrays and functions. 

## Sequencing using Arrays

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

<!-- ```hydra
bpm = 50
arr = [0,0.8,2]
osc(50,.1,arr.smooth())
	.rotate(arr.fit(-Math.PI/4,Math.PI/4).smooth())
	.out()
```

Try smoothing some of the above examples and see what happens! -->

<!-- ##### Easing functions

The default interpolation used by Hydra on an Array that called `.smooth` is linear interpolation. You can select a different easing function as follows:
```hydra
shape([3, 4, 5, 6, 7, 8].smooth().ease('easeInQuad'))
	.out()
```


```hydra
bpm = 50
arr = [0,0.8,2]
osc(50,.1,arr.ease('easeInQuad'))
	.rotate(arr.fit(-Math.PI/4,Math.PI/4).ease('easeOutQuad'))
	.out() // try other easing functions !
```

The following are the available easing functions:

* linear: no easing, no acceleration
* easeInQuad: accelerating from zero velocity
* easeOutQuad: decelerating to zero velocity
* easeInOutQuad: acceleration until halfway, then deceleration
* easeInCubic
* easeOutCubic
* easeInOutCubic
* easeInQuart
* easeOutQuart
* easeInOutQuart
* easeInQuint
* easeOutQuint
* easeInOutQuint
* sin: sinusoidal shape
--- -->

## Custom Functions

Each numerical parameter in hydra can be defined as a function rather than a static variable. For example,
```javascript
osc(function(){return 100 * Math.sin(time * 0.1)}).out()
```
modifies the oscillator frequency as a function of time. (Time is a global variable that represents the milliseconds that have passed since loading the page). 

The above example can be written more concisely using es6 syntax:

```javascript
osc(() => 100 * Math.sin(time * 0.1)).out()
```
<!-- The other main way of adding dynamic inputs to your sketches is passing functions as arguments. When Hydra takes a function as an argument, what it will do is evaluate it every time it renders a frame. The return of the function will be used as the value for that parameter during that frame render. So you can use a function to simply keep track of a value that you know will change over time, for example, mouse position (which we'll see later). -->

 Custom functions are especially useful for controlling hydra parameters using external inputs, such as the microphone, mouse, or midi control. 


<!-- #### Changing the global speed

You can either slow down or fasten the rate at with `time` increases via changing the `speed` variable:

```javascript
speed = 1  // default
speed = 2  // twice as fast
speed = .5 // half as fast
speed = 0  // freezed 
``` -->



---
by geikha