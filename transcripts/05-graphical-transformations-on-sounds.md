## Setting up a `[jit.buffer~]`

This video is going to be a little different. This time, we're not going to build a Jitter-based oscillator, it's going to be a Jitter-based audio effect. Or rather, a whole range of those.

One way of loading audio into jitter, other than the `[jit.poke~]` object, is a `[jit.buffer~]`. Basically this objects works just the same as a normal `[buffer~]`, only that it stores and outputs audio data in a Jitter matrix. We give it a name, a length and a channel count. The length actually doesn't matter, because we are going to `[replace(` it and sample memory will be resized anyway, so let's set it to -1, and the channel count to 1, for simplicity. 

We add a `[fpsgui]` here so we can take a look at the matrix dimensions. One thing we have to take care of is the length of the output matrix, because it's set to 44 by default, which is a bit small. So we send it the `[getframes(` message, which will give us the number of frames (or rather, samples) out the right outlet. We look for the `frame` message here, subtract 1 and send it to the `outputlast` attribute. Now we trigger `[output(` again, and see that the output matrix now has the correct amount of cells. 

## Making a Square Matrix of Audio Data

For what we are going to do, however, we want that matrix data to be two-dimensional rather than one-dimensional - we want a square matrix, more precisely. So we take the square root and round. Unfortunately, there is no `[ceil]` object in Max, and for this tutorial I didn't want to instantiate a Javascript object or do a complex if-then calculation, so `[round]` will have to do here.

Okay, so a one plane, two-dimensional matrix of float32 values, we'll name that `buffer_as_square` and set the right dimensions.  So how do we get that one dimensional matrix data into a two-dimensional one of equal cell count? Well, the obvious way would be to `[jit.spill]` it into a list and then again `[jit.fill]` it into that matrix. It turns out that this approach isn't feasible, because Max caps the list length at 2^14 = 16384 values, which is a bit short for audio data. So we'll have to find another way.

For this demo, the most useful way seemed to be to `[jit.iter]` over the values and manually set the cell values of the target matrix. So we have to do a little calculation here to return column and row indexes (or rather, the x and y coordinates in the matrix) and set the values via `[setcell(` respectively.

## Time For Some Data Moshing

So where do we want to go with this? Eventually, I want to be able to pipe this matrix into any jitter video effect and then again construct an audio buffer from that. Let's draft this part up front.

Okay, first we need a `[buffer~]` of proper size, so we send in `[sizeinsamps]`. Cool. We also set up a `[groove~]` object for playback - fine.

To get audio data in there, we need another `[jit.iter]` and a `[poke~]` to feed data into the buffer. Poke wants a sample value and a buffer index to write it to, so we kind of reverse the process from above, to get a single row from those x and y coordinates. To check out if that process works, let's just pipe the input matrix in there and see if the output sounds as we expect. Seems fine!

Alright, let's try out some video effects. There's a ton of what you can do here, and we'll dive into more effects in a later episode, but here are some pointers.


`jit.op @op >p`

`jit.convolve`

`jit.plur`

`jit.rota`