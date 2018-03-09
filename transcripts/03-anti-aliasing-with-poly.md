## Anti-alasing FTW

Now this is a quick, albeit maybe not obvious one. We can improve the anti-aliasing of the "oscillator" we just made up by encapsulating it in a `[poly~]` subpatcher, thus allowing for upsampling the signal.

Bear in mind that this imposes additional load on the CPU, but in terms of audio quality, especially when talking about oscillators, it's almost always worth the effort.

## A `[poly~]` without polyphony?

So first we're going to create ourselves a poly patcher file, which is going to be passed to the `[poly~]` as an argument. In our case, we're just duplicating the matrix lookup we used in the previous episodes, and wrap it between `[in~]` and `[out~]` objects. These are special in- and outlets designed for working with `[poly~]`s.

Next up, we're going to remove the random sequencer components from the patch, making room for a little experiment. Let's double the lookup subpatcher and call it `lookup_poly`. In it, we make up a `[poly~]` object and pass some arguments:

- The name of the patcher file we wan't to "multiply",
- The number of voices we want to allocate,
- Whether we want to down- or upsample,
- And by what factor we want to do so.

Note that the last two arguments are optional. So with just one voice, why all the hassle? The answer lies in those last two arguments: Here we upsample by four times whatever the sampling frequency is, so e.g. $4 * 44100 = 176400 Hz$, effectively shifting  the foldover frequency (also called the Nyquist frequency) to 88200 Hz. This will make a huge difference for the oscillator's sound, as we'll soon hear.

## Testing out the anti-aliasing effect

So, without further ado, I'm going to set the pitch to a very low one, 40 Hz, so we can optimally inspect the outcome. I'm also pulling in a `[selector~]` for A/B testing. Let's make a radio group here, fine.

I'm gonna use `[spectrumdraw~]` here, which is part of the [HISS toolbox](http://eprints.hud.ac.uk/id/eprint/14897/). 

Let's start with the pure sine wave. Hear the hissing? That's the anti-aliasing produced by foldover at the Nyquist frequency.  Now let's also check out the other two waveforms.

Clearly, sound-wise, this is an improvement. We could have gone farther by pushing the upsampling factor up to 8 or 16, but as I said, this comes at the price of higher CPU cost.