## An old acquaintance

Our good old friend, the wavetable oscillator. We use it to lookup waveforms of varying character, play them at arbitrary speed and enjoy nice additional features - such as interpolation - provided by friendly helper objects, such as `[wave~]`.

Internally, we know, even the `[cycle~] `object uses a 16K wavetable, which we can override if we like to, and drive it by phase.


## A somewhat weird sibling

Now what if we liked to make the sample lookup a little more complex, by using a two-dimensional wavetable? To be sure, there are interesting opportunities in the MSP domain, such as the `[2d.wave~]` object. While experimenting with it may be worthwhile, it’s also a bit counter-intuitive. In fact, there is a significant number of questions regarding `[2d.wave~]` in the [cycling74 forums](https://cycling74.com/search/page/1/2d.wave).


## Something different altogether

However, the `[jit.poke~]` and `[jit.peek~]` objects are valuable alternatives to consider residing in the Jitter domain.

We start our experiment by loading in `[jit.expr]`, which allows us to, well, compute various functions based on the cell indexes of a jitter matrix. I'm not going to go into great detail of the syntax here, suffice it to say, the `norm` expression gives us access to the cell indexes of the dimension specified in square brackets, normalized from 0 to 1. So `sin(norm[0]*TWOPI)` gives us a single sinusoid in **every row** of the matrix.

However, since a matrix is two-dimensional, we can also define two-dimensional expressions. For example, we can use the signed normalize (`snorm`) expression to calculate a bivariate cosine or sine function, which depends on the x and y coordinates of the matrix.

Now off to `[jit.peek~]`. It takes the name of the lookup matrix, an argument specifying the dimension count, and a third determining the plane to take the values from. The number of inlets generated depends on the second argument, so here we have one for the x, and one for the y axis. We add to `[phasor~]`s, which scan the matrix in x and y direction, accordingly.

If we use the first expression, we get a plain sine wave regardless of the frequency of the second phasor. Why is that? Well, as I said, the same sinusoid is applied to every row of the matrix, so the y coordinate - the second phasor - won't make any difference.

If we take one of the other two expressions, that's a different story.

Now, let's have a little fun with it. Make a sample-and-hold circuit to specify the frequency of the first `[phasor~]`. And one for the second one. The sound is yet a bit harsh, but it's an excellent starting point for subtractive synthesis. We're going to deal with that in the next episode.
