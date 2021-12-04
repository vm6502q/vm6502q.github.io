:orphan:

.. Copyright (c) 2017-2021

QUnitMulti
========================

Defined in `qunitmulti.hpp <https://github.com/vm6502q/qrack/blob/master/include/qunitmulti.hpp>`_.

Qrack::QUnitMulti is a direct subclass of Qrack::QUnit that attempts to dispatch its separable internal subsystem work to multiple OpenCL devices, including all devices visible to the OpenCL context by default. (Qrack::QUnit maintains explicit separation of representation between separable subsystems, when possible and efficient, greatly reducing memory and execution time overhead.)
