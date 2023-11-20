---
title: Drawing an oscillator
weight: 2
---

## First line of code

Use the ***clear all button*** <img src="https://i.imgur.com/zQLjhBs.png" alt="drawing" width="40" style="display:inline;vertical-align:middle;"/>
to erase the previous sketch.

Then, type or paste the following in the editor:
```javascript
osc().out()
```
Press the ***run button***  <img src="https://i.imgur.com/sm5d3VX.png" alt="drawing" width="40" style="display:inline;vertical-align:middle;"/> to run this code and update the visuals on the screen. You should see some scrolling stripes appear in the background. You can also edit the code directly on this page:

```hydra
osc().out()
```

This creates a visual oscillator. Try modifying the parameters of the oscillator by putting a number inside the parentheses of `osc()`, for example ```osc(10).out()```.

Re-run the code by pressing the ***run button*** again, and seeing the visuals update. Try adding other values to control the oscillator's `frequency`, `sync`, and `color offset`.

```hydra
osc(5, -0.126, 0.514).out()
```

An oscillator is a type of **source** in hydra. For a complete list of available sources, see [the interactive function reference](./../../../reference) or [the list of available sources](./../../../reference/api/src).
*Trick: you can also use the keyboard shortcut **‘ctrl + shift + enter’** to have the same effect as the run button.*