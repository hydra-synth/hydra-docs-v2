---
title: javascript for hydra users
---

# JavaScript Guide

This guide is made for users who are new to JavaScript or coding in general and would like to dive into these topics. You don't need to fully understand what's here to use Hydra. If you're just starting with Hydra and you have no coding experience, we recommend you experiment with Hydra a bit before reading this.

## Comments

```javascript
// This is a one line comment
```

Most programming languages have implemented in them a feature commonly referred as comments. These are ways to write annotations into your code without having the machine interpret them as code. JavaScript, the scripting language that Hydra works on, has implemented comments in the same tradition as many other languages such as Java or C. You use `//` for single line comments, and you can use `/* ... */` for multi-line comments.

```javascript
/*
    An example of a multi line commentary:
    This sketch shows an oscillator:
*/
osc().out()
```

You can also write comments at the end of lines of code too, which is very useful while annotating what's going on with your visuals sometimes:

```javascript
noise(2,.5)
	.diff(src(o0).rotate(Math.PI/4)) // rotating by 45°
 	.thresh(.5)
	.color(1,.1,.3) // pink color
	.out()
```

You will surely find useful sometimes to "comment in and out" some lines of code to see how it affects the visuals, or simply to understand what each line of code does. By adding a `//` at the start of a line you can comment it out and see how some sketch would look like without a given transform without having to delete the original line.

```javascript 
noise(2,.5)
	.diff(src(o0).rotate(Math.PI/4)) // rotating by 45°
 	//.thresh(.5)
	.color(1,.1,.3) // pink color
	.out()
```

---

## Variables

Variables are spaces of memory in your computer that you reserve to store some value. Each variable you use will have a unique symbolic name. This definition may sound complicated, but you'll see it's really as intuitive as it can be. You may remember variables from mathematics being letters that represent some sort of number. This is precisely the same, you just choose some name and assign some number (or other type of information) to it.

```hydra
freq = 50 // change this value and see what happens
osc(freq)
	.diff(osc(freq).rotate())
	.out()
/*
	the above is equivalent to:
    osc(50)
	.diff(osc(50).rotate())
	.out()
*/
```

In the previous example, `freq` is the name of the variable and `50` is its value.

Variable names can't start with numbers, they start with letters and it's conventional in JavaScript to start with a lowercase. When the name of your variable is more than one word, it's also conventional to write them as such:

```hydra
feedbackIntensity = .3
src(o0)
	.colorama(feedbackIntensity/10)
	.scale(.96)
	.layer(noise().luma(.1))
	.out()
```

However this is just a convention, you may find other ways of naming your variables more useful. You may even like to use only one letter variables (such as `x`, `y`, etc), it's faster to code but harder for others to understand. Find your own balance and style.

### Global variables

