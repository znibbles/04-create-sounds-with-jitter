Let's get back to the basic concept of applying graphical transformations on sounds. For that, I'm going to pick up the patcher from episode 5 from the "Create Sounds with Jitter" playlist. I'll remove the effects chooser, so we can start afresh.

What I'd like to take on this time is applying pseudo color transformations on our sounds-as-images. Pseudo coloring is the process of applying color to grayscale images by mapping luminance to the red, green and blue channels by applying some mapping function. By varying the mapping functions slightly, subtle differences in the grayscale image are emphasized. You can see this stuff in action on any airport security X-ray.

So, extracting the subtleties of a sound using pseudo-color imaging, let's try it out!

## Convert to `char` and Spread Out
First, we are going to need to convert our `float32` grayscale matrix to a 4-plane ARGB one. Let's open a new subpatch and first shift the values in the matrix up to a range of `0.` to `1.` (from `-1.` to `1.`) to avoid any transformation errors. Now we can plug it into a 1 plane char matrix, but we need to provide a second inlet to get hold of the actual matrix dimensions. Eventually, we're packing the same matrix 3 times as red, green, and blue planes.

## The Mapper
Next, we are going to implement the actual mapping. As I want to expose some UI objects, I'm going to do this in a `bpatcher`, for which I create a new patcher file called `pseudo_color_mapper`. 

First, we need an idea of what our mapping function should look like. To demonstrate this, I'm going to use an absolute cosine function, i.e. a cosine with the negative lobes flipped to the positive side. Thus we can generate interesting overlapping mappings with only two parameters, the cosine frequency and offset. Let's use `[jit.expr]` to achieve that. We generate the appropriate expression using `[sprintf "expr abs(cos(norm[0]*%f+%f))"]`. Using `[bondo]` we make sure that every change to either frequency or offset will trigger a change.

Now, let's visualize the transfer function using `[jit.graph]`. For that we need to first cast everything back to `float` and set the `[jit.graph]`'s range accordingly from 0. to 1.

This will only yield colored images, however, if we apply a different color channel to every channel, and that's excactly what we are going to do - we duplicate this transfer function generator twice and pack everything into an ARGB mapping matrix, which is needed by `[jit.charmap]` to operate. This object is at the heart of our mapper patch, because it maps 256-point input data according to, you guessed it, a 4 plane ARGB matrix containing our mapping functions. All that is left to do is generate in- and outlets for our `bpatcher` and set up the presentation mode.

## Calculating the Difference and Converting Back to Audio
For the last step of our transformation, we take the difference of both mapped images using `[jit.-]`, which is a shortcut for `[jit.op @op -]`, and afterwards convert back to a grayscale char matrix using `[jit.rgb2luma]`. Afterwards, let cast the data type to `float32` using another `[jit.matrix]`.

Finally, we need to plug our transformed image back into the `write_transformation` subpatcher, but since that is a cpu-hungry operation, let's put a `[jit.change]` object in between to forward jitter matrices only when they change.

Now, a problem with this approach is that the computed sample values will all be positive, amounting to a DC offset which isn't good for our speakers. The simplest remedy is to insert a highpass filter before playing back our transformed sound to the sound card.

So let's explore that a little. Warping around our transfer functions so that they are close, but not congruent, gives the most interesting results.

