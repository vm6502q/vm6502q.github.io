:orphan:

.. Copyright (c) 2018

QInterface
========================

Defined in `qinterface.hpp <https://github.com/vm6502q/qrack/blob/master/include/qinterface.hpp>`_.

This provides a basic interface with a wide-ranging set of functionality 

.. doxygenclass:: Qrack::QInterface
    :project: qrack

Creating a QInterface
-----------------------

There are four primary implementations of a ``QInterface``:

.. doxygenenum:: QInterfaceEngine

These enums can be passed to an allocator to create a ``QInterface`` of that specified implementation type:

.. doxygenfunction:: Qrack::CreateQuantumInterface(QInterfaceEngine, QInterfaceEngine, QInterfaceEngine, Ts...)

Constructors
------------

.. doxygenfunction:: Qrack::QInterface::QInterface(bitLenInt)

.. doxygenfunction:: Qrack::QInterface::QInterface(bitLenInt, bitCapInt)

.. doxygenfunction:: Qrack::QInterface::QInterface(const QInterface&)

Members
-------

.. doxygenvariable:: stateVec

Configuration Methods
---------------------------------

.. doxygenfunction:: Qrack::QInterface::GetQubitCount

.. doxygenfunction:: Qrack::QInterface::GetMaxQPower

State Manipulation Methods
--------------------------

.. doxygenfunction:: Qrack::QInterface::SetPermutation

.. doxygenfunction:: Qrack::QInterface::SetQuantumState

.. doxygenfunction:: Qrack::QInterface::Cohere(QInterfacePtr)
.. doxygenfunction:: Qrack::QInterface::Cohere(std::vector<QInterfacePtr>)

.. doxygenfunction:: Qrack::QInterface::Decohere

.. doxygenfunction:: Qrack::QInterface::Dispose

.. doxygenfunction:: Qrack::QInterface::Prob

.. doxygenfunction:: Qrack::QInterface::ProbAll

.. doxygenfunction:: Qrack::QInterface::Swap(bitLenInt, bitLenInt)

.. doxygenfunction:: Qrack::QInterface::Swap(bitLenInt, bitLenInt, bitLenInt)

.. doxygenfunction:: Qrack::QInterface::Reverse(bitLenInt, bitLenInt)

Quantum Gates
-------------

.. note:: Most gates offer both a single-bit version taking just the index to the qubit, as well as a register-spanning variant for convienence and performance that performs the gate across a sequence of bits.

Single Register Gates
~~~~~~~~~~~~~~~~~~~~~

.. doxygenfunction:: Qrack::QInterface::AND(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CLAND(bitLenInt, bool, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::OR(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CLOR(bitLenInt, bool, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::XOR(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CLXOR(bitLenInt, bool, bitLenInt)

.. doxygenfunction:: Qrack::QInterface::H(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::M(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::X(bitLenInt)

.. doxygenfunction:: Qrack::QInterface::Y(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Z(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CY(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CZ(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RT(real1, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RTDyad(int, int, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRT(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRTDyad(int, int, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RX(real1, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RXDyad(int, int, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRX(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRXDyad(int, int, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RY(real1, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RYDyad(int, int, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRY(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRYDyad(int, int, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RZ(real1, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RZDyad(int, int, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRZ(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRZDyad(int, int, bitLenInt, bitLenInt)

.. doxygenfunction:: Qrack::QInterface::Exp(real1, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpDyad(int, int, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpX(real1, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpXDyad(int, int, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpY(real1, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpYDyad(int, int, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpZ(real1, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpZDyad(int, int, bitLenInt)

Register-wide Gates
~~~~~~~~~~~~~~~~~~~

.. doxygenfunction:: Qrack::QInterface::AND(bitLenInt, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CLAND(bitLenInt, bitCapInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::OR(bitLenInt, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CLOR(bitLenInt, bitCapInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::XOR(bitLenInt, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CLXOR(bitLenInt, bitCapInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CCNOT(bitLenInt, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::AntiCCNOT(bitLenInt, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CNOT(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CNOT(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::AntiCNOT(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::H(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::MReg(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::X(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Y(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Z(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CY(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CZ(bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RT(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RTDyad(int, int, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RX(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RXDyad(int, int, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRX(real1, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRXDyad(int, int, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RY(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RYDyad(int, int, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRY(real1, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRYDyad(int, int, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RZ(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::RZDyad(int, int, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRZ(real1, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::CRZDyad(int, int, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::Exp(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpDyad(int, int, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpX(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpXDyad(int, int, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpY(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpYDyad(int, int, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpZ(real1, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::ExpZDyad(int, int, bitLenInt, bitLenInt)

Algorithmic Implementations
---------------------------

.. doxygenfunction:: Qrack::QInterface::QFT
.. doxygenfunction:: Qrack::QInterface::IndexedLDA
.. doxygenfunction:: Qrack::QInterface::IndexedADC
.. doxygenfunction:: Qrack::QInterface::IndexedSBC
