# frames and timing
by geikha

## Using the update function

There's a function in the Hydra API called `update`. This function runs at the beginning of every frame render right before the values for `time` and `a.fft` are calculated. If you are familiar with Processing or p5, you can think of `update` as Hydra's equivalent to the `draw` function.
Using `update` can be very useful for creating generative visuals (generative in the sense of controlling visual elements with values that evolve through time either randomly or following certain rules). It can also be used to have a finer and/or connected control over parameters when compared to using simple arrow functions as arguments.
It is also worth noting that the function receives a `dt` argument, which contains the delta time elapsed between the rendering of the previous frame and the current one. You may or may not want to use this to control your visuals (most of the examples don't use it, actually).

### Examples

#### Using `update` and `time` to have complex control of parameters through time

```hydra
inv = 0; r = 0;
osc(30,.1,()=>-inv)
	.invert(()=>inv)
	.diff(osc(30).rotate(()=>r))
	.out()

update = (dt) => {
	inv = time % 12 < 6 ? Math.abs(Math.sin(time)) : time*2 % 2
	r += Math.sin(time/2) * (time%2.5) * 0.02
  	r -= Math.cos(time)*0.01
}
```

The structure `time % every < duration` is super useful to make stuff happen `every` certain amount of seconds, for a given `duration` (also in seconds).
`time % wavelength / wavelength` can be interpreted as a sawtooth wave with a given wavelength. This would generate an ascending sawtooth going from 0 to 1 in the amount of time specified by whe wavelength. For a descending one you can write something like `1-(time % wavelength / wavelength)`. If you want values from 0 to `wavelength` just remove the division.

#### Adding a frame counter to make frame-specific actions

Having something appear for only one frame can be super useful in many feedback-based sketches:

```hydra
toggle = 0; rotation = .01
src(o0)
    .scale(1.017)
    .rotate(()=>rotation)
    .layer(
        osc(10,.25,2)
            .mask(shape(4,.2))
            .mult(solid(0,0,0,0),()=>1-toggle)
    )
    .out()

frameCount = 0
update = (dt) => {
    toggle = 0
    if(frameCount % 120 == 0){
        toggle = 1; rotation *= -1;
    }
    frameCount++
}
```

#### Randomly evolving values through time (Random walker)

```hydra
x = 0; y = 0;
t = ()=> solid(1, 1, 1, 1).mask(shape(3, .05, .01).rotate(Math.PI))
src(o0)
	.blend(osc(8,.1,.2).hue(.3),-.015)
  	.scale(1.01)
  	.rotate(.01).mult(solid(0,0,0),.006)
	.layer(t().scroll(()=>x,()=>y))
	.layer(t().scroll(()=>-x,()=>-y))
	.out()
update = (dt)=> {
	x += (Math.random()-.47)/100
	y += (Math.random()-.47)/100
}
```

### `update` vs Arrow functions

Every function that you use as an argument is evaluated right before the current frame is about to be rendered. Which is the same thing that happens with the `update` function! This means, unless we use `dt`, everything we can do on `update` we can technically do on argument functions. It's up for us to decide when one's better than the other. If we are controlling many interconnected variables and procedures, most probably, an arrow function or a named function won't be that nice to use. It'll be confusing as to why a function which is supposed to represent a simple dynamic argument is doing so much stuff inside of itself. Maybe we could separate the different behaviors into many arrow functions. But if these functions were to feed from each other, this will yet again get confusing quite rapidly. Even then, there are many scenarios where an arrow function can do the same work as the update function with less code. For example, here's a patch where a circle chases your mouse:

```hydra
// works well only on editor
x = () => (-mouse.x/innerWidth)+.5
y = () => (-mouse.y/innerHeight)+.5
posx = x(); posy = y();
shape(16,.05)
	.scrollX(() => posx += (x() - posx) / 40)
	.scrollY(() => posy += (y() - posy) / 40)
	.add(o0,.9)
	.out()
```