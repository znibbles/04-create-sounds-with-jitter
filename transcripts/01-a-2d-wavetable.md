## An old acquaintance

Our good old friend, the wavetable oscillator. We use it to lookup waveforms of varying character, play them at arbitrary speed and enjoy nice additional features - such as interpolation - provided by friendly helper objects, such as `[wave~]`.

Internally, we know, even the `[cycle~] `object uses a 16K wavetable, which we can override if we like to, and drive it by phase.

## A somewhat weird sibling

Now what if we liked to make the sample lookup a little more complex, by using a two-dimensional wavetable? To be sure, there are interesting opportunities in the MSP domain, such as the `[2d.wave~]` object. While experimenting with it may be worthwhile, it’s also a bit counter-intuitive. In fact, there is a significant number of questions regarding `[2d.wave~]` in the [cycling74 forums](https://cycling74.com/search/page/1/2d.wave).


## Something different altogether
However, the `[jit.poke~]` and `[jit.peek~]` objects are valuable alternatives to consider residing in the Jitter domain.
