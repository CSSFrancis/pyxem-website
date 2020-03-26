Non Crystalline Materials
===========================

This section is designed to explain the workflow for analyzing a non-crystalline dataset.  Primarily we can
separate things into three different methods.

* `Fluctuation Electron Microscopy (FEM)`_
* `Angular Correlations and Angular Power Spectrum`_
* `Electron Correlation Microscopy (ECM)`_

.. _`Fluctuation Electron Microscopy (FEM)`:

**Fluctuation Electron Microscopy(FEM):**

FEM characterize the atomic structure in disordered materials by measuring the statistical fluctuation in scattered electron intensity `[1]`_. The normalized variance :math:`V(k)` containing three- and four-body correlation functions, which could be used to interpret medium range order, is defined as

.. math::
    
    V(k)=\frac{\left<I^2(k,r)\right>_r}{\left<I(k,r)\right>^2_r},

where :math:`k` is the length of the reciprocal space vector and :math:`r` is the probe position of a frame. Values within :math:`\left<...\right>` are averaged over probe position :math:`r` (i.e. averaged over all frames).

There are two different manner to calculate :math:`V(k)` from a 4D dataset: inter-frame and intra-frame. Inter-frame variance takes the variance at each k (or each k space position) over all electron diffraction frames. Intra-frame variance takes the variance over the angular ring at each k and average over all frames. There is no evidence suggests any differences in the mathematical encodings of the structural information between these two. See `[1]`_ for more information.

*Calculating* :math:`V(k)`





.. _`Angular Correlations and Angular Power Spectrum`:

Angular Correlation gives information about the symmetries of the local atomic structures in the sample`[2]`_. Angular correlation function is shown below.

.. math::

   C(\phi,k,r)= \frac{\left<I(\theta,k,r)*I(\theta+\phi,k,r)\right>_\theta-\left<I(\theta,k,r)\right>^2}{\left<I(\theta,k,r)\right>^2}

Where :math:`\theta` is the entire :math:`2\pi` radians that :math:`\phi` (the angular lag) is averaged
over. :math:`k` is the length of the reciprocal space vector and :math:`r` is the probe position of the frame. The symmetry present at every probe position :math:`r` is accessed by taking the Fourier series of the angular correlation function at each :math:`k`. This gives the angular power spectrum :math:`P(n,k,r)=FT(C(\phi,k,r))`, where :math:`n` is the order of symmetry. For each order, angular power spectrum can be mapped spatially to represent the atomic structure symmetry in the sample region.

While the Electron Microscope community has decided to use the terminology of angular correlations, what is being
calculated is in actuality the self-correlation as a function of angle instead of time.

As result efficient methods for convolution can be applied in which the actual computation occurring is:

.. math::

   C(\phi,k,n)=\frac{IFFT[FFT(I(\theta,k,n))_\theta * Conj(FFT(I(\theta,k,n)))_\theta]}{<I(\theta,k,n)>^2}

Which speeds up the calculation more than 100x


*Calculating the Angular Correlations*

In order to calculate angular correlations, start by loading a 4-D data set.

.. code:: python

    import hyperspy.api as hs
    import matplotlib.pyplot as plt

    dif_signal = hs.load(file, signal_type ='diffraction_signal')

    # adding a mask to the signal for to block the beam stop
    dif_signal.mask_below(.1)
    dif_signal.show_mask()

    # correcting for not elliptical diffraction patterns. Make sure there is not wobbling from pattern to pattern
    dif_signal.determine_ellipse()
    pol_signal = dif_signal.calculate_polar_spectrum()
    ang_signal = polar_signal.autocorrelation()


.. _`Electron Correlation Microscopy (ECM)`:

ECM measures structural dynamics with spatial (and possible reciprocal) resolution`[3]`_. Local structural persistence can be evaluated by the lifetime of the corresponding speckle. This is statistically represented by time autocorrelation function:

.. math::
    
    g_2(t,r)=\frac{\left<I({t}',r)I({t}'+t,r)\right>_{{t}'}}{\left<I({t}',r)\right>^2},

where :math:`{t}'` is the time of a frame, :math:`t` is the delay time after :math:`{t}'`, :math:`r` is the probe position of a frame, and :math:`\left<...\right>_{{t}'}` denotes average over :math:`{t}'`. The calculated :math:`g_2(t,r)` is fitted to the Kohlrausch–Williams-Watt (KWW) equation:

.. math::
    
    g_2(t)=1+A\exp\left[-2\left(\frac{t}{\tau}\right)^\beta\right]

to give a spatial map of the relaxation time :math:`\tau(r)` and the shape parameter :math:`\beta(r)`.

Note the above analysis is based on the tilted dark field ECM experiment where a 3D data set are acquired time-resolved on a fixed reciprocal space position :math:`q`.



**References**

.. _`[1]`:

[1] Daulton, T. L., Bondi, K. S. & Kelton, K. F. Nanobeam diffraction fluctuation electron microscopy technique for structural characterization of disordered materials-Application to Al88-xY7Fe5Tix metallic glasses. Ultramicroscopy 110, 1279–1289 (2010).

.. _`[2]`:

[2] Im, S. et al. Direct Determination of Structural Heterogeneity in Metallic Glasses Using Four-Dimensional Scanning Transmission Electron Microscopy. Ultramicroscopy 195, 189–193 (2018).

.. _`[3]`:

[3] Zhang, P., Maldonis, J. J., Liu, Z., Schroers, J. & Voyles, P. M. Spatially heterogeneous dynamics in a metallic glass forming liquid imaged by electron correlation microscopy. Nat. Commun. 9, 1–7 (2018).