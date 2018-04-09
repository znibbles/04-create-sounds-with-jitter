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

```
----------begin_max5_patcher----------
1827.3oc4Z80iiZCD+4c+Tfh5ioHFaCX5SspeIpT0oUjDmbbGAmBjc26Nc6m
8N1FHgFfX1.q165pUIDyDFO+l+ONe696VrR9rnXgyu472N2c22t+t6zKoV3t
pOe2h8wOuNMtPS1hUGKKkYKVZtUQ4WRE50qWQdrLUTV9kCByScwp3rcKb9P0
sODWt9iIY6dHWrtzPAMvy0aoCgqekU+ZyWIYilAxUe5WIzZ1jcbeRFxH8lBN
sng8lUUK986uW8xRKkt3xx7iIiP5FTxXnLQoAAgKcfHv0Gey2HqjdjOnlQpM
hd4m2FuQLyRcZxiB2MIwo074w37r38FQ+uNeCbo7uzYw1TYbo0.Akq.BlVOy
B6AG7Vz7vxwMRoH+AQV7Ji1.rGMH0Zx3GEadPApInAr3zUEUvREtnD8ziB41
5kqW+7cRpLaWmvSKpJ9nLu75jUikdcbu8nVRKatcc2jrjRTmcIvzEUF8f2I3
tEQGQhZL165oHd9fLSjoUmnsqg.sUV8aizjKS7Dpnq0xkhm0O5EeJozskEuE
tf324g8wn97Ykw3f1gDlw8yK.MG877Hn8HDNjGoemtdjtM1dE3vzGNkDp7u3
QWOZJctil1iR9P75O6jIKEn8H9+TEs0H3fIORk72iVkx5TxoSljigDwGxXjr
kVnZABbR2NblDp+PQP8FeDzsx78w58Qv6F7HfXMbD7tBNldudfnEepEd8vOU
d8FAOfbcmdRzOlN8XxpFU6UJej++.mdVn0vQ36J3Xvpr+yikxsau0xrMVJXs
0T7OB+ZkYCQ+vTlcK7Yf5r6mtgKzNIScOc4g8WGNCKY7MsTb5aQo36EEEw6D
WDvVdPj4Plr3zgZeUfQzFmL107dgQT4ML0hMLYhM3ORwdtk69ROKS+xKNGDh
OiBfq5StORbwuIJTNDmiGbXN+dQoHN8D3jljIVKOVYKZOhUjfN1oVjVGB0CK
.BzI1o98.Y7YthlC4hBQ4HLIp+BNKRxzu0dggDbe.zxpNvMni2nCK0kfGNB4
lUyR8N4gMwkw0i867vu3WZkHucDrSzh5BSMjJIoTtaGBDMBIrr49.qJgkN+e
cpKTPb8ILRH+LJ0OJcpwhzjMHqGl7nFxMYROQL3yc4THhvNQtpBft1G9ZmxF
p3cSkmx1qQjC5lnJG7Fx766YEdFC85UJ7bi7oTf47g5.6K6UCQlAMTjKkCbh
kJnKod.8CGbYPD+5ZGvyFsC3Rtt1IxNcCex0MzYP2PbY9gbVfkJmNHuesCE8
dfHlO+mKmGyUSSFxTIF57EUQfpMzhYHcG07FevIRB9ybarsLmtrSkNt8qrck
PhonHfhMvqLWnZ20n9RyS+goakh0woVbn.8RlM8p.C1pBo6FUZOn+au2g0x8
6U8hzqyPWcoh01nT1Dc2p0mHVOk3.dinFGuoyYuzI0Icjitvpid.E2f.znGu
LRAC8NwxwTT6q4jGTG0xgmRx1He50KUrnHrnKSeMDJF+MJHHDulyPoD+ScYP
0kcpdIyrTNv4LkollSZxWm2yZBBTJ7nHUgO7lgz1oFuAK1JyNEj3OxOajQch
RJxKTxgZMpq2jAd80hrNtkyu.l+mpVkOOcfpXQz4HhODfQXiDvHiAvd+LA3V
.iuovJug.FvBfYziD0NqrIYRoC3ztZ6NGvQWaAkfcJPXUuLuNwdUii.FB14u
27emueMKF7fv7GBOhtNdz7ybYUbQRwfnjxL3gmR1T9Q83LBcArRbx4tzaiW2
xLtGbMUt9yuIm30+f9W4R0Pblzy2GAc0OfFtNwKEaBtxprmBIXy77DKNfas7
i62jG+zKuxSt23kQMGcOyqIwY2g8GoaFyVSDxDlIXfPXlPONOgceTph71U7r
YJZlI4AIx38RHCZ2Pm4Rypm.h0R50FcZkmAD5hkkpK3hLXuEzI53AeM1Ghut
Id8KipuIyvAp7NLQdY8zpbvH74OuoI8yROV++yuIT81QsdaQpPdLecs5oJju
yoczFQQYRVbYhL6LZ3snYexlCRTyVT2bXHFWyjjwzfHyLP71epZDILcKTb8w
a1oRv18M2h8sp9bmakQ91vn16lmTOZMF8fQESt7FaSi20RGKy2TMF+Ye6BjV
D0vYuahyL6sjtI9PsQ0qfAneEBaHExsu6fqr6TQpGxdgNeau.aLmYFY313ju
sbhbqbxFKOXRL8n1JS2LmHVZjOfUDLeVQfE6NRzTfCVvno.uIQuU1PDtsX2s
5WnONHq3zs5qSX2ZUDgTP2cjZhIDOcsBAbyuJxVeZpKifPeixZQ.6cnuMF4Y
iVGl.NAV4y3OA1W.2VNcyxjMYsB5t7IxaCmgK4LL+bdVJYDrItAYJhuC13mC
SBmf2pLIuRFYZYL9vgGE4EUTq4A1u6mjZsZ3R8GSxjm9oQrHW7XRM895Uhyw
NdKw1cOla5E9YtoO1E6kn8Q1wjJiSjye+9+kwyIyQ
-----------end_max5_patcher-----------
```

