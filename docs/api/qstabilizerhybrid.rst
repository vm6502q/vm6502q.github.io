:orphan:

.. Copyright (c) 2017-2021

QStabilizerHybrid
========================

Defined in `qstabilizerhybrid.hpp <https://github.com/vm6502q/qrack/blob/main/include/qstabilizerhybrid.hpp>`_.

The API is provided by Qrack::QInterface. This is an extended "stabilizer" or "Clifford" simulation. If a Qrack::QInterface method call cannot be carried out by this simulator via "stabilizer" simulation methods, it will automatically convert to traditional state vector representation, (while keep its Qrack::QStabilizerHybrid class type).

.. doxygenfunction:: Qrack::QStabilizerHybrid::QStabilizerHybrid(bitLenInt, bitCapInt, qrack_rand_gen_ptr, complex, bool, bool, bool, int, bool, bool, real1, std::vector<int>, bitLenInt)
