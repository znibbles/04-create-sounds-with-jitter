## Sequencing in two dimensions

Let's add another twist to this, exploring the two dimensional space for sequencing. First, let's encapsulate the whole matrix lookup into its own subpatcher, and make another one for the noise matrices we are going to use for sequencing.

We instantiate a `[jit.noise]` object with 2 planes and 16 by 16 cell values. We are going to use these for pitch and filter sequencing, so we store them in two matrices. Let's look at the values we are dealing with here by unpacking the matrices and plotting them in a `[jit.pwindow]`.

## Looking up pitches

Audio tends to be rather loud in this patch, so let's soften it. We then make another subpatcher for the pitch sequence. We again create two phasors to peek into the two planes of the pitch sequence matrix. In this case, we reuse them for looking up the y coordinate of the other `[jit.peek~]`, respectively.

We then need to scale the float values from the matrices into meaningful oscillator frequencies, and take an exponential curve here, hence the last argument. Now we have the pitch lookup ready - let's take a listen. Let's also look at those scopes to get a feeling for the data stream here.

## ... and filter values

We then duplicate the whole process to make a filter sequencer. For simplicity, we take the same noise matrices and just read them out at lower rates as well as scale the values to accomodate low pass filter cutoff frequencies and resonances - but we could of course have taken a fresh matrix just as well.

Let's use a `[lores~]` IIR lowpass filter for this purpose, so we have a resonance factor we can play around with. So here we are with an almost R2-D2y generative pitch and filter sequence. Needless to say we could vary the content of those noise matrices based on a `[metro]` etc. - the possibilities are endless.

### Max Compressed Representation

