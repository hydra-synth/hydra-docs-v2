---
title: Array
date: 2023-11-16
---
### Arrays

#### Sequence your inputs

When you send an Array as an input, Hydra will automatically switch and jump from each element from the Array to the next one. When there are no more elements, it wraps all the way back to the beginning. Let's see it in action:

```hydra
osc([20,30,50,60],.1,[0,1.5])
	.out()
```

As you can see, the fact that both these Arrays have a different amount of values doesn't matter, Hydra will take values from each element of any Array for the same amount of time by default.

The Arrays can be passed in any way, you may have a variable that stores an Array and use its name within your sketches (not recommended in some scenarios, more info below), you may create a function that returns Arrays and use that to automatically generate discrete sequences of values:

```hydra
randomArray = (l=12)=> Array.from({length: l}, Math.random);
gradient()
	.hue(randomArray())
	.out()
```

#### Changing the global bpm for Arrays

To change how rapidly Hydra switches from element to element of all Arrays, you can change the `bpm` variable (meaning beats per minute) to any value you desire:

```hydra
bpm = 138 // change me !
randomArray = (l=12)=> Array.from({length: l}, Math.random);
gradient()
	.hue(randomArray())
	.out()
```

The default value for `bpm` is 30.

When livecoding visuals at the same time that music is playing, it can be useful to have a tapping metronome opened to keep track of the BPM being played and set this variable as such.

#### Changing the speed of a specific Array

Hydra adds a couple of methods to all Arrays to be used inside Hydra. `.fast` will control the speed at which Hydra takes elements from the Array. It receives a Number as argument, by which the global speed will be multiplied. So calling `.fast(1)` on an Array is the same as nothing. Higher values will generate faster switching, while lower than 1 values will be slower.

```hydra
bpm = 45
osc([20,30,50,60],.1,[0,1.5].fast(1.5)) // 50% faster
    //.rotate([-.2,0,.2].fast(1)) // try different speeds for each array
	.out()
```

#### Offsetting the timing of an Array

Another one of the methods Hydra adds to Arrays, allows you to offset the timing at which Hydra will switch from one element of the Array to the next one. The method `.offset` takes a Number from 0 to 1.

```hydra
bpm = 45
osc([20,30,50,60],.1,[0,1.5].offset(.5)) // try changing the offset
	.out()
```

#### Fitting the values of an Array within a range

Sometimes you have an Array whose values aren't very useful when used as input for a some Hydra function.
Hydra adds a `.fit` method to Arrays which takes a minimum and a maximum to which fit the values into:

```hydra
bpm = 120
arr = ()=> [1,2,4,8,16,32,64,128,256,512]
osc(50,.1,arr().fit(0,Math.PI))
	.scale(arr().fit(1,2))
	.out()
```

#### Interpolating between values

You can also interpolate between values instead of jumping from one to the other. That is, smoothly transition between values. For this you can use the `.smooth` method. It may take a Number argument (defaulted to 1) which controls the smoothness.

```hydra
bpm = 50
arr = [0,0.8,2]
osc(50,.1,arr.smooth())
	.rotate(arr.fit(-Math.PI/4,Math.PI/4).smooth())
	.out()
```

Try smoothing some of the above examples and see what happens!

##### Easing functions

The default interpolation used by Hydra on an Array that called `.smooth` is linear interpolation. You can select a different easing function as follows:

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

#### Note on storing Arrays on variables / functions

Storing an Array in a variable can lead to some trouble as soon as you apply some of the just-mentioned functions to it. Since Arrays are Objects, each time you call your variable, you'll be calling the same Object. If you apply some speed via `.fast` or smoothness via `.smooth` somewhere in your patch, and then use the same variable, all the following uses of the Array will also have these effects applied to them. For example

```hydra
arr = [1,2,3]
osc(30,.1,arr.smooth())
	.rotate(arr)
	.out()

arr2 = () => [1,2,3]
osc(30,.1,arr2().smooth())
	.rotate(arr2())
	.out(o1)

render()
```

#### Note on Arrays and textures

Note that the following will not work:

```javascript
solid(1,.5,0)
	.diff([osc(),noise()])
	.out()
```

Hydra can't handle Arrays of textures. You can work around it in some ways:

```hydra
solid(1,.5,0)
	.diff(osc().blend(noise(),[0,1].smooth()))
	.out()
```

Unfortunately, if you want to use many textures this solution doesn't really apply.

Users of Hydra have come up with some experimental solutions which might come in handy in some scenarios, but they come with some drawbacks:

```javascript
// blending method, heavy GPU load.
// every element from the array will be rendered even if not shown.
// allows for blending between elements.

select = function(arr,l=0){
	const clamp = (num, min, max) => Math.min(Math.max(num, min), max)
	const blending = (l,i)=> (clamp(l-(i-1),0,1))
	const isFunction = (typeof l === 'function')
	return arr.reduce((prev,curr,i)=>
		prev.blend(curr, isFunction ? ()=>blending(l(),i) : blending(l,i))
	)
}
textures = [noise(), osc(), voronoi(), gradient()]
select(textures,()=>Math.floor(mouse.x/innerWidth*4))
    .out()
```

```javascript
// re-compiling method, heavy CPU load. 
// it reserves an output for the switching. 
// can't blend between elements. 
// each time an element switches the shader must be recompiled

osc(20)
  	.rotate()
  	.modulate(o3,.2)
	.out()

textures = [noise(), osc(), voronoi(), shape()]
index = 0
tex = textures[index]
update = (dt)=> {
	if(time % (60 / bpm) * 1000 < dt){
		index++; index %= textures.length;
		tex = textures[index]
		tex.out(o3)
    }
}
---
```

## available methods for Arrays

### fast
fast( speed = 1 )

```hydra
// default
osc([10,30,60].fast(2),0.1,1.5).out(o0)
```

### smooth
smooth( smooth = 1 )

```hydra
// default
shape(999).scrollX([-0.2,0.2].smooth()).out(o0)
```

### ease
ease( ease = 'linear' )

```hydra
// default
shape(4).rotate([-3.14,3.14].ease('easeInOutCubic')).out(o0)
```

### offset
offset( offset = 0.5 )

```hydra
// default
shape(999).scrollY(.2).scrollX([-0.2,0.2])
  .add(
  shape(4).scrollY(-.2).scrollX([-0.2,0.2].offset(0.5))
  ).out(o0)
```

### fit
fit( low = 0, high = 1 )

```hydra
// default
shape().scrollX([0,1,2,3,4].fit(-0.2,0.2)).out(o0)
```

