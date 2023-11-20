---
title: "Using hydra with other javascript libraries"
---
# Using hydra with other javascript libraries

Hydra is written in javascript, and compatible with many other javascript libraries. The hydra web editor executes javascript directly in the browser, so it is possible to load many other libraries and scripts directly in the browser. 

## p5.js
[p5.js](https://p5js.org/) is a JavaScript library for creative coding, with a focus on making coding accessible and inclusive for artists, designers, educators, beginners, and anyone else! p5.js is pre-loaded on the Hydra editor with a wrapper that makes it easier to use inside the website. The wrapper is a class called `P5` (notice the upper-case P). A p5.js sketch can be used as a source within a hydra sketch, and vice versa.

### p5 + hydra example

```javascript
// Initialize a new p5 instance It is only necessary to call this once
p5 = new P5() // {width: window.innerWidth, height:window.innerHeight, mode: 'P2D'}

// draw a rectangle at point 300, 100
p5.rect(300, 100, 100, 100)

// Note that P5 runs in instance mode, so all functions need to start with the variable where P5 was initialized (in this case p5)
// reference for P5: https://P5js.org/reference/
// explanation of instance mode: https://github.com/processing/P5.js/wiki/Global-and-instance-mode

// When live coding, the "setup()" function of P5.js has basically no use; anything that you would have called in setup you can just call outside of any function.

p5.clear()

for(var i = 0; i < 100; i++){
  p5.fill(i*10, i%30, 255)
  p5.rect(i*20, 200, 10,200)
}

// To live code animations, you can redefine the draw function of P5 as follows:
// (a rectangle that follows the mouse)
p5.draw = () => {
  p5.fill(p5.mouseX/5, p5.mouseY/5, 255, 100)
  p5.rect(p5.mouseX, p5.mouseY, 30, 150)
}

// To use P5 as an input to hydra, simply use the canvas as a source:
s0.init({src: p5.canvas})

// Then render the canvas
src(s0).repeat().out()
```
### breakdown of p5.js functions in hydra
```javascript
p1 = new P5() // first, load p5 in instance mode
```

You can also specify some settings:

```javascript
p1 = new P5({width: 512, height: 512, mode: 'P2D'})
```

Now the p5 canvas is overlaying the Hydra canvas. You can hide it by running:

```javascript
p1.hide() // p1.show() to revert
```

And you may load it to a source to use p5's canvas as one:

```javascript
s0.init({src: p1.canvas})
```

### p5.setup()

When live coding, the `setup()` function of p5 has basically no use; anything that you would have called in setup() you can simply call outside of any function. For example:

```javascript
p1.noStroke()
p1.fill(255, 0, 100)
```

### p5.draw()

Now, to set a `draw` loop simply use all the functions and variables you are used to on global p5 from the variable you're using:

```javascript
p1.draw = () => {
    p1.fill(p1.mouseX/5, p1.mouseY/5, 255, 100)
    p1.rect(p1.mouseX, p1.mouseY, 30, 30)
}
```

### Livecoding

You can technically call any p5 function while livecoding. So you can draw anything onto screen on evaluation instead of using the `draw` loop.

```javascript
p1.clear()

for(let i = 0; i < 50; i++)
    p1.rect(20, 20, p1.width/50*i, p1.height/50*i)
```

### Using Hydra's render loop

You can stop p5's own looping and do your p5 actions inside Hydra's render loop via the `update` function. This will synchronize p5's and Hydra's frame renders.

```javascript
p1.noLoop();

p1.clear()
p1.colorMode(p1.HSB)
p1.stroke(0)
p1.strokeWeight(1)

src(o0)
	.scale(1.05)
	.blend(src(o0).brightness(-.02),.4)
	.modulateHue(o0,100)
	.layer(s0)
  	.out()

p1.draw = () => {
  	if(p1.random() < 0.01) p1.clear()
    p1.fill(time*100%200, 70, 100)
    p1.rect(p1.random()*p1.width, p1.abs(p1.sin(time*2))*p1.height, 50, 50)
}

update = (dt)=> {
    p1.redraw();
}
```

You could also use shape drawing functions such as `rect` directly inside `update`, but you'll need to take into account the coordinate system won't be reset automatically if modified, like when using `draw`. So you'll have to reset it manually by putting actions between `push()` and `pop()`. This would also stop the `frameCount` increment. 

### Note on using different frame rates

There are many situations where you can save resources by using a very low frame rate on p5 and a high one on Hydra or vice-versa. For example, if you want to place random shapes on the p5 canvas every second, you can set p5's `frameRate` to 1 and leave Hydra's fps undefined.

TO DO: add example of using hydra as texture in p5

## Loading external scripts
The `await loadScript()` function lets you load other packaged javascript libraries within the hydra editor. Any javascript code can run in the hydra editor.

## THREE.js
Here is an example using Three.js from the web editor:
```javascript
await loadScript("https://threejs.org/build/three.js")

scene = new THREE.Scene()
camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)

renderer = new THREE.WebGLRenderer()
renderer.setSize(width, height)
geometry = new THREE.BoxGeometry()
material = new THREE.MeshBasicMaterial({color: 0x00ff00})
cube = new THREE.Mesh(geometry, material);
scene.add(cube)
camera.position.z = 1.5

// 'update' is a reserved function that will be run every time the main hydra rendering context is updated
update = () => {
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  renderer.render( scene, camera );
}

s0.init({ src: renderer.domElement })

src(s0).repeat().out()
```

## Tone.js
And here is an example loading the Tone.js library:
```javascript
await loadScript("https://unpkg.com/tone")

synth = new Tone.Synth().toDestination();
synth.triggerAttackRelease("C4", "8n");
```

## Custom libraries
In the Hydra editor, you can load any external scripts, libraries or hydra-synth extensions using the following syntax at the top of your sketch:

```javascript
await loadScript("https://www.somewebsite.com/url/to/hydra-script.js")
```


You can also suffer from [CORS policy problems](./external_sources.md#cors-policy) if the script/package you're loading doesn't come from a CDN. If you want to load from a GitHub or GitLab repo, you can use special CDNs like `statically.io`.

----
contributors: geikha, olivia
