Noisy Simulation
----------------

Qrack's primary modes of operation are intended to simulate ideal, noiseless, pure quantum states. However, Qrack can achieve imperfect fidelity on larger circuits, with the aid of the `QRACK_QUNIT_SEPARABILITY_THRESHOLD` environment variable and `a calibration table that has been provided by the authors of Qrack. <https://docs.google.com/spreadsheets/d/1u2Qum9W768rMWIoKlz658i1P6RTmX1ekgartNMHuR-s/edit?usp=sharing>`_

The calibration table can estimate "heavy output generation" ("HOG") rate as a function of circuit width, depth, and separability rounding parameter, modeled based upon "square circuits" up to 20 qubits. The correction for excess width, compared to a square circuit, is

.. math::

    \ln \left( \left[ Infidelity \, of \, your \, circuit \right] \right) = \ln \left( \left[ Infidelity \, of \, square \, depth \, circuit \right] \right) * \ln \left( \left[ depth \right] \right) / \ln \left( \left[ width \right] \right).
    
A pessimistic estimate for fidelity, based upon HOG rate, is provided as

.. math::

    sqrt([Noisy \, HOG \, rate] / [Ideal \, noiseless \, HOG \, rate]).

"Infidelity" is defined as 1 minus "fidelity." Note that the calibration table and included models directly model and predict HOG rate; we only provide a safely pessimistic lower bound on average "fidelity." The rounding parameter values in the table, which correspond to noise quantity, imply values of `QRACK_QUNIT_SEPARABILITY_THRESHOLD` from 0/default (least severe noise, ideal simulation) to 0.5 (most severe noise, all entanglement effectively destroyed). Following the examples of the included analysis for Google's 2019 "quantum supremacy" class of circuits and other random circuits, it's possible to make a problem-specific resource projection for attainable fidelity. If a user only wants to estimate the fidelity of a single circuit simulated with a nonzero rounding parameter value, then we use the excess width correction model and (pessimistic) fidelity estimate.
