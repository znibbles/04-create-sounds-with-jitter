## Setting up a `[jit.buffer~]`

This video is going to be a little different. This time, we're not going to build a Jitter-based oscillator, it's going to be a Jitter-based audio effect. Or rather, a whole range of those.

One way of loading audio into Jitter, other than the `[jit.poke~]` object, is a `[jit.buffer~]`. Basically this object works just the same as a normal `[buffer~]`, only that it stores and outputs audio data in a Jitter matrix. We give it a name, a length and a channel count. The length actually doesn't matter, because we are going to `[replace(` it and sample memory will be resized anyway, so let's set it to -1, and the channel count to 1, for simplicity. 

We add a `[fpsgui]` here so we can take a look at the matrix dimensions. One thing we have to take care of is the length of the output matrix, because it's set to 44 by default, which is a bit small. So we send it the `[getframes(` message, which will give us the number of frames (or rather, samples) out the right outlet. We look for the `frames` message here, subtract 1 and send it to the `outputlast` attribute. Now we trigger `[output(` again, and see that the output matrix now has the correct amount of cells. 

## Making a Square Matrix of Audio Data

For what we are going to do, however, we want that matrix data to be two-dimensional rather than one-dimensional - we want a square matrix, more precisely. So we take the square root and round. Unfortunately, there is no `[ceil]` object in Max, and for this tutorial I didn't want to instantiate a Javascript object or do a complex if-then calculation, so `[round]` will have to do here.

Okay, so a one plane, two-dimensional matrix of float32 values, we'll name that `buffer_as_square` and set the right dimensions.  So how do we get that one dimensional matrix data into a two-dimensional one of equal cell count? Well, the obvious way would be to `[jit.spill]` it into a list and then again `[jit.fill]` it into that matrix. It turns out that this approach isn't feasible, because Max caps the list length at 2^14 = 16384 values, which is a bit short for audio data. So we'll have to find another way.

For this demo, the most useful way seemed to be to `[jit.iter]` over the values and manually set the cell values of the target matrix. So we have to do a little calculation here to return column and row indexes (or rather, the x and y coordinates in the matrix) and set the values via `[setcell(` respectively.

## Time For Some Data Moshing

So where do we want to go with this? Eventually, I want to be able to pipe this matrix into any Jitter video effect and then again construct an audio buffer from that. Let's draft this part up front.

Okay, first we need a `[buffer~]` of proper size, so we send in `[sizeinsamps]`. Cool. We also set up a `[groove~]` object for playback - fine.

To get audio data in there, we need another `[jit.iter]` and a `[poke~]` to feed data into the buffer. Poke wants a sample value and a buffer index to write it to, so we kind of reverse the process from above, to get a single row from those x and y coordinates. To check out if that process works, let's just pipe the input matrix in there and see if the output sounds as we expect. Seems fine!

Alright, let's try out some video effects. There's a ton of what you can do here, and we'll dive into more effects in a later episode, but here are some pointers.


`jit.op @op >p`

The `jit.op` is the Swiss Army Knife of the Jitter domain. You can do all sorts of arithmetic and logical computations, just look at the help patch! We'll take the `>p` op here, which means "pass if greater than". You can think of it as a kind of noise gate in our case. Every point in the matrix is evaluated if it's greater than the value set at the right outlet and if so, is passed through.

`jit.convolve`

Convolution, in the 2d-space domain, is an operation that assigns a new value to a pixel based on the surrounding cells, a local operation. In that respect it is very similar to an FIR audio filter. If we, for example, calculate an average of the cell and its surrounding 8 neighbours - the so-called _convolution kernel_ - we get a blur, a graphical lowpass. Let's try that out. We have to set every cell in the convolution kernel to 1/9th = 0.11 to do this.

`jit.rota`

This is a global operation, i.e. all matrix cells are updated simultaneously. We can, for example, scale or rotate an image this way. There are 4 different `boundmodes`, which you can look up in the help patch. We are going to use `wrap` (2) here, and rotate by an angle specified in radians.

So you again have an interesting pattern here, which you can explore further. We're going to look at it again in a later episode. Meanwhile have fun implementing your own effects!

