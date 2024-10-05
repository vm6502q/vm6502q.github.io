:orphan:

.. Copyright (c) 2017-2022

QAlu
========================

Defined in `qalu.hpp <https://github.com/vm6502q/qrack/blob/master/include/qalu.hpp>`_.

This is an interface layer, separate from and in addition to `Qrack::QInterface`, meant to isolate less-used functionality as optional. Specifically, it provides all emulated arithmetic logic unit (ALU) functionality that is not yet implemented at gate level in `Qrack::QInterface`.

.. doxygenclass:: Qrack::QAlu
    :project: qrack


Arithmetic
--------------------------

.. doxygenfunction:: Qrack::QAlu::INCSC(const bitCapInt&, bitLenInt, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QAlu::INCSC(const bitCapInt&, bitLenInt, bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QAlu::MUL
.. doxygenfunction:: Qrack::QAlu::DIV
.. doxygenfunction:: Qrack::QAlu::CMUL
.. doxygenfunction:: Qrack::QAlu::CDIV
.. doxygenfunction:: Qrack::QAlu::POWModNOut
.. doxygenfunction:: Qrack::QAlu::CPOWModNOut
.. doxygenfunction:: Qrack::QAlu::IndexedLDA
.. doxygenfunction:: Qrack::QAlu::IndexedADC
.. doxygenfunction:: Qrack::QAlu::IndexedSBC
.. doxygenfunction:: Qrack::QAlu::Hash
