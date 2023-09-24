# Gravity Assignment 3

  * 30% of final grade
  * assigned 29 Sep 2023
  * due 13 Oct 2023

---

## Problem 1

There are snippets of simulated data for a hypothetical into which we will inject a few signals.
Your job is to write a matched-filter search from scratch to detect these signals.

The data is stored within an HDF file

  * [assignment-3.hdf](assignment-3.hdf)

---

Write a matched filter search and use it to determine

  * the number of signals present
  * a reference time for each detected signal
  * the statistical significance of each signal (i.e., a False Alarm Rate or Probability)

Signals will be sine-Gaussians of the form

```math
h(t) = A \cos(2\pi f_o (t-t_o) + \phi_o) \exp\left( -\frac{(t-t_o)^2}{2\tau^2} \right)
```

The Fourier Transform of this is

```math
\tilde{h}(f) = A \sqrt{\frac{\pi}{2}} \tau e^{-2\pi i f t_o} \left( e^{-i\phi_o - 2\pi^2\tau^2(f+f_o)^2} + e^{+i\phi_o - 2\pi^2\tau^2 (f-f_o)^2}\right)
```

We assume that

```math
2\pi \tau f_o = \frac{f_o}{\sigma_f} \gg 1
```

and only consider positive frequencies so that the Fourier-domain signal simplifies to

```math
\tilde{h}(f) \approx A e^{i\phi_o} \left[\sqrt{\frac{\pi}{2}} \tau e^{-2\pi i f t_o - 2\pi^2\tau^2 (t-t_o)^2}\right]
```

Additionally, you can assume that 

  * all signals have a known central frequency and width
    - `fo = 20.0 Hz`
    - `tau = 2.0 sec`
  * the noise is stationary, Gaussian, and white.

You should analytically maximize over `A` and `phi_o`.
However, you will have to numerically maximize over `t_o`.

---

We have provided some basic Python code within this repository as a module ([utils.py](utils.py)) and an initial search script ([search](search)).
You should modify [search](search) as necessary to get the search running.

It may be helpful to note that if

```math
x(f) = x^\ast(-f)
```

then

```math
2\mathcal{R}\left\{\int\limits_0^{+\infty} df\, e^{-2\pi f t} x(f) \right\} = \int\limits_{-\infty}^{+\infty} df\, e^{-2\pi f t} x(f)
```

Note that the right-hand-side looks like a Fourier transform.

Similarly,

```math
\mathcal{I}\left{x\right} = \mathcal{R}\{-i x\}
```