```
----------begin_max5_patcher----------
2352.3oc6c0zjiZCD8rmeET9TxVSlRRH9J2xwbJ4PpJG1ZKWXaMdXGLvBx6L
6tUle6QPC1fMXKLHayrbXvCRXn6mZ0Rpesv+3tISmG9JKYp1uq8QsIS9wcSl
jUTZASxOexz0tutv2MI6xlFvdIb9mmdOTEm8JOq3O7lF5ARQwdKyJTbg+Ftn
rD927YYEWTRvl0dA9Ld1MljWXjKewSdAqlEyVvAAiX7.5dMrEJ8CJI8HQbT6
S6tOga3E2HbdoPQ7uEwf6xzDuUAt9S09TZ8+2c2kd39to09gwrj2zHHzCB02
xnF8mPjD.zOI.PrR+vw5FB.hzdzymyhmkv9RcJurs9nlUdLIqEWOSsMcNgxS
NgxeeIXn7yjEmq74Z+joBEi8UVbhWXPITcxT2nnREOozWIEx9bX1Mx59sE4E
.EousnX1W8J99zsk5FKzbtPs2DC.0q1lS2caBWxhC13kIJPghFu6JtgasTzw
YlJXyLzhBcYrQ6vKQC8J+vEOyVVB2E3UDKvKHRXNyB3t7boaa0KYO5twmO6w
v.dh22yDPbZ6RM0+n6BVie4.20f18GwdoMG4WxpXukgAoBQErNs3hGmvR.5F
XTVYxth.2nZ9xBCPAtzPkIBkbSxb23zlh4foIonRdXne0p1987YOxyqNxKHX
OTjGF0bkwdqd5He24ghJWer6cVMIy1D.0NSz4jOKw8qUQatquedu0p29WcC7
V6xYbOnIP30pnRVfqPQeJYQbnueE8Ep4q0TyRgU7B1KdK4Ok8fJaLHtbunBi
noaakW5shkvqVF2cUR0RNvUgnnMyy6kNiyVG4KzhpWPkgwJ2krresJkeL+aU
8wkrv0mkN7lFN0KOQ7mc4Krr+txkendbfaOyRUTmeeLLXmowCFocpOXnul89
KwH.SJFEH2YR+ibwgaBV9l.1Z.tzaIbQNNbg0cJ6768FZg6Wzx.ASp.MHAqJ
cJyl+k3P5wFQO69suI.eEcMolCL7SLecQ2jlvJSkzwz1n9IvOngJCUzqbfhT
e1i+PDi87a4qJPingaB25ae+FvjCMy5HhQx.cDIfNw5EtlvGpI3CojACFxnW
zStIgwoCIfLZ.0rUhaMCZVWV7.qy5N7hz.bYoBaL.srn2hnUxBw5DeqAzvjz
yVOVPuLnyVN3bb.AUpFw7eVrPLiwr6lIsTMy27XwZl0IWJzwnmGGz1YXANff
zj2511SBKSOIcpU5bO0gSPxCNgqWyB3GHAdAKYuVJ9RW.bwoewk70KqLfoNC
lcghx2KnoXPjIqo0WOhkDtIdQgis7ApzpJ0KYIbufsAl6i6lnd5EJUCVaEBK
YEBCEJD5xJDGdggwKgn5httRlXpZZ35kLrRjLhrRliBa3vRaCaoRovTRoPWk
BggrPgJEBYsWMIM2SBecEMxktOdVeWYjLCiaVQqlNhpt8rM9rwWTPqM9raAl
k2msfQtoojxrbFP.0LWNO1a9FNL4fxTL1JhRV4GN20OmFjszncLVS1Qsxc6j
29h22HOgfzDsuntS6Kv4Mv5qEYj02iw5qEJe4zNir9Nx56HquCQtKoTf6RqQ
tKkNb0CTvpB2kXr3Sa6iRcIsuSq.mxbWpa+dG+TJ0uCN36xR8K3Wqob2bj52
8MqFnH0NtKEKI3pw6qi8fl1W.5tJb9NHQtcTXpegX7E7lA79daxg4sGiu2xn
0HiuiL9Nx36PlwWmQFeGY78mcFeGIa8FirU5MAE7uCn7sFjbjy2QNeG478Zv
4aPnWBa1ZWgt8Zca0aROP6KMO3pvNF4Ta2Wz6P1bMQvlIxTejM2Q1bGYyU5U
dlEH0WDqcK7EYhuUaVmodN8rvFyx.hQgy4Grz9OLomV4sO6EYqeqq7mH55hd
atKdVizWAVWR7BdecXIyFCR+X3kPE1Nn69mcQwQ3wta2os3I2XMroVURC6v9
gWNf0A1kZF3tRYw0GJAFeNIN1yQMSWuDLRsukgwS6Vy7bQALUev6S2nal.ua
coSUQGlAnCcwRP4hYv0OovlTfDbjPKNdtr8M2MX0E0hJawsx3LFqBXKemWSt
HiooDFLbjLNKTEFwS5AAWpg.dpXYPpvGdKvghBkARKi610Mho1pVHjwrTojy
HIP3bqF43Vt4u9IL7r9ggOuIpt.y1auDNsLS4Y2vryuBJeGF21r3zhFiY6XL
aGiYaek70HwRHZZId8dtclmO+F06hafjbmoDn0DhYoFDCgFju.jNk8koRR25
At4kNoYyKkreHFpVW6xp+WDiTvS8lejb6mR6YryNqmILcMbdLYtASt+LEpmx
cejLlSzsoH5ISTT4rltVoY6wPNm9E4x8yeUgtKWla2yjAB8DIvGWlL295FdO
UFDCYSyUpJCtFUZoHObjpQJjMYsopLzVTy1fEJSJjMmeoJMvuztGwy2uQspx
dIpxTeb5dvox+MBI+mHlC27PMj0f0togNbCC0OwmqDq+6lQHV6Q+PWtNIaoH
YGpChzkDhvmNwJI4+7wb1uRcpkfoVBQqYIItqXGfQrWih0R7B9kjfv30eD8o
O.+C9Se3e92+5u+yes3aj1sbaKGoFDizciJJ1Bl8LM6Ca3cKoQ6C5oBvnEgI
cGiv8FFYaCYSvsDDkZFUfPUAlxTf2CNezcxv.J7iOA8riM94gAGliHUzOq15
mPGaW5Wz.ST5Il3GLZqahy24vQ7el1vVmVZ1C7aPyiOCrLQyqo6wSOBhbian
azaia3.oznowMHt7k0LdbnXYU0MzYQYMRBR2sYLgWCQ5R1wudtbNZFvzRbhG
tZkOqNChdyd.dw7A+3wPPmTmibiEXe5OTb.wFkmE1AJtW5PVmidy99R2E0MG
Sy9h+y7LPkZTbroYXVR5ytjpKEGTl8WfQtJc3BKpP8cS6oxJqDa5ZukQgBfL
+QRgo8ggbWSGdI7u2YDvmGjLf6eFzPKFiyH+rFZfjVmv8lNcYkaYD6RQe3re
P5NcGfPWA.ZOQpg0gWIAk1KCST6iV25XPVAPX.uVAf2ql6cFE50Pr2d1gJBp
aJhQKZ661SRlFqd34PjwEF0odqBj5ezXUXORjwGWl3g6J7hKmgYG6IQ55SBI
CZh6CSFjD5DF2CnGV51otpSx7fLUgonrOX7w68ASkYurvJ8YtW1WsWlWcXVW
0bFWse1VIdx+2c+OYuekU
-----------end_max5_patcher-----------
```