:orphan:

.. Copyright (c) 2017-2021

QHybrid
========================

Defined in `qhybrid.hpp <https://github.com/vm6502q/qrack/blob/master/include/qhybrid.hpp>`_.

Qrack::QHybrid is a Qrack::Engine that switches between QEngineCPU and QEngineOCL as optimal. It may be used as sub-engine type with Qrack::QUnit. It supports the standard Qrack::QInterface API.

The parameter "qubitThreshold" is the number of qubits at which QHybrid will automatically switch to GPU operation. A value of "0" will automatically pick this threshold based on best estimates of efficiency.

.. doxygenfunction:: Qrack::QHybrid::QHybrid(bitLenInt, bitCapInt, qrack_rand_gen_ptr, complex, bool, bool, bool, int, bool, bool, real1, std::vector<int>, bitLenInt)

.. doxygenfunction:: Qrack::QHybrid::SwitchModes
