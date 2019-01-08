:orphan:

.. Copyright (c) 2018

QUnitMulti
========================

Defined in `qunitmulti.hpp <https://github.com/vm6502q/qrack/blob/master/include/qunitmulti.hpp>`_.

Qrack::QUnitMulti is an experimental QUnit type, which distributes separable subsystems to multiple OpenCL devices, if multiple devices are available. It does not yet give a practical speed return, for the use of multiple devices, and it might be unstable. It is included to help development of a practical multiprocessor type. (Past multiprocessor types have included QEngineOCLMulti, effectively inheriting from the QEngine type instead of QUnit. QEngineOCLMulti is not currently supported.)

The API is provided by Qrack::QInterface.
