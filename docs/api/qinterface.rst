:orphan:

.. Copyright (c) 2017-2021

QInterface
========================

Defined in `qinterface.hpp <https://github.com/vm6502q/qrack/blob/master/include/qinterface.hpp>`_.

This provides a basic interface with a wide-ranging set of functionality 

.. doxygenclass:: Qrack::QInterface
    :project: qrack

Creating a QInterface
-----------------------

These are the primary implementations of a ``QInterface``:

.. doxygenenum:: QInterfaceEngine

These enums can be passed to an allocator to create a ``QInterface`` of that specified implementation type:

.. doxygenfunction:: Qrack::CreateQuantumInterface(std::vector<QInterfaceEngine>, Ts...)

Constructors
------------

.. doxygenfunction:: Qrack::QInterface::QInterface(bitLenInt, qrack_rand_gen_ptr, bool, bool, bool, real1_f)
.. doxygenfunction:: Qrack::QInterface::QInterface()

Copying
-------

.. doxygenfunction:: Qrack::QInterface::Clone

Members
-------

.. doxygenvariable:: stateVec

Configuration Methods
---------------------------------

.. doxygenfunction:: Qrack::QInterface::GetQubitCount
.. doxygenfunction:: Qrack::QInterface::GetMaxQPower
.. doxygenfunction:: Qrack::QInterface::isBinaryDecisionTree
.. doxygenfunction:: Qrack::QInterface::isClifford()
.. doxygenfunction:: Qrack::QInterface::isClifford(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::SetReactiveSeparate
.. doxygenfunction:: Qrack::QInterface::GetReactiveSeparate
.. doxygenfunction:: Qrack::QInterface::SetDevice
.. doxygenfunction:: Qrack::QInterface::GetDevice
.. doxygenfunction:: Qrack::QInterface::GetMaxSize

State Manipulation Methods
--------------------------

.. doxygenfunction:: Qrack::QInterface::SetPermutation
.. doxygenfunction:: Qrack::QInterface::SetQuantumState
.. doxygenfunction:: Qrack::QInterface::Compose(QInterfacePtr)
.. doxygenfunction:: Qrack::QInterface::Compose(QInterfacePtr, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Decompose(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Decompose(bitLenInt, QInterfacePtr)
.. doxygenfunction:: Qrack::QInterface::Dispose(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Dispose(bitLenInt, bitLenInt, bitCapInt)
.. doxygenfunction:: Qrack::QInterface::Prob
.. doxygenfunction:: Qrack::QInterface::ProbAll
.. doxygenfunction:: Qrack::QInterface::ProbReg
.. doxygenfunction:: Qrack::QInterface::ProbMask
.. doxygenfunction:: Qrack::QInterface::ProbMaskAll
.. doxygenfunction:: Qrack::QInterface::ProbBitsAll
.. doxygenfunction:: Qrack::QInterface::GetProbs
.. doxygenfunction:: Qrack::QInterface::ExpectationBitsAll
.. doxygenfunction:: Qrack::QInterface::Swap(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Swap(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ISwap(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ISwap(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::SqrtSwap(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::SqrtSwap(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CSwap(const std::vector<bitLenInt>&, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::AntiCSwap(const std::vector<bitLenInt>&, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CSqrtSwap(const std::vector<bitLenInt>&, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::AntiCSqrtSwap(const std::vector<bitLenInt>&, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::FSim(real1_f, real1_f, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::FSim(real1_f, real1_f, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::UniformlyControlledRY
.. doxygenfunction:: Qrack::QInterface::UniformlyControlledRZ
.. doxygenfunction:: Qrack::QInterface::UniformParityRZ
.. doxygenfunction:: Qrack::QInterface::CUniformParityRZ
.. doxygenfunction:: Qrack::QInterface::Reverse(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::TrySeparate(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::TrySeparate(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::TryDecompose
.. doxygenfunction:: Qrack::QInterface::MultiShotMeasureMask(const std::vector<bitCapInt>&, unsigned)
.. doxygenfunction:: Qrack::QInterface::MultiShotMeasureMask(const std::vector<bitCapInt>&, unsigned, unsigned long long *)
.. doxygenfunction:: Qrack::QInterface::ApproxCompare
.. doxygenfunction:: Qrack::QInterface::TimeEvolve

Quantum Gates
-------------

.. note:: Most gates offer both a single-bit version taking just the index to the qubit, as well as a register-spanning variant for convienence and performance that performs the gate across a sequence of bits.

.. note:: Qrack::QInterface also offers arithmetic logic unit (ALU) gates. See the Doxygen.

Single Gates
~~~~~~~~~~~~

.. doxygenfunction:: Qrack::QInterface::Mtrx(complex const*, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::MCMtrx(const std::vector<bitLenInt>&, complex const*, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::MACMtrx(const std::vector<bitLenInt>&, complex const*, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Phase(const complex, const complex, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::MCPhase(const std::vector<bitLenInt>&, complex, complex, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::MACPhase(const std::vector<bitLenInt>&, complex, complex, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Invert(const complex, const complex, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::MCInvert(const std::vector<bitLenInt>&, complex, complex, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::MACInvert(const std::vector<bitLenInt>&, complex, complex, bitLenInt)

.. doxygenfunction:: Qrack::QInterface::AND(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CLAND(bitLenInt, bool, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::OR(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CLOR(bitLenInt, bool, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::XOR(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CLXOR(bitLenInt, bool, bitLenInt)

.. doxygenfunction:: Qrack::QInterface::M(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ForceM(bitLenInt, bool, bool, bool)

.. doxygenfunction:: Qrack::QInterface::U(bitLenInt, bitLenInt, real1_f, real1_f, real1_f)
.. doxygenfunction:: Qrack::QInterface::U2(bitLenInt, bitLenInt, real1_f, real1_f)

.. doxygenfunction:: Qrack::QInterface::H(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::X(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Y(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Z(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::S(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::IS(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::SH(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::HIS(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::T(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::IT(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::SqrtX(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ISqrtX(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::SqrtY(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ISqrtY(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::SqrtH(bitLenInt)

.. doxygenfunction:: Qrack::QInterface::PhaseRootN(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::IPhaseRootN(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CPhaseRootN(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CIPhaseRootN(bitLenInt, bitLenInt, bitLenInt)

.. doxygenfunction:: Qrack::QInterface::CNOT(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::AntiCNOT(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CCNOT(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::AntiCCNOT(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CY(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CZ(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RT(real1_f, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RX(real1_f, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RY(real1_f, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RZ(real1_f, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRZ(real1_f, bitLenInt, bitLenInt)

.. doxygenfunction:: Qrack::QInterface::UniformlyControlledSingleBit(const std::vector<bitLenInt>&, bitLenInt, complex const *)
.. doxygenfunction:: Qrack::QInterface::UniformlyControlledSingleBit(const std::vector<bitLenInt>&, bitLenInt, complex const *, const std::vector<bitCapInt>&, bitCapInt)
.. doxygenfunction:: Qrack::QInterface::UniformlyControlledRY(const std::vector<bitLenInt>&, bitLenInt, real1 const*)
.. doxygenfunction:: Qrack::QInterface::UniformlyControlledRZ(const std::vector<bitLenInt>&, bitLenInt, real1 const*)

Arithmetic
----------

Qrack can build with quantum arithmetic methods, using CMake option ``-DENABLE_ALU=ON`.

.. doxygenfunction:: Qrack::QInterface::INC
.. doxygenfunction:: Qrack::QInterface::DEC
.. doxygenfunction:: Qrack::QInterface::CINC
.. doxygenfunction:: Qrack::QInterface::CDEC
.. doxygenfunction:: Qrack::QInterface::INCC
.. doxygenfunction:: Qrack::QInterface::INCS
.. doxygenfunction:: Qrack::QInterface::DECS
.. doxygenfunction:: Qrack::QInterface::MULModNOut
.. doxygenfunction:: Qrack::QInterface::IMULModNOut
.. doxygenfunction:: Qrack::QInterface::CMULModNOut
.. doxygenfunction:: Qrack::QInterface::CIMULModNOut

.. doxygenfunction:: Qrack::QInterface::FullAdd
.. doxygenfunction:: Qrack::QInterface::IFullAdd
.. doxygenfunction:: Qrack::QInterface::CFullAdd
.. doxygenfunction:: Qrack::QInterface::CIFullAdd

.. doxygenfunction:: Qrack::QInterface::ADC
.. doxygenfunction:: Qrack::QInterface::IADC
.. doxygenfunction:: Qrack::QInterface::CADC
.. doxygenfunction:: Qrack::QInterface::CIADC

Algorithmic Implementations
---------------------------

.. doxygenfunction:: Qrack::QInterface::QFT
.. doxygenfunction:: Qrack::QInterface::IQFT
.. doxygenfunction:: Qrack::QInterface::QFTR
.. doxygenfunction:: Qrack::QInterface::IQFTR
