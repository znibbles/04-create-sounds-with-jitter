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

## Dealing with Artefacts

One problem we face is, if we scale the image with our slider here, there are audible clicks, because the image processing is far slower than the audio sample rate. Were this an envelope calculation, we'd add a `[line~]` object in between to interpolate the values. It turns out, we can do something similar here.

We will use a trick normally employed in computer vision, more precisely motion detection, to process the current with the previous image in the stream. That trick consists of adding a `[trigger l l]` object and hijack Max's right-to-left, depth-first message traversal (see Max Tutorial #5: Message Order and Debugging). We pipe the right list outlet to our wavetable matrix, and the left back to a `[jit.xfade]` object in a feedback loop, which we will discuss in a moment.

Let's enable debugging and add 4 breakpoints to see what is going on: coming into the crossfader, the matrix is then passed on to the `[t l l]`. It first goes off to the wavetable, then back to the right inlet of the crossfader. That way, always the previous image is present on this right inlet. **Be careful not to mix up that ordering, otherwise your patch will blow up!**

So we use this crossfade object to interpolate between successive images, providing a smooth transition and avoiding those clicks. If we add another knob here, we have another parameter to do sound design with.

## Finishing Touches

We provisioned our patch for 2-voice polyphony, so let's add that second voice and play around with it a little.

	I hope you enjoyed this episode as much as I did and already have a ton of ideas of where to go with this. Feel free to reach out to me, I'd love to hear about it!