## Sequencing in two dimensions

Let's add another twist to this, exploring the two dimensional space for sequencing. First, let's encapsulate the whole matrix lookup into its own subpatcher, and make another one for the noise matrices we are going to use for sequencing.

We instantiate a `[jit.noise]` object with 2 planes and 16 by 16 cell values. We are going to use these for pitch and filter sequencing, so we store them in two matrices. Let's look at the values we are dealing with here by unpacking the matrices and plotting them in a `[jit.pwindow]`.

## Looking up pitches

Audio tends to be rather loud in this patch, so let's soften it. We then make another subpatcher for the pitch sequence. We again create two phasors to peek into the two planes of the pitch sequence matrix. In this case, we reuse them for looking up the y coordinate of the other `[jit.peek~]`, respectively.

We then need to scale the float values from the matrices into meaningful oscillator frequencies, and take an exponential curve here, hence the last argument. Now we have the pitch lookup ready - let's take a listen. Let's also look at those scopes to get a feeling for the data stream here.

## ... and filter values

We then duplicate the whole process to make a filter sequencer. For simplicity, we take the same noise matrices and just read them out at lower rates as well as scale the values to accomodate low pass filter cutoff frequencies and resonances - but we could of course have taken a fresh matrix just as well.

Let's use a `[lores~]` IIR lowpass filter for this purpose, so we have a resonance factor we can play around with. So here we are with an almost R2-D2y generative pitch and filter sequence. Needless to say we could vary the content of those noise matrices based on a `[metro]` etc. - the possibilities are endless.