# Gravity Assignment 3

  * 30% of final grade
  * assigned 29 Sep 2023
  * due 13 Oct 2023

---

## Problem 1

There are snippets of simulated data for a hypothetical into which we will inject a few signals.
Your job is to write a matched-filter search from scratch to detect these signals.

Write a matched filter search and use it to determine

  * the number of signals present
  * the statistical significance of each signal (i.e., a False Alarm Probability)
  * the physical amplitude, signal to noise ratio, and reference time for each detected signal

Signals will be sine-Gaussians of the form

```math
h(t) = A \cos(2\pi f_o (t-t_o) + \phi_o) \exp\left( -\frac{(t-t_o)^2}{2\tau^2} \right)
```

The Fourier Transform of this is

```math
\tilde{h}(f) = A \sqrt{\frac{pi}{2}} \tau e^{-2\pi i f t_o} \left( e^{-i\phi_o - 2\pi^2\tau^2(f+f_o)^2} + e^{+i\phi_o - 2\pi^2\tau^2 (f-f_o)^2}\right)
```

We assume that

```math
2\pi \tau f_o = \frac{f}{\sigma_f} \gg 1
```

and only consider positive frequencies so that the Fourier-domain signal simplifies to

```math
\tilde{h}(f) \appro A \sqrt{\frac{pi}{2}} \tau e^{-2\pi i f t_o + i\phi_o - 2\pi^2\tau^2 (t-t_o)^2}
```

Additionally, you can assume that 

  * all signals have a known central frequency (`fo = 20.0 Hz`) and width (`tau = 2.0 sec`)
  * the noise is stationary, Gaussian, and white.

You should analytically maximize over `A` and `phi_o`.
However, you will have to numerically maximize over `t_o`.

---

We have provided some basic Python code within this repository.
It will allow you to do things like the following:

```
import utils

#---

# compute a waveform

A = 1.0
fo = 10.0
to = 0.0
phio = 0.0
tau = 1.0

Npts = 1001

t = np.linspace(-1, +1, Npts) * 3*tau
h = utils.sine_gaussian(t, A, to, fo, phio, tau)

#---

# read data from an hdf file

t, h = utils.load_data('assignment-3.hdf')
```
