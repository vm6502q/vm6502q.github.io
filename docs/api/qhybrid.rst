:orphan:

.. Copyright (c) 2017-2020

QHybrid
========================

Defined in `qhybrid.hpp <https://github.com/vm6502q/qrack/blob/master/include/qhybrid.hpp>`_.

Qrack::QHybrid switches between QEngineCPU and QEngineOCL as optimal. It may combined with Qrack::QUnit. It supports the standard Qrack::QInterface API.

The parameter "qubitThreshold" is the number of qubits at which QHybrid will automatically switch to GPU operation. A value of "0" will automatically pick this threshold based on best estimates of efficiency.

.. doxygenfunction:: Qrack::QHybrid::QHybrid(bitLenInt qBitCount, bitCapInt initState = 0, qrack_rand_gen_ptr rgp = nullptr, complex phaseFac = CMPLX_DEFAULT_ARG, bool doNorm = true, bool randomGlobalPhase = true, bool useHostMem = false, int deviceId = -1, bool useHardwareRNG = true, bool useSparseStateVec = false, real1 norm_thresh = REAL1_DEFAULT_ARG, std::vector<int> ignored = {}, bitLenInt qubitThreshold = 0)

.. doxygenfunction:: Qrack::QHybrid::SwitchModes
