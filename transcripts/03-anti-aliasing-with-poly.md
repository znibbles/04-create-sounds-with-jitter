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

### Max Compressed Representation

```
----------begin_max5_patcher----------
1835.3oc6a07kaZCD+r8eE73TaxF+PhOLt25wdp8PeudHce9ICxdUBFQAwZm
jW1+1q9.vfMXKafjrs9Bd0HglY9oQilYP6WlNwbEcONyz3WLduwjIeY5jIRR
BBSJZOwbKZePDJSNLy.51s3Xl4Cp9X38LIcTLi7NTDAkgC+63TztxQDQhwAz
7X4vfEDIgxWht5CuC.KGYF6SQXI8RJw4aIwQXlj0fBhIHVvSj3MKSwALknCr
smY8fAvag3m49hmBRFOdXhn4rxYxRP7qSmJd7fl5cLdGWZOQseqAvrEcxUSc
BdFcxBLy8ACH.HzF3BYCXm5TI5nHw9TBVMMlDAx+3MnwonPBcSJMOoMMzd.V
0rfBkB34K+Qsp4bQMLAkh1hY3zk3XzJEus5R6MqlLBCusrixgmQ9Lt9xvynn
7FCHjjIXQnZ13xm0MAkcX7jgi3fAM8ECXaPrklPr8Yf34RiGn7m4VpFWuMTF
YSLJxbH08DiHJ8isZa4ODadjlVKbDZrqWu065bBmVnsEp6Dy0jH7y3zLBMt1
zMwDkjTi7jZuh.i9.UNQyenhDIVQxthTJ9YR466TQEkx0WFWYySUvydeOyCS
CMDmFmSjhhhHe0ZZ4DVBQtRek.KID44HsMb7sN.R7E1MQzfOpr9sJIRSvwj3
jTbF+X.DqP3p5NDuFkGwVtlFyJ2dA3Pea8uFEf67ki4axkJ2ulR3qAkCYSJI
jFKDhFPsfbI63K+tRcystxHGQLJokWlazwgkN5Liqj4YqPohUhB2MvxNYTZT
ytpduH7ZVQ2Ij33iPQFMo6NSIad5Lu6JJuysmatk8jsLOV06R9lQ1xLzyMQa
FJJpX2YyoeOJlrEwvLhZI.ZU0oxk6SYAoznnF5qpmmaomPtQb.dGIj8jjQ0M
F3CmjTZDYVsJGR1fyXMowPaxZR4D+CbR4qJ1jtj6uOIhqEMGPi.dpuirterF
zOm+rl9zdyKBGONyp2YM+ZN9063TguKGbc5jywUdvos7mS8x0smNM7xOozSe
g+igGsRdBkIO6i66oKDa93fXVRuc9fWYH1kru7FVzxUhRu1Murgcad4NJ.1q
TqqOPXyRv3O9hwN9IELg2bCngUWXmy.icpj.l6oboYqA3A0.7dPlBv.BfREp
CL4Z8uaoi4jJxL6pm81Zp1fJSl+XojDGh2WK9wQG4VLrHWge9uqPGb.gNkD0
A1ArtRvCnyNQn5Gs.Oq9ZRcHJcQ0h5H7LonJ5uc.KilmFTthUXSYzTp4gaxH
wU4r79CAYTlW+EWutVgvWWgvcDEBGskBtScCvXIEy0UJ7GSrv6ZvhQSJb0UJ
7FSovQSoP3hoConfXYEFLEYYFtTkQ8RDikRVkyTaoqWxjqJyuMQzUnnh75pp
Kv4RC7PthSOHuCTQ6Hadw3PJK08D6N.0DUVoNa4SW6uG0qKKgKLo4aCSQ6do
EkrpTkcVjllJqS2JKTc.MzyWEniJ7YOqKpvsT7Hy1K3TqUDd3LF3YjYMq0h2
N.0vTU614x5z4.ecT6Vvfo32qca20t0yBpbSL+dwauW716EucPpWDM5SuXHJ
.B2azLQqY7WkKO7.hySLb5J6qQoDR9p5T5u3GwZHcuDH2KAx+wKAhJtTm4+O
oDHfQLse+qPFFqDcA2yy8LI7EvCapsL8bVz+X4UGjUjDCvxV8423w1wy1yBz
08ypfZ.JJn5pi4UlH4p70kAKZCGtjZDe9CdbSoj8099G.i0QTDyFJ+vaxGsA
TCvkgpLWXXwM14R46A6JuGtdrToG093GWIDsEmkg1fOAiv6SRMxHw+TFOams
u25w2n9Cviu4O+qe+O9seV2K8mCr+lVNf4puUjiJVIUJQtWelhi.FEPy5OFA
FLLx2WEU4ORPjvLpDgZBL0v.6AvEj8BIF3nbA4byET31v.4WUcG+ne5t1zu4
WqeBaf5CkpzCOKQCOvL2q0Mws6b3L9OEKrsokd8eUrv+HPEM1EWEGU2iW9DD
8N2v1cvN2PUsLfm6Of3x+rEyRo7Hna6nyKWM4day3opRltUSW2BIuBEu41vI
FcylHbaFDCl8f5KH.sJeNj2o5a9Fki+bHJnsHM8FphFa6HWkcbKepw+A.xgz
LqKkxbbVFEpz4yv4bYV.6.0zkQtZvnlBCMMTUxav2DFCNkwViNiAf1UY6uMb
tEcF1KNKhM9hr1twmw1bKILgx2WVXA6nxh.LGnB3yokVP0Qntv1ZUT+C4+zI
.k+i9oSfASm91J25H10t5F2LirWze.x56..cjH0wkXvdLbJoEqsmeNHqDHT+
uPXq9eg3nVNpcMP+pVCsSNa2qXsuebRmEqAfO.WcORr2bRGUBLD6PA.c0IPe
4jkNKSKFi8TZwZ3Y2RIu.uPUL2vhvPOpkSMmPxVC8NJsTiNBaneGdqiQh2nr
xoIiuPHZpfgO5xOH34QW5gitvCmdYG59hNb7kbfy4uN8eArBDtHF
-----------end_max5_patcher-----------
```