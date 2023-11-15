---
title:  Using iteration and conditionals to create patches
---

## Using iteration and conditionals to create patches
---
by geikha

##### Note

For this tutorial we'll be assuming you've already learned by your own means what iteration and conditionals are in a programming context.

### Iteration : automatically generate patches

As you may know from regular programming, or other creative coding environments such as p5, iteration helps us repeat some operation(s) many times to achieve a specific goal. Maybe you would like to layer many similar objects but with slightly different values, and you want so many of them that writing each one manually isn't desirable. Maybe you want to have some form of very specific feedbacks, etc.
Let's jump straight into some examples.

#### for loops

For loops that generate patches can be used inside or outside functions, but we will be sticking with the latter for convenience.

The typical structure of a patch-generating for loop is as follows:

```javascript
someFunction = (iterations) => {
	accumulator = osc(); // first part of the patch, a source
  	for(i=1; i<iterations; i++){ // i is also called a "counter`
    	accumulator.someTransform(i);
    }
  	return accumulator;
}
someFunction(5).out()
```

Of course this is just a useful example, and your code may end up looking very different depending on what crazy ideas you want to try. But let's use this as a starting point. See how the use of a function allows us to reuse this iterative process with different parameters such as the amount of iterations. Also note how we start the counter variable `i` on the value `1` instead of the typical `0`. Since `0` will usually null an effect, the result will be equal to the first value assigned our accumulator, so we can skip the `0` iteration altogether. For those not familiar with the abbreviation `i++`, this basically means `i+=1`, which means `i = i+1`.

##### Example: rotating

```hydra 
// 'nest' is only a creative, arbitrary name
nest = (freq,div) => {
    nest = osc(freq,.02); step = Math.PI*2/div
    for(r = step; r<Math.PI*2; r+=step)
            nest.diff(osc(freq,.02).rotate(r))
    return nest;
}
nest(30,4).out()
```

Here we want to see how it would look like if we grab an oscillator of a given `freq` frequency, and calculate the `diff` between other rotated oscillators of the same frequency. To achieve that, we define our accumulator `nest` with the initial value of `osc(freq,.02)`. Then, we define a `step` which will be how many radians the oscillator will rotate. We calculate this as a division of 2pi (a full 360° turn) by some `div` number. Then we iterate over `nest`, applying the `diff` and the effect respectively, and adding a `step` to our counter `r` each iteration.

##### Example: very specific feedback

```hydra
feedbacks = (q) => {
    result = noise(4).luma(-.1);
    for(i = 1; i<q; i++)
        result.modulate(src(o0).scale(1/(i)),.03*i)
    return result;
}
feedbacks(5)
    .out()
```

##### Example: layering varying circles

```hydra
screenRatio = () => 1 // innerHeight/innerWidth // this only makes sense in the editor
circle = () => shape(64,.9,.1).scale(1,screenRatio).luma().color(1,.1,.2)
tunnel = (q=5) => {
    tunnel = circle();
    for(i=1; i<q; i++){
        nextCircle = circle()
                        .invert(i%2) // inverts every other iteration
                        .hue(.01*i)
                        .scale(0.85**i);
        tunnel.layer(nextCircle);
    }
    return tunnel;
}
tunnel(8)
    .out()
```

Try adding or changing the transforms that happen to every `nextCircle` and see how drastically yet easily they can change the visuals. Specially using transforms like `repeatX`.
Still, always keep in mind while using iteration, that the more effects and iterations you add, the heavier the sketch will be to process.

#### .forEach, .map and .reduce

Those familiar with more array focused programming languages such as Python or Haskell, or more functional structures even inside JavaScript, may be used to iterating using the `forEach`, `map` and/or `reduce` structures. Where given an Array, we use each value to alter something or to reduce the entire Array into a desired result.
Practically anything done with these functions can be done using `for` loops, so if you are new to these or you just don't like how they look, then there really is no need for you to learn these, even if you're super interested in iteration.

##### .forEach

Structures using `.forEach` are quite useful for those who'd like to generate patches from predefined data. Here's an example using the ASCII values of a given string:

```hydra
text = "Hydra :)"
arr = Array.from(text).map(char => {return char.charCodeAt()})
result = solid()
arr.forEach(ascii => {
	result.add(
		shape(4,.2,.5).luma(.6,.3)
      		.color(1,0,0).hue(ascii/100)
      		.scrollX(.4+ascii/100)
      		.rotate(Math.PI*ascii/100)
    	,.4)
})
result.out()
```
Try changing the text, and remember not to use very long strings given they will be quite heavy to process.

##### .reduce

Using `.reduce` is quite useful when you have an array of textures. Here's a simple example:

```hydra
vs = [noise(), osc(), voronoi(), gradient()];
result = () => vs.reduce((prev,curr)=> {
	return prev.diff(curr)
	},solid())