When you declare a variable in Hydra, it declares it for you on the global scope. You can imagine a [scope](https://developer.mozilla.org/docs/Glossary/Scope) as a piece of code that works on its own and has its own variables. However, the global scope is basically a bunch of variables and functions that can be accessed from anywhere (functions such as `osc()` are declared in the global scope so that you can use them by just calling them, no matter where, for example).
We can make it explicit that we want something on the global scope. In JavaScript, since it's made to run on a browser, we do this by declaring variables on the `window` object (what is an object, you can find out below), which represents the browser's window.

```javascript
window.globalVariable = 21.4

osc(globalVariable).out()
```

However, you can drop the `window.` part since the default behavior is the same:

```javascript
globalVariable = 21.4

osc(globalVariable).out()
```

If you see JavaScript code elsewhere you'll surely see the keywords `let` or `const`. These define variables on their scope. So avoid them if you want to declare variables that can be freely used while livecoding.

```javascript
let scopedVariable = 21.4
// this will only work if evaluated on the same block
osc(scopedVariable).out()
```

This knowledge will come in handy if you start coding functions for example, since each function has its own scope, and if you want to declare something on the global scope, you'll have to be explicit about it.

---

## Arrays

[Arrays](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array) are basically lists of values. Instead of declaring 100 variables to represent different values of the same concept you can just use a list of values. The key thing is these values are related, they will serve the same purpose somewhere in our code. If they are not related, using a list isn't really useful, we'll be just confusing ourselves thinking about where in the list did we put this or that other value. Here's an example of an Array:

```hydra
rots = [0,.5,1.2,2.2,3] // this is how we declare an array
osc().rotate(rots[0]) // this is how we call an element from an array
	.diff(osc().rotate(rots[2]))
	.out()
```

The example above isn't that useful in a Hydra context, but we hope it illustrates the basics of how an Array is created and used.
Arrays in JavaScript (and in most programming languages) start counting their elements from 0 and not from 1. So if you want the first element of the `rots` Array, you need to call `array[0]` instead of `array[1]`! Same goes for every element. If you want the third element call `array[2]`, and so on. Remember that `nth element = array[n-1]`


### Arrays as sequences

Arrays in Hydra can be used as inputs. Hydra takes the list of values and makes a sequence out of them:

```hydra
rots = [0,.5,1.2,2.2,3]
osc().rotate([-.5,.5])
	.diff(osc().rotate(rots))
	.out()
```

You can learn more about dynamic inputs [here](/guides/advanced#dynamic-inputs).

---

## Functions

A function is similar to a variable in the sense that you're going to give it its own name and call it multiple times later. The difference being that functions do not store values, they store pieces of code that -usually- return some value. 
You can see them as little boxes where you put something in and they spit something out.
Functions will help you not to repeat your code multiple times, sometimes you'll see you can write a function that spits out what you need instead of rewriting it many times.

### Defining functions

There are multiple ways to define functions in JavaScript, here's an example of a function named `sum` that takes two numbers called `a` and `b` and returns (spits out) the sum of both numbers:

```javascript
function sum(a,b){
	return a+b;
}

sum = function(a,b){
	return a+b;
}

sum = (a,b) => a+b
```

We'll be sticking with the last form of defining functions, usually called 'arrow function'. It is worth noting the first form it's a bit like using `let` and `const` for variables, they work on their own scope.

#### Local variables in functions

Talking about scope, you may want to define variables inside your functions, which are local to the functions and aren't variables that should be used globally. Now the `let` keyword becomes useful.

```javascript
sum = function(a,b){
    let result = a+b;
	return result;
}

sum = (a,b) => a+b
```

### Functions that return a texture

```hydra
// works properly on the editor
circle = () => shape(64,.4,.1).scale(1,innerHeight/innerWidth)
circle()
	.out()
```

Now you may be thinking "Wait, shouldn't this simply be a variable that stores that texture? If there's no input what's the use of having a function here?". And in a way you would totally be right. Except for the fact that if you use a variable to store that shape, you'll be always referring to the exact same object that represents that shape. If you use a variable `circle` and apply some transforms to it somewhere in your patch, and try to use circle again later, all the transforms that you applied will be there! Because you applied those to that object precisely. Also, even if you don't apply any transforms, JavaScript can be very messy when referencing the same object multiple times in some situations. So, if you use a function, each time you call it a new object representing that texture will be created.
Another reason we would use a function in this example is that if we want to add some input to this function, well, it's already a function so we can do it.

Let's see how we could make the circle function more useful by adding parameters:

```hydra
// works properly on the editor
circle = (size,blur=.1)=> shape(64,size,blur)
                        .scale(1,()=>innerHeight/innerWidth)
circle(.4)
	.diff(circle(.2,.6))
	.out()
```

Now, each time we call the circle function we can specify a size and blur. We can also omit the blur and the function will use the default value specified next to it. 
We also changed the scaling to an arrow function, which you may find surprising if you haven't seen it before. When you use a function as an argument, Hydra will evaluate that function every time it renders a frame and use the return of that function in the rendering of that frame. In other words, functions can be used as [dynamic inputs](/guides/advanced#functions).

### Using declared functions as inputs

As we just mentioned, we can use arrow functions inside the arguments of a given source or transform for it to react in real time. If you have many arguments using the same arrow function, you may want to declare it and reuse its name:

```hydra
scaling = () => .9+(Math.sin(time*2)/3)
noise(3)
	.scale(scaling)
	.layer(
		src(o0)
			.scale(scaling)
			.mask(shape())
		)
	.out()
```

#### Calling declared functions from other functions

Sometimes you want to reuse a function but have something change about it. For example, maybe we want to make the scaling negative for the feedback in the last example. But calling `-scaling` doesn't make sense, at least to JavaScript, since the negative of a function doesn't exist. But the negative of its return does:

```hydra
scaling = () => .9+(Math.sin(time*2)/3)
noise(3)
	.scale(scaling)
	.layer(
		src(o0)
			.scale(()=>-scaling())
			.mask(shape())
		)
	.out()
```

##### Note on functions with parameters

You'll also come across this if your function has parameters. For example:

```javascript
scaling = (multiplier)=> (.9+(Math.sin(time*2)/3))*multiplier
```

Doing `.scale(scaling)` doesn't make sense anymore, since you aren't giving it its necessary input. And if you try to do `.scale(scaling(-1))`, Hydra will evaluate the function once and use its return as the input to scale, instead of using a function which is what we want for the visual to react to the changes in time. The solution is, again, a function that calls your function, such as `.scale(()=>scaling(-1))`. If for some reason you hate arrow functions, you could also try [binding it](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) doing `.scale(scaling.bind(0,1))`.

### Higher-order functions

Higher order functions just means functions that take other functions as arguments. These are useful when you want to make u new functions which take behavior from other functions. As an example, let's visualize applying a sine function (with some tweaks) to itself:

```hydra
fps=40
twice = (func,...args) => func(func(...args))
myFunc = (x)=> Math.sin(x*2)*2

src(o0)
	.luma(.02)
	.mult(solid(),.02)
	.add(
		shape(32,.02)
			.scroll(0, ()=>0.25+myFunc(time)/50, -.2)
		)
	.add(
		shape(32,.02)
			.scroll(0, ()=>-.25+twice(myFunc,time)/50, -.2)
		)
	.out()
```

That new `...args` thing simply takes all the arguments sent to a function, we use it so we can call whatever function sent with as many arguments as it needs.
Take into account `twice(myFunc,time)` is the same as `myFunc(myFunc(time))`, and you may prefer to write the latter in many occasions. But you can also send an arrow function to `twice`, which could save you declaring a functions you may only want to use once, or writing the same declaration twice.

---

## Objects

You can imagine an object as a special variable, which instead of containing a value, it contains other variables and functions. The former are usually called properties of an object and the latter are methods of an object.
For example, if you ever use Hydra on instance mode, what you'll come across is Hydra as a special object containing all the functions you know and love, instead of having them on the global scope.

Let's see an example of how to declare and use an object with some properties:

```hydra
coords = {x: 0.25, y: 0.25} // this is how we declare an object with some properties
shape(8,.1)
	.scroll(coords.x,coords.y) // this is how we call properties
	.out()
```

And now let's add a method:

```hydra
coords = {
    x: 0.25, 
    y: 0.25,
    inverted: function(){
        newCoords = {x: -this.x, y: -this.y}
        return newCoords
        }
}
shape(8,.1)
    .scroll(coords.inverted().x,coords.inverted().y)
    .out()
```

There's a new keyword that we hadn't seen before here: `this`. The `this` keyword is used in methods (functions of an object) to refer to the object from which the method is called.

Objects can also be conceptualized as dictionaries, with keys and values. The keys would be the names of the properties (and methods) of the object and the values is what they store:

```javascript
numbers = {
	pi: 3.14159265359,
	e: 2.71828182846,
	golden: 1.61803398875
}
numbers['pi'] // another way we can call keys from an object
```

### Useful properties in the window object

The window object has lots of information about the environment that our visuals run on. You'll see lots of Hydra sketches that make use of them, more commonly for example, the `innerWidth` and `innerHeight` properties. These properties store the respective width and height that the webpage occupies on your screen.

For example, we can calculate the ratio between height and width to have perfect squares on our sketches:

```javascript 
// this example will only work on the editor or atom-hydra
screenRatio = innerHeight/innerWidth
shape(4,.4).scale(1,screenRatio)
	.out()
```

There's also the less used `screenX` and `screenY` which will tell you the position of the window relative to the full screen. 
Try to move your browser's window with the following example:

```hydra
osc()
	.rotate(()=>20+(screenX/300))
	.out()
```

---

## The Math Object

You have surely seen many examples in Hydra and in these tutorials that make use of mathematical functions such as the sine wave. You may have also noticed that each time one of them is used, they're written as `Math.somefunction()`. The reason for this is that all these very useful functions are taken from a special object called `Math` that is present in practically every JavaScript implementation. 
You can see the full list of functions and variables in the Math object [clicking here](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Math).

### Math.PI

One of the most useful predefined variables that the Math API has is the value of pi (well, an approximation considering that pi has infinite decimals). Many Hydra functions take radians as arguments which you may know are usually represented using multiples of pi. For example, if you want to rotate a texture exactly half a pi (90 degrees), you can do it as such:

```hydra
osc().rotate(Math.PI/2).out()
```
---
by geikha