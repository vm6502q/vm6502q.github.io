:orphan:

.. Copyright (c) 2018

QUnit
========================

Defined in `qunit.hpp <https://github.com/vm6502q/qrack/blob/master/include/qunit.hpp>`_.

Qrack::QUnit maintains explicit separation of representation between separable subsystems, when possible and efficient, greatly reducing memory and execution time overhead.

The API is provided by Qrack::QInterface. However, QUnitMulti has a custom constructor:

.. doxygenfunction:: Qrack::QUnit::QUnit(QInterfaceEngine, QInterfaceEngine, bitLenInt, bitCapInt, std::shared_ptr<std::default_random_engine>, complex, bool, bool, bool)

Qrack::QInterface::TrySeparate() is primarily intended for use with Qrack::QUnit.

.. doxygenfunction:: Qrack::QInterface::TrySeparate(bitLenInt, bitLenInt)
