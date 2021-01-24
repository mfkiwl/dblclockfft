# A Generic Piplined FFT Core Generator

This generic pipelined FFT project contains all of the software necessary to
create the IP to generate an arbitrary sized FFT.  The FFT has been modified
for operation in one of the following modes:

- Two samples in per clock and, after some delay, two samples out per clock.
  This uses 6 multiplies per FFT stage in the butterflies.  This was the purpose
  of the original `dblclkfft`.  (Why double clock?  I don't know.  Double-sample
  FFT might've been a better name.)

- One sample in per clock, with the `i_ce` line being high for every incoming
  sample--up to one sample per clock.  There's also options to run with at
  least one clock between samples, or even two clocks between samples (or more).
  This mode uses 3, 2, or 1 multiplies per FFT stage respectively.

- Eventually, I want to support a real FFT mode which will accept real samples
  input, and alternately produce real and imaginary samples output--or the
  converse for the inverse FFT.

The FFT generated by this project is very configurable.  By simple adjustment
of a command line parameter, the FFT created will either be a forward FFT or an
inverse FFT.  The number of bits processed, kept, and maintained by this
FFT are also configurable.  Even the number of bits used for the twiddle
factors, or whether or not to bit reverse the outputs, are all configurable
parts to this FFT core.

These features make this open source pipelined FFT module very different
and unique among the other open HDL cores you may find.

For those who wish to get started right away, please download the package,
change into the ``sw`` directory and run ``make``.  There is no need to
run a configure script, ``fftgen`` is completely portable C++.  Then, once
built, go ahead and run ``fftgen`` without any arguments.  This will cause
``fftgen`` to print a usage statement to the screen.  Review the usage
statement, and run ``fftgen`` a second time with the arguments you need.

# Windows Users

My test platform is an Ubuntu system.  I don't normally test the core
generator on Windows platforms.  Others have used it on Windows platforms
quite successfully.  There are two problems they have encountered when doing
so:

1. I use a `mkdir(dirname, chmod)` function in the core generator to make a
   directory in which to place the generated core.  Windows doesn't seem to
   support this function, but rather a similar `mkdir(dirname)` function.
   Switching between the two is simple enough.

2. The generator also uses the `lstat(path, statbuf)` function to make certain
   that it doesn't overwrite any pre-existing files having the same name as
   the directory it wishes to create.  This function can be replaced with the
   constant `1` or boolean `true` in order to bypass the check and build the
   design on a Windows system.

There is a `#define` option set at the top of the generator that applies these
changes for some versions of Microsoft Visual C++.

# Current State

This particular version of the FFT core now passes all my tests.  It has
yet to meet hardware to be finally verified.

- The [FFT test bench](bench/cpp/fft_tb.cpp) doesn't yet have a threshold that
  adjusts with input parameters to determine success or failure (yet).

- I haven't started on the real-only version of this FFT.

While my previously stated goal ws to continue working with this core until it
has a real-FFT capability before releasing it back into the master branch,
I'm actually so excited that I got it to this point that I'm going to move
it from dev to master earlier, and come back to get the real only version.

# Common Confusions

This core does not contain any overflow protection.  Should you send it data
large enough to overflow its internal registers, you will likely get something
unexpected in return.

This is perhaps for the best.  While overflow detection is a possible addition,
overflow protection will only minimize an already bad situation.  It would be
better to avoid that bad situation in the first place by providing the core
with enough bits of precision to do its mathematics with.


# Commercial Applications

Should you find the LGPLv3 license insufficient for your needs, other licenses
can be purchased from Gisselquist Technology, LLC.

Likewise, please contact us should you wish to fund the further development
of this core.