```
----------begin_max5_patcher----------
4053.3oc6cs0aiaiE94L+JDLl8kcc8JdWpOrXmVjhBrc2tncdYQQggrshi5n
H4QRNISKZ+su7hjrjsjCkEoSxTWzwwVxx7vOdH447wCO72dyUSVj9XX9Dmuz
4mbt5pe6MWck7RhKbU4muZxcAOtLNHW90lrL8t6BSJlLUcuhvGKjWGOy4+9c
u6+8Uu6q+WU2KY6cQIwgExGDr6hoaKptpa4UiVI+QRW7KeAzs54yK9Tbn75U
WYSPwxaiRVOOKbYgRpYtnYtScfPl3O.hq7S7Wc9Ywy76u4MhWlNxZGZlyW+s
e+2+iW6b827MW+0uebUQf2fphHjmnRgbsZUDNy45+y69pu6Zm+80u+G99QVC
YCqFBjUIBvlUPvLme78u6Gduy2d8Ob8HqdjgoiREUHLxD0ts7511ckUVvcgE
gYyCSBVnDjZwsH7txd1SRRSBmL0YxTwKqC3OP8mVllbeZ78MtRVZA+aLw4m0
GhPc.Qf9gH0SV7oMgkxWjn4Rbe9+pK2tzSHRLDUpn3pfR3IBkIgOvkzCzSD3
iCxAzoJBT25u2.p9ZU2wL2F0cB1FU8MNkM9k2XYZbZlp34EtOA6AlxeGG3Q9
zx2A8Q7lBgbT9PKV25wnDruKT7kILH+8h2gXTLCz9wzPMCbHLiF.L+KQEyuK
nHK5wiBzDHQ1ckObKgCzfc.cymILqDdKw2qlbSTbHueTdTZRCg8pIAa1z3xW
03QDMJ+hBoXSquTTh5Rn5KkEdeT0ySpuZPFWxK3h81LUE+QO5jc+LoqByR1F
IEE0E4pGuo5GrtGkROxSVkoXoJF1ycWsUzdFmt7CgqZL3BGb2DlDkrIKLmOT
aPQovUe6Ug2DrMtX9MoIE4Q+pT9.bLrq6eSvxvde3D93axJ26xhBhqpdSVmE
sJMQHDsfZwkqJtehOTqZ.2lUF42HIXSGOLWchCK8bybdkba9hfLQKQ4Hsvpa
VjlF29V0OWb3MEk2dSTRxdnXQ5l9uYVz5aOxytHkey6N1us7N4y2lnt6bdu+
h44A22FsKBhiKGNn8O+iAIQ7tKgEQpl.na8MUy1ba9xrz33V0W0ctui6rhqD
uL7gnUE2JKnlJC7udzlJknI0sxqhVGlWz9ZEAqyaekC54yuz1EkcRmymEbSr
b9rlegVl81rGYyANac88F.MnnHi24Z5ta2u1Z0c2WQu550cPPyZdqtFPToSw
6Nt+0DxirjWjtMYkny+j1+TGNL59Ckx.MejCQ0tm5ptqUOilBfRiWAHRSqeP
UyacU0bWkCP84VyQwsgEACuov0lMEJylrUSQKSKZadAeB3YBCLlnCr1FnfcC
TPxI.TMrCXpFvlBm.L+cFwuytKCAZRHnWXwcn5OVomrBGTlHfpes0SU4509E
VTxpvGaXIhQvLUM3Dzkb6QW5oAsdQEHEXZXYmMKwQI8MYkTfD2ua7JOca1xp
F3pw3cZKb7YeKhRpMg6m10wxQWc7AKEtuDjhpe7mVJrnPXf1ixKVYc+DgEdq
lqrlctXhnnEaKTJPMcWYPVcwcAXQPboMU0SqdLSv1Ym1a1IulxK0kAwK2Jrp
ad9G2FjENeUzciiQGOvfXzAxTj43iE9HBPrOqcRzS5jH.5ewIwKNIdwIQSXJ
btSd.uLCWxcWqkML6wdFxE6BkblAItTer7cdDJzqE6YFv7GF6jr+obTAfmbT
AOOqXd7wAR0T.YooGEH8b8IRZHQHHwmMUAt9dFGHI9iAHgp4R7vmafLSPbPu
VSCGpKHPr87AApFzFCrBFcSbJuF1t0p2kY435K2jlwGbTbU5f8rEcJ3G2e1E
AIq0EGox+PbO6cZ+X1o33VepZmh6t7VYQKil99Ck90YG8MdcbgX42Lf9ldZV
t1WyBnVWgy+zA7QwJBctQfe4mvfY8.Y0KX+PgLcgKEkbLuWCjK449ZhcIqPi
hmtDXHTbrGAFXMkBgEI68ESyVobN084UzDSy0mnArinoMkKXa11oqFjvr7yb
am1ryAN2scUcmdZQy8BcY0SI1gqlmnalcNywSGjFL+gDLLGM.fpBhJo0ETaD
CLu.fKOSAWTnBtX1Ct13bWvGp4gsZY0FSH8vvlp5ynMCMTD5yZRZk1L4dgg1
KLzdggVi3IYeDKNNumnVw6InayHh0Cdtc6tvYgyMlKrN.mRXcHomX5.Y1g3a
Slc5Gv96N.yw0J3TzoDwb8SFwPzRysjr4zbUNOOnzewjnDyVfT09x.+7nJsI
X4GbxCKVFFGy84w049.9emMrfgSmfdiLX8RKFGRP2lwU34NJ1hJZyXqlC2gL
X3ZM8TXXD69ZfgQFbbQ02e1ijMFZLgxl+qjPYCpKGr1LR1zUFDBAvRBAUatD
UBw4iuNskLgEL8IZ1gkS.YHMcVK7+HGTuOhTfskTnKk3LjMwB2gzh.sX.YBz
ruz4k6acQGhkCWUft8mGvpV74Ko7EoqWGGpyF18zXKk5Mv8Y6QoWW47hZqN.
wUuZR9h+Hu5mkJXgZ.6u1tp2jATuaE4B8WwQJidX.6PT9CYb2FlWjEjjqBLH
gZ6nhZYJdXaydlzYIRYH6UFvYetRHthZGfK9Bm3W3D+Bm3VkSbSErsCmNcnU
1ojX5yJc5b8ZAodtNtliScj2Ix+4T8nAkBarJqT+yMjE93lLm2FA9auMB9We
aDpWfCMTULjUTwTHDfJmcx2NLq2NS3zFudbpymL3dm7D27.H+FJMXb6LiyqR
pbYu14x8XqzP5GB+CmZCX4y6tX6M2DlcjUoAMTMI.dLbWBKyfTuJBjVJ50IM
2VgNWJRW9MXVjeCptjJKlTvZbPg7FhT3ZSo.nqTXKxsQZSyO1hRAfoKVXy.9
FvFBVbI7W60mkSzekSLdNo.SEPikYvEnueW9kXrcW+tTBXGoANBffYdBDhh8
wtxnfkh.dvilE33l3QYx3jEQvdTYxiCJtDzDYANhm4SCb3RFiToANF7BWYW3
J6BWYW3Jq0Xn2ElmGrNr6M4Zn.ScdKXpiJzDOo3wBLi3S.dLiEFbXqD.pnRO
uTKjR4VM.XX+uDtqKhxsExTtl13YV5CG3KJexoCtVC0UWMxUb4gwx7r2dQF2
I25wEo5tE39i0Nbi6ba2RbyVavQaWiix2k4cOjABd6a8LNX8T.7K2rET0JMs
m+l7lhaCECnIQG14ikG0D9bS3kQGLB5fbPVpG4fYlECseRrqp4Qsi+APnU5f
dXpS3335SsQ2OYbmiK4KCJ+Am4alTx.lXyMNeUKjaybx.37xEZkk+lO1gG7V
tmb9xri.4XULlUBjXCyYHgdI0NdBojG1HHUF45+pfAUgpgdoC.aFgXDciqRh
M4wEqqTfcsoT.GRKxYMxb0Vzvfyc9G.qabfhgWX4bGqckGqGcvXGviP.R1Mg
TDCo3di5RgziRYGFQgpCtArG2ddIkeXjKQP4mAN3F7MOkckw2VIkcXuKT1cg
xtKT1cgxtWfo2OjUSue.JTlWhcelxueBeIS237O4+6ev+iZCoRLW9XCQOaNG
pHxpLe18ROhRP3KdGNbuCQiw6PHv6Ug2gHc8NDYS+xP5lW3pDW6HEngHEfKd
XTmlA1EQxm311AQLbrPTtmcHiK4V057CpaOZpOReZeVCb7pKjZ5pqJfgA99i
p9Jlf9lM4q2Um60j8tV+qIc6cP2.W0hjg5DzfcbzQhF5Ym3QANlBw7.BSh7J
ODEOQbq858tqygqdv3gLkehcivFKi5w.0KbKGdPR6F2QBtgfGvYFd.FCdnLx
LHwT3SOCslGs9OLCBoM8K3gDwTb4KgKGGGmT8xnjp9Wi.lB+0UAK+iALQSGa
MR7vNAhKyThkGAwXR0q1YThSrSO0z4ERr+LJhIIGD5K0rM8YoaVZ58ctmA5D
JzdJBLd3puS0SQF15j8gb3djUSK8VDjGsrXaRjve8uTDNgUFocSZbb5CJi6D
jbj1h9FIoBIEKSyDB09zyEmJH3ZUiT93Dwo77O2794EAYE89MplNVIf0VNll
Esla.bbbXxZEOOhS4HWuYdLJDB3+JEQK+PyenpmntJ36MC.XHlW02XysA4gM
Ej8+I1Dww+dpoebaPbTwm5PTEzZkWjExe1xe7ZiwMXmUwPsQIhbAatyaAirm
Kxv8bUavEejMB.XU+yN2mOPWW2dNXrA510cHSMqRkeS0XC7WhK9zYfxAzD8f
GqIxadf6nd5CmPENOaYiTy5Tw+ifpWpqFMWfZMsuckPu+I9Y6BcTVufwp0LP
EpC.e2YPF1m4a1vGeUZXdRw7GRy9vbQ7kMWMjxb9SOGPQd3dBBeJxkpBmbHi
wGFQd9hy7AzisdS563o4BBekoNvxMYFv+xY+1kkY5xxLcYYldVOUen91bAkJ
SJfkIcS5YOgH7qwhEyR7eN7YSL2BIQY15f8oDw.TIwj91YI3N7XOuWknZ5KE
SIWZj+fC9Pj8Vdo8NUxOyKVIep4XGkQtyCxKOMCLZ12.aKEMP4QhghQX.9YA
+x2vAPChV.aiVJFpdcbZtC+SQL+ZZPiYmDCDZ7nF7k7Zg6dZIxkxdR9uRR1D
5tQ5o91LTo0dYnAuHVRdaFpzHcSuozy94gl1hVGHosOK6XuDzffZuwCP+ILf
seQuT6TpIWpcnZ0+JoYbjK0dO7K9ENijKdB1jIL2R6AT6crwsDZCIFT3B5Ft
rFjWncnnPfF9P3i0LC185flcnuc4YWcfycNnYuwdZ9.2V2sImAhkqomLsr1K
XC7DStJS05z4CqN4pfXuQoF02p3sNrn0gA8oFPIlKNtTQRrpyiZ7RiWoUiNL
xZrwFt.5RjYPG+F43QSWiyB2DKl2bboTbiESU3cU384yxbc+qVm1nDdic0Jz
9EfAs9rnNVq5St29f66Wk9OGIFsIHILdbIR8gErPHEA3nx0eWk8RIMWixJCw
pJpfj0kKIDqdcM1e8OYTWlmLEY4w+KBHdGyWbw1q+4lrzMoY0qn1LjeCLS9s
Z6prBB22n9Rf7vi17l6D09riuQRt6f1pgTPtOQAg7MTAAepBx.kCyWiZDAah
RRGrCaj5DViRBZB0ApN0IJwDkD4rUm.5ni2VbZyXvYnnAGVztiqn0Q4z.nKQ
KMFrMPWsJ5dZWGG3RzoCIo0l6excQq1jx8jsbFADQFDyLjx875OL1lC3YpcG
qy3rXSz+Eq03rFoNQOakjNJPXlM50neQa7wjvHMJZSXrAVCiM7.FpfdpJDyJ
C9oSICP1XvOj+y1H9ZUzVYDejVlqPMfJEBL.Ci5dtEfGRxApJ0DC.Ju8a8Ij
mjVLOIo.xOLVw94So34Rm.pyDSs8jauFJE7igR+vUmx2s9.p7j+trMq0mJCs
IBPRyg7SOKUPJ0FMpZUzP+tKZ33JZ54xyDvYyFJv4xc.v4x9S.PyNe8aAtJ8
bTlthPJVv16Sp7NAlIWSI4mZI1Jdd1KxwER7dQL9dQK9gQJd+QI99QHNuj+8
27+QjC5sd
-----------end_max5_patcher-----------
```