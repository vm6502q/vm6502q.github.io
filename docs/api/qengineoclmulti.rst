:orphan:

.. Copyright (c) 2018

QEngineOCLMulti
========================

Defined in `qengine_opencl_multi.hpp <https://github.com/vm6502q/qrack/blob/master/include/qengine_opencl_multi.hpp>`_.

The API is provided by Qrack::QInterface. However, QEngineOCLMulti has two custom constructors:

.. doxygenfunction:: Qrack::QEngineOCLMulti::QEngineOCLMulti(bitLenInt, bitCapInt, std::shared_ptr<std::default_random_engine>, int)

.. doxygenfunction:: Qrack::QEngineOCLMulti::QEngineOCLMulti(bitLenInt, bitCapInt, std::vector<int>, std::shared_ptr<std::default_random_engine>)