### Poly Patch

```
----------begin_max5_patcher----------
744.3oc2WssahCCD84vWgUdbEKJ14Bo6uxpJjA7RcahcThCETU4aesGmztsE
BNEiPZkP13Kwm4Ly3YF+xjfvkxcrlPzuP+FED7xjf.XJyDAciCBKo6VUPafs
EVxZZnaXgSsq0n1WvfE5mQ1pJXJ09Jl8XCCQ22sTEUs5AtXyhZ1JkcULIYVz
TTbpoMMxzRHyhd6S3qgCWt7wehS6gPzVxEZP.Ih79jVngYwlIecxDSyTGolf
8rFmdPTrcfLF9iCnnYoifvM7MBZwfzlDOKUyzDf7IjAoM91P6VQEc0SZpq+M
Bx+mBIUENs+OmWIjcmg8yyFTGDeTc.9n5.x2PG7Iu52TBksJFRKXXe4uSh.O
cKoyvCRZxswvqPEnkiguZi8RpXiC1534vk8gulGcks0mh1OvapjE6OLBpyEf
qtoyAiN11klOD8u6p6oqX0GF8E5Aig2c4MFbmyAqKN93ziLB54QeZ8odXnKw
mNDcdtoyFn9T1r7QPpndYftksdg9q0.tfpT07k5XMMcTqiaAglUVrRVVxDVl
DBK.ZEOoadjqlUwXOc.8rVlTzkELDAEM9DdSOWrOPihyyft33gToyGQrOhWy
0iizkj32r8XL39fSfa9oClpK6lUjyUf3cVbW38Mp3tpGnMx5CiqFmQXxifLd
YCFxO4+Kp2Yzcg4w2FlyEdrfNagMPyv007My7AmUXAW742oABiY9Op.Zjs0q
5E8tPon2kn0rFEWPUbo3e1C4C6QVulUCIqNpN2m.adcywPFeQHm4.xFoCit+
hvI0QbhtPbRb.mLOfSrC3j5AbLumyn7GDnDeAz4XTrO.h3.P24CfvN.TtO.J
xkquoevPVxWWI0uFpKRksDuLHrX1W9OAdt.NKApyFOy6webiAwWi.P.zmyAu
2m4xPxENF8EfrYUnUUaY0Mc6FvPm.8QInAlOEFxE1gwvvZ1Vd+9SgYn05ThJ
c9v1Zadzc41ZXCKkZconk2oN0H+5j+hHHBZn
-----------end_max5_patcher-----------
```