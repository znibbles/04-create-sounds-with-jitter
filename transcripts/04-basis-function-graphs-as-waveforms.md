## Preparing for Polyphony

In the previous videos, we have laid the ground for some awesome moshing of two dimensional data. In this episode, we will look at another method for generating wavetables to create interesting drone sounds.

But first, I have altered the `peeking.poly` patch in such a way that we can employ it for real polyphony, not only upsampling. To achieve this, we have to include a `[thispoly~]` in the patcher, and send it a `[mute 0, 1(` message: This will unmute the voice and mark it as busy. Through the inlet we now receive a list of two frequencies that we then have to unpack.

Outside, we specify that we now want 2 voices and activate the `@steal` attribute, otherwise we would have to manually mute and release the voice in the poly subpatcher.

Furthermore, I have again added a `[lores~]` lowpass filter so we can fully concentrate on the Jitter part. Refer to the source code if you want to follow along from here.

## Basis Function Graphs

Jitter includes the `[jit.bfg]` object, which is used to generate what is called _basis function graphs_, which can in turn be used for geometry or texture generation, or other 2D and 3D graphics related stuff. We, however, will once more use it for the creation of waveforms. Since this process can be CPU-intensive depending on the used function, we downsample to one image every 100 milliseconds, that's more than enough for now.

Let's look at the help patch. We will copy and paste the `basis` attribute, the `scale` property and the `[jit.normalize]` object. Let's take a look.

We have to adapt the matrix dimensions to 1024 x 1024, otherwise we're good. Let's add a `[live.slider]` to adjust the scale between 1. and 20.

The `[jit.normalize]` object normalizes the matrix to cell values between 0. and 1. We can select different basis functions such as a simple sine, a sawtooth, or noises. Let's listen to a few.

To do this, we need to send a `note` message to the `poly~` containing the two peeking frequencies. Remember, a note message is sent to the first non-busy voice, or a stolen one in our case. We can verify that by opening that voice.