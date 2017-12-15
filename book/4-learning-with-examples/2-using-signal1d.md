# Filters for Signal1D
These examples use Signal1D objects. Such signal can be used for a lot of applications where time is recorded on a single sensor (e.g. EEG, MEG). They can also represent these data in the frequency domain.

- [Compute the 1D Fourier transform and inverse transform on a signal](https://github.com/Pixpipe/pixpipejs/tree/master/examples/fftSignal1D.html) | [source](https://github.com/Pixpipe/pixpipejs/tree/master/examples/fftSignal1D.html)

The two following are doing basically the same but the first using two filters (one for lo, one for hi), while the second uses a single filters that takes two cutoff frequencies:
- [Performing a high-pass and/or low-pass filter in Fourier domain (EEG)](https://github.com/Pixpipe/pixpipejs/tree/master/examples/fftHiLoPassSignal1D.html) | [source](https://github.com/Pixpipe/pixpipejs/tree/master/examples/fftHiLoPassSignal1D.html)
- [Performing a band pass filter in Fourier domain (EEG)](https://github.com/Pixpipe/pixpipejs/tree/master/examples/fftBandPassSignal1D.html) | [source](https://github.com/Pixpipe/pixpipejs/tree/master/examples/fftBandPassSignal1D.html)


- [Performing a high-pass and/or low-pass filter using difference equation and convolution (EEG)](https://github.com/Pixpipe/pixpipejs/tree/master/examples/differenceEqSignal1D.html) | [source](https://github.com/Pixpipe/pixpipejs/tree/master/examples/differenceEqSignal1D.html)
