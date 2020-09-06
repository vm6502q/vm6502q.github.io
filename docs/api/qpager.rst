:orphan:

.. Copyright (c) 2017-2020

QPager
========================

Defined in `qpager.hpp <https://github.com/vm6502q/qrack/blob/master/include/qpager.hpp>`_.

Qrack::QPager "pages" RAM between several QEngineCPU or QEngineOCL instances, by splitting the simulation into equal segments. QEngineOCL device IDs can by individually specified per page with the "devList" parameter. This allows for a different model of multi-device parallelism.

The parameter "qubitThreshold" is the number of qubits in each "page." A value of "0" will automatically size pages based on best estimates of efficiency.

.. doxygenfunction:: Qrack::QPager::QPager(QInterfaceEngine eng, bitLenInt qBitCount, bitCapInt initState = 0, qrack_rand_gen_ptr rgp = nullptr, complex phaseFac = CMPLX_DEFAULT_ARG, bool ignored = false, bool ignored2 = false, bool useHostMem = false, int deviceId = -1, bool useHardwareRNG = true, bool useSparseStateVec = false, real1 ignored3 = REAL1_DEFAULT_ARG, std::vector<int> devList = {}, bitLenInt qubitThreshold = 0);
