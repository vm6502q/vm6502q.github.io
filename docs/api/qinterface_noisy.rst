:orphan:

.. Copyright (c) 2017-2023

QInterfaceNoisy
========================

Defined in `qinterface_noisy.hpp <https://github.com/vm6502q/qrack/blob/main/include/qinterface_noisy.hpp>`_.

The API is provided by Qrack::QInterface. Qrack::QInterfaceNoisy adds depolarizing noise to every gate executed on the simulator instance. Noise level can be controlled with `SetNoiseParameter(real1_f)` or the environment variable `QRACK_GATE_DEPOLARIZATION`.

.. doxygenfunction:: Qrack::QInterfaceNoisy::SetNoiseParameter(real1_f)
