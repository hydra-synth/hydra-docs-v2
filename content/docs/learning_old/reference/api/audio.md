
## Audio reactivity
FFT functionality is available via an audio object accessed via "a". The editor uses https://github.com/meyda/meyda for audio analysis.
To show the fft bins,
```javascript
a.show()
```
Set number of fft bins:
```javascript
a.setBins(6)
```
Access the value of the leftmost (lowest frequency) bin:
```javascript
a.fft[0]
```
Use the value to control a variable:
```javascript
osc(10, 0, () => a.fft[0]*4)
  .out()
```
It is possible to calibrate the responsiveness by changing the minimum and maximum value detected. (Represented by blur lines over the fft). To set minimum value detected:
```javascript
a.setCutoff(4)
```

Setting the scale changes the range that is detected.
```javascript
a.setScale(2)
```
The fft[<index>] will return a value between 0 and 1, where 0 represents the cutoff and 1 corresponds to the maximum.

You can set smoothing between audio level readings (values between 0 and 1). 0 corresponds to no smoothing (more jumpy, faster reaction time), while 1 means that the value will never change.
```javascript
a.setSmooth(0.8)
```
To hide the audio waveform:
```javascript
a.hide()
```

```javascript
a.setBins(5) // amount of bins (bands) to separate the audio spectrum

noise(2)
	.modulate(o0,()=>a.fft[1]*.5) // listening to the 2nd band
	.out()

a.setSmooth(.8) // audio reactivity smoothness from 0 to 1, uses linear interpolation
a.setScale(8)    // loudness upper limit (maps to 0)
a.setCutoff(0.1)   // loudness from which to start listening to (maps to 0)

a.show() // show what hydra's listening to
// a.hide()

render(o0)
```

---

## available Audio methods

### fft
fft = Array(4)

```javascript
// default
osc().modulate(noise(3),()=>a.fft[0]).out(o0)
```

### setSmooth
setSmooth( smooth = 0.4 )

```javascript
// default
a.setSmooth(0.8)
osc().modulate(noise(3),()=>a.fft[0]).out(o0)
```

### setCutoff
setCutoff( cutoff = 2 )

```javascript
// threshold
a.setCutoff(4)
osc().modulate(noise(3),()=>a.fft[0]).out(o0)
```

### setBins
setBins( numBins = 4 )

```javascript
// change color with hissing noise
a.setBins(8)
osc(60,0.1,()=>a.fft[7]*3).modulate(noise(3),()=>a.fft[0]).out(o0)
```

### setScale
setScale( scale = 10 )

```javascript
// the smaller the scale is, the bigger the output is
a.setScale(5)
osc().modulate(noise(3),()=>a.fft[0]).out(o0)
```

### hide
hide(  )

### show
show(  )