result().out()
```

##### .map 

Haters of state (non-political) will prefer `.map` any day over `.forEach`. Looking at the example for `.forEach`, we see were creating a texture and adding it to an accumulator _for each_ element in the Array. We can separate the texture generating part of the code and the blending part using `.map` to get an array of textures and `.reduce` to blend them:

```hydra
text = "Hydra >:]"
arr = Array.from(text).map(char => {return char.charCodeAt()})
arr = arr.map(ascii => 
	shape(4,.2,.5).luma(.6,.3)
      		.color(1,0,0).hue(ascii/100)
      		.scrollX(.4+ascii/100)
      		.rotate(Math.PI*ascii/100)
)
result = arr.reduce((prev,curr)=>prev.add(curr,.4),solid())
result.out()
```

### Conditionals

Conditionals aren't very useful on their own here, given all code execution on Hydra happens arbitrarily and manually via the interaction of the user. The only case you would want to use an `if` statement by its own while livecoding Hydra is that where you'd like some variable to change given some condition and only at the time of each code evaluation. But even still, you'll see that putting any conditionals inside functions will be the most useful approach because of code reusability and readability. Let's get to it.

#### Conditionals in functions

We know from previous tutorials we can make our own functions to be used as arguments of Hydra sources and transforms, and how Hydra evaluates these functions each frame. Here's an example where we use conditionals to have a hue change happen only during 3 seconds out of every 10 seconds:

```hydra
hueChange = () => {
    if(time%10 < 3)
        return time/2
    else
        return 0
}
osc(20,.1,2.6)
	.modulate(osc(20).rotate(Math.PI/2),.3)
	.hue(hueChange)
	.out()
```

Another common use of conditionals in programming is to avoid errors or undesired behaviors. Here's a simple example where we wrap the square root function from the Math API into our own `sqrt` function which turns any negative input into positive:

```hydra
sqrt = (n) => {
	if(n<0) n*=-1; //if negative, multiply by -1
  	return Math.sqrt(n)
}
noise(2)
	.diff(noise(2).scale(()=>sqrt(Math.sin(time/2))))
	.out()
```

#### The ternary operator

Before we go forward and use both iteration and conditionals, we'd like to show you the ternary operator. This operator can simplify many conditional operations. The syntax is the following:

```javascript
x = condition ? valueIfTrue : valueIfFalse;

// which is the same as
if(condition)
    x = valueIfTrue
else
    x = valueIfFalse
```

Now we can simplify the hue change example into:

```javascript
osc(20,.1,2.6)
	.modulate(osc(20).rotate(Math.PI/2),.3)
	.hue(()=> time%10<3 ? time/2 : 0)
	.out()
```

#### Conditionals inside iterations

Let's go back to a previous example, the nest, where we wanted to do many `diff` using the same oscillator many times with different angles of rotation. Here's a new version where we invert the colors of the first half of oscillators, and we apply colorama to the oscillator in every other iteration.

```hydra
// 'nest' is only a creative, arbitrary name
nest = (freq,div) => {
  	nest = osc(freq,.02); step = Math.PI*2/div
	for(i = 1; i<div; i++){
      r = i*step;
      nextOsc = osc(freq,.02).rotate(r);
      if(r<Math.PI) nextOsc.invert();
      nest.diff(nextOsc);
      if(i%2==0) nest.colorama(.06) 
    }
  	return nest;
}
nest(15,6).out()
```

The first change you'll notice is that now we're calculating the angle of rotation `r` inside the iteration, and for that we now use a regular counter such as `i`. We can get the exact same angle of rotation as before via multiplying the counter by the step. We do this specifically because if we want to have something happen every other condition, we'll need to know if the number of iteration we're in is even or not. This is what happens at `if(i%2==0)`.
However we still make use of `r` inside of the first conditional, `if(r<Math.PI)`. This will result in about half of the oscillators to be inverted, given Math.PI is half a turn.

---