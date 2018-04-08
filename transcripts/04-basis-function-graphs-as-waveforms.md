## Preparing for Polyphony

In the previous videos, we have laid the ground for some awesome moshing of two dimensional data. In this episode, we will look at another method for generating wavetables to create interesting drone sounds.

But first, I have altered the `peeking.poly` patch in such a way that we can employ it for real polyphony, not only upsampling. To achieve this, we have to include a `[thispoly~]` in the patcher, and send it a `[mute 0, 1(` message: This will unmute the voice and mark it as busy. Through the inlet we now receive a list of two frequencies that we then have to unpack.

Outside, we specify that we now want 2 voices and activate the `@steal` attribute, otherwise we would have to manually mute and release the voice in the poly subpatcher.

Furthermore, I have again added a `[lores~]` lowpass filter so we can fully concentrate on the Jitter part. Refer to the source code if you want to follow along from here.

## Basis Function Graphs

Jitter includes the `[jit.bfg]` object, which is used to generate what is called _basis function graphs_, which can in turn be used for geometry or texture generation, or other 2D and 3D graphics related stuff. We, however, will once more use it for the creation of waveforms. Since this process can be CPU-intensive depending on the used function, we downsample to one image every 100 milliseconds, that's more than enough for now.

