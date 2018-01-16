Now this is a quick, albeit maybe not obvious one. We can improve the anti-aliasing of the "oscillator" we just made up by encapsulating it in a `[poly~]` subpatcher, thus allowing for upscaling the signal.

Bear in mind that this imposes additional load on the CPU, but in terms of audio quality, especially when talking about oscillators, it's almost always worth the effort.
