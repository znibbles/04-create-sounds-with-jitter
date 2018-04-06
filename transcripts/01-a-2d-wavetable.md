## An old acquaintance

Our good old friend, the wavetable oscillator. We use it to lookup waveforms of varying character, play them at arbitrary speed and enjoy nice additional features - such as interpolation - provided by friendly helper objects, such as `[wave~]`.

Internally, we know, even the `[cycle~] `object uses a 16K wavetable, which we can override if we like to, and drive it by phase.


## A somewhat weird sibling

Now what if we liked to make the sample lookup a little more complex, by using a two-dimensional wavetable? To be sure, there are interesting opportunities in the MSP domain, such as the `[2d.wave~]` object. While experimenting with it may be worthwhile, it’s also a bit counter-intuitive. In fact, there is a significant number of questions regarding `[2d.wave~]` in the [cycling74 forums](https://cycling74.com/search/page/1/2d.wave).


## Something different altogether

However, the `[jit.poke~]` and `[jit.peek~]` objects are valuable alternatives to consider residing in the Jitter domain.

We start our experiment by loading in `[jit.expr]`, which allows us to, well, compute various functions based on the cell indexes of a jitter matrix. I'm not going to go into great detail of the syntax here, suffice it to say, the `norm` expression gives us access to the cell indexes of the dimension specified in square brackets, normalized from 0 to 1. So `sin(norm[0]*TWOPI)` gives us a single sinusoid in **every row** of the matrix.

However, since a matrix is two-dimensional, we can also define two-dimensional expressions. For example, we can use the signed normalize (`snorm`) expression to calculate a bivariate cosine or sine function, which depends on the x and y coordinates of the matrix.

Now off to `[jit.peek~]`. It takes the name of the lookup matrix, an argument specifying the dimension count, and a third determining the plane to take the values from. The number of inlets generated depends on the second argument, so here we have one for the x, and one for the y axis. We add two `[phasor~]`s, which scan the matrix in x and y direction, accordingly.

If we use the first expression, we get a plain sine wave regardless of the frequency of the second phasor. Why is that? Well, as I said, the same sinusoid is applied to every row of the matrix, so the y coordinate - the second phasor - won't make any difference.

If we take one of the other two expressions, that's a different story.

Now, let's have a little fun with it. Make a sample-and-hold circuit to specify the frequency of the first `[phasor~]`. And one for the second one. The sound is yet a bit harsh, but it's an excellent starting point for subtractive synthesis. We're going to deal with that in the next episode.

### Max Compressed Representation

```
----------begin_max5_patcher----------
1149.3ocyZ0zbahCF9r8uBFNsMqWOnOAu21i8ztG5L6gNYxHiUbHKfn.Nwsc
p+su5CnwNQNVwH2vEMQuQgW8779MjuOcR3RwVdSXveF74fIS99zISzhTBlzs
eRXAaaZNqQervlTQEeW3LyuJakVnX48+AE1Kro8q4bs7dIkaJxJy4s5GArSX
EqM8trx02TySaMWAP7h4QyBRRTqjH8JbdTv0O8bDaZ6ePQcRSY4oohMk5GBE
2Ic4laax9l9dffJQ+X5T0xLGAZI+QIr5QPKeq9wG966B.QQysQ..OP.PrBz.
.dNwf8YAviy.fNoFQseshadLgMYqKY4gAW6OfekD3.fUfG4Mfar733QDtqti
0Hp2ErvFzIK7lSujakqzjQDzaX2sKHZNwFvS7.vAZaNwDsOlr4xfbHzZPNId
33dgJ1F.Rzg3fwUHNFaG1TOA6t.7EuGn9nktHjgiNJRm+xj2dTV4pOQFH1pE
F6AKLx3YqPeLdDlCGfrBcjGxkQeJG9nB5uVNbOzwFD9dmB+nQ0XeTaVFrHco
glDViy3ZUiYQx1mrQAIdiB.I.cpM5HLrFZunEN1enGqwcxHqZ8Qs6TeEYOlM
6H6sngIdC7iPq98Ysyq37+aWvirG3srk47.XPjMdvCUzQQ5zeRrqBBPnSPDv
SPDyj2.eSFEr15rs6wFffayErVDTGdnWrwNtVzG7JdI3HCgzU3CdtziDG2Xv
w4SQE7lF1Z9K3H91p5flrxeqoTTW74nqux7Cfqu5S+6e+Oe7C8+E4Yk7eVzB
Zgw7P6B3tzoHrolhlwPj2dj0EfiREMCmi.diiLSJMtnHkaTOCcHwrGGf7PeW
nEZNvz1UWZ3yIA74wA5brOlUtR7nM7E+VySfLy52gCpNmJUVW8sll3hj+TYX
sgRezBgI+H.pw8IshWzzimtBha0MPDuU2Xg4c8RIiPd4KE71Zgr9usRm8xtU
T1VxJLjveUmopv6IeFptjJE4Xfu5lzOaEPkw7HL1RV45yimZEqWmys4P3M+A
joMhn90Sf4JVsj6a402vKUM9r+.nu.3YpRVmCt4eaEK0526wacWR0Y4wj90i
Mb8d2d8QzUie1GuRqEk7CgTiXScZOWz+wZBdRSq3MsYkr1LQ4dGRM.o5PVoM
m0TjCZp+5LHMo9XDmTSpWbuLU2.0ThKXBdvgD0q30O2NdoTcjcUCFlpicwmg
3CKI0EKoO7NUu12SqInO7YPt5c5eeFjqfz+9LPWTM88S0jKAgicIHU89IFrW
E1oXxDODofothoAqIhKZh5CM4Rz+QbNg+ZzLv2gENoY7hKRXgKQjnC46hrUU
BY2ac84fMuqIPLv7ZAvV1YF2BZ9uG446LcWhMeCZSWlCCS.ugoes2aWt19nV
NZwvInn2ABB4T9NzknxkSpFE+ZTVOQPzyVfhLQFGtCahZfI+bmuC3Qj2fseX
ZxEi0KziYLJVU0C75ltCqUgbFv6EZBHdldaVoYKRusl+PV+40ecyPVsb3tV4
jcapMyBtMwLfXXgPRkkax5bKjZ9GS+e.JnkBz
-----------end_max5_patcher-----------
```