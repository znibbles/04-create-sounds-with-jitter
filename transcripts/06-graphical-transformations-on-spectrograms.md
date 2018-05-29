Similar to what we did in the previous episode, we can apply graphical transformations to spectrograms, too. We take [Jean Francois Charles'](https://cycling74.com/tools/charles-spectral-tutorials) ideas on transforming spectrograms into Jitter matrices and back, and fiddle around with it. You can look up the details of the process in his impressive tutorials, because we're only interested in the graphics stuff here.

I've prepared a short sample I recorded with the Korg Volca FM and saved the spectrogram as a Jitter jxf file, so we can just read it in here. Down here I have a playback section that just comprises the `jit.peek~`ing necessary to reconstruct the sound via inverse FFT, which is exactly what this `play_transformed.pfft` patch does. 

Now we need to understand that the original matrix consists of two planes: One for the amplitude, and one for the phase response of the spectrogram. Hence, we can either choose to `jit.unpack` them and treat them separately, or to leave them interleaved and transform both at the same time.

Let's do a resynthesis run without any transformation to understand how this works. First we have to invert the y dimension with `[jit.dimmap]`, otherwise the spectrogram will look unfamiliar because lower frequencies are plotted in the top area and high ones in the bottom. We `jit.unpack` and see two plots. The first looks familiar - it's an amplitude spectrogram where brightness denotes intensity. The second much less so, and that's because it's the phase spectrogram of the sample. However, as we all know, phase information is critical to reconstruct audio frames from spectral information.

Okay, let's just run the Jitter data stream into the `transformed_spectrum` matrix and resynthesize it using the above mentioned pfft~ patch. Sounds a bit blurry, that's because of the window size used in this example - I'll leave it up to you to google the time/frequency resolution tradeoff that is inherent to the DFT approach ;).

## Rota, again

I've prepared a few `bpatchers` so we can focus on the transformations rather than build everything from scratch. The first one is a wrapper around `[jit.rota]` again. Using just zooming and offsetting, we can build a simple pitch shifter and time scaler, but using the built in bound modes also proves an interesting tool to freeze and fold samples over time and frequency.


## Streak

What we can do here as compared to the static transformations we  employed thus far is create temporally dynamic effects. For example, `[jit.streak]` is an object that smears single pixels depending on the `probability`, `scale` and `direction` parameters. If we combine this with the feedforward crossfading from episode 4 of this playlist, we can create slowly evolving, compelling drones.

## Sprinkle

A similar effect is `[jit.sprinkle]`. It also adds pixels based on a `probability` parameter, but does so in a bounding box defined by `x_range` and `y_range` rather than smearing individual pixels. Combined with the feedforward crossfade again, we can generate all sorts of self-transforming soundscapes.