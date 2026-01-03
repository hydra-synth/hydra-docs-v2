---
title: "web editor"
date: 2023-04-04T15:10:36+02:00
draft: false
author: "Flor and Olivia"
weight: 3
---

# web editor
Basic usage of the browser editor at [hydra.ojack.xyz](https://hydra.ojack.xyz)

---

## Keyboard shortcuts
* CTRL-Enter: run a line of code
* CTRL-Shift-Enter: run all code on screen
* ALT-Enter: run a block
* CTRL-Shift-H: hide or show code
* CTRL-Shift-F: format code using [Prettier](https://prettier.io/)
* CTRL-Shift-S: Save screenshot and download as local file.
* CTRL-Shift-G: Shares to Mastodon (not available right now, but you can still store sketches).

---

## Toolbar
At the right up corner you will find a toolbar with these buttons:
![](../../images/iCG8Lrq.png)
{.center}

1. **run all code** Runs all code on the page (same as typing *ctrl+shift+enter)
2. **upload to gallery** upload a sketch to Hydra's gallery and create a shorter URL
3. **clear all** resets the environment and clears text from the editor
4. **show random sketch**. Loads random sketch examples. Always it is a good way to learn Hydra by studying someone elses code.
5. **make random change** **dices** modify values automatically. Try it with some of the sketch examples.
6. **show info window** show overlay window with help text and links

---

## Save your sketches

### As a link

When you evaluate the entire code with the ***run button*** or with `shift + ctrl + enter`, Hydra automatically generates a URL that contains the last changes of your sketch. You can copy and paste the url from the URL bar to save it or share it with other people. You can also use the browser `back` and `forward` arrows to navigate to earlier versions of your sketch.
![](../../images/lV0rmoh.png)

### On the internet

You can use the upload button on the editor to upload your sketch to the hydra database. This is useful for sketches with longer code. Sketches and their screenshots are shared publicly on Mastodon. (NOTE: the Mastodon account isn't working right now, so sketches aren't shared).

### Share hiding code by default
The `showCode=false` URL flag makes it possible to share a sketch with the code hidden.

#### Example
Here's a patch and its URL:
```javascript
osc(10, 0.1, 1.2).modulateScale(noise(3)).out()
```
```
https://hydra.ojack.xyz/?code=b3NjKDEwJTJDJTIwMC4xJTJDJTIwMS4yKS5tb2R1bGF0ZVNjYWxlKG5vaXNlKDMpKS5vdXQoKQ%3D%3D
```


We can easily share the same sketch, but with the code and toolbar hidden by adding `&showCode=false` at the end:
```
https://hydra.ojack.xyz/?code=b3NjKDEwJTJDJTIwMC4xJTJDJTIwMS4yKS5tb2R1bGF0ZVNjYWxlKG5vaXNlKDMpKS5vdXQoKQ%3D%3D&showCode=false
```

Pressing `Ctrl+Shift+H` will show the code again.

---

## What is an error?
Sometimes, you will try to run a line of code, and nothing will happen. If you have an error you’ll notice text in red at the left-bottom on your screen. Something like ‘Unexpected token ‘.’ (in red) will appear. This doesn’t affect your code, but you won’t be able to continue coding until you fix the error. Usually it is a typing error or something related to the syntax.

## What is a comment?

```javascript
// Hello I’m a comment line. I’m a text that won’t change your code. You can write notations, your name or even a poem here.
```

---

By Flor de Fuego, Olivia Jack
