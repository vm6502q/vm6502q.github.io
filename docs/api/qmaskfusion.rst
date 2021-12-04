:orphan:

.. Copyright (c) 2017-2021

QMaskFusion
========================

Defined in `qmaskfusion.hpp <https://github.com/vm6502q/qrack/blob/main/include/qmaskfusion.hpp>`_.

The API is provided by Qrack::QInterface. This is intermediate optimization "layer" inteface that coalesces X, Y, and Z gates in parallel across qubit width, in order to make them less expensive to simulate.

.. doxygenfunction:: Qrack::QMaskFusion::QMaskFusion(bitLenInt, bitCapInt, qrack_rand_gen_ptr, complex, bool, bool, bool, int, bool, bool, real1, std::vector<int>, bitLenInt)
