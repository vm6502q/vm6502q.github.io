:orphan:

.. Copyright (c) 2017-2021

QUnit
========================

Defined in `qunit.hpp <https://github.com/vm6502q/qrack/blob/master/include/qunit.hpp>`_.

Qrack::QUnit maintains explicit separation of representation between separable subsystems, when possible and efficient, greatly reducing memory and execution time overhead.

Qrack::QInterface::TrySeparate() is primarily intended for use with Qrack::QUnit.

.. doxygenfunction:: Qrack::QInterface::TrySeparate(bitLenInt)
.. doxygenfunction:: Qrack::QInterface::TrySeparate(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QInterface::TrySeparate(const std::vector<bitLenInt>&, real1_f)
