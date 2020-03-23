Non Crystalline Materials
===========================

This section is designed to explain the workflow for analyzing a non-crystalline dataset.  Primarily we can
seperate things into three different methods.

* `Fluctuation Electron Microscopy (FEM)`_
* `Angular Correlations and Angular Power Spectrum`_
* `Electron Correlation Microscopy (ECM)`_

.. _`Fluctuation Electron Microscopy (FEM)`:

**Fluction Electron Microscopy(FEM):**

Insert information about FEM `[1]`_



.. _`Angular Correlations and Angular Power Spectrum`:

The Anuglar Correlation as described in `[2]`_ can be shown as the function below.

.. math::

   C(\phi,k,n)= \frac{ <I(\theta,k,n)*I(\theta+\phi,k,n)>_\theta-<I(\theta,k,n)>^2}{<I(\theta,k,n)>^2}

Where :math:`\theta` is the entire :math:`2\pi` radians the that :math:`\phi` (the angle of correlation) is averaged
over. k is the radius of the reciporical space vector and n is the diffraction pattern number (assuming the correlation
is being calculated for a series of diffraction patterns)

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

Insert information about ECM `[3]`_



**References**

.. _`[1]`:

[1] Daulton, T. L., Bondi, K. S. & Kelton, K. F. Nanobeam diffraction fluctuation electron microscopy technique for structural characterization of disordered materials-Application to Al88-xY7Fe5Tix metallic glasses. Ultramicroscopy 110, 1279–1289 (2010).

.. _`[2]`:

[2] Im, S. et al. Direct Determination of Structural Heterogeneity in Metallic Glasses Using Four-Dimensional Scanning Transmission Electron Microscopy. Ultramicroscopy 195, 189–193 (2018).

.. _`[3]`:

[3] Zhang, P., Maldonis, J. J., Liu, Z., Schroers, J. & Voyles, P. M. Spatially heterogeneous dynamics in a metallic glass forming liquid imaged by electron correlation microscopy. Nat. Commun. 9, 1–7 (2018).