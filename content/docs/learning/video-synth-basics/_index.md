---
title: video synth basics
weight: 1
---

# Modular Video Synth Basics

Hydra is inspired by [modular synthesis](https://en.wikipedia.org/wiki/Modular_synthesizer), where patching together different functions generates a visual output.

![](https://i.imgur.com/RBRxeiL.jpg)
###### source [Sandin Image Processor](https://en.wikipedia.org/wiki/Sandin_Image_Processor)

There are five base types of functions in hydra: source, geometry, color, blend, and modulate.
A basic hydra pattern always starts with a ***source*** (such as `osc()`, `shape()`, or `noise()`), followed by transformations to ***geometry*** and ***color*** (such as `.rotate()`, `.kaleid()`, `.pixelate()` ), and ends with `.out()` to show the result on the screen. ***blend*** and ***modulate*** functions allow combining multiple patterns and textures together. 

## getting started
For a detailed tutorial of how to use these basic function types, see [getting started](getting-started). For a complete list of functions and example usage, see: [reference](../../reference).
