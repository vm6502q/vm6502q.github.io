:orphan:

.. Copyright (c) 2017-2021

QEngine
========================

Defined in `qengine.hpp <https://github.com/vm6502q/qrack/blob/master/include/qengine.hpp>`_.

This is an (abstract) intermediate specialization that inherits from Qrack::QInterface. This type is specifically a "state vector" simulation, with corresponding special methods.

.. doxygenfunction:: Qrack::QEngine::QEngine(bitLenInt, qrack_rand_gen_ptr, bool, bool, bool, bool, real1_f)

.. doxygenfunction:: Qrack::QEngine::QEngine()

.. doxygenfunction:: Qrack::QEngine::ZeroAmplitudes

.. doxygenfunction:: Qrack::QEngine::CopyStateVec

.. doxygenfunction:: Qrack::QEngine::IsZeroAmplitude

.. doxygenfunction:: Qrack::QEngine::GetAmplitudePage

.. doxygenfunction:: Qrack::QEngine::SetAmplitudePage(const complex *, const bitCapIntOcl, const bitCapIntOcl)

.. doxygenfunction:: Qrack::QEngine::SetAmplitudePage(QEnginePtr, const bitCapIntOcl, const bitCapIntOcl, const bitCapIntOcl)

.. doxygenfunction:: Qrack::QEngine::ShuffleBuffers

.. doxygenfunction:: Qrack::QEngine::QueueSetDoNormalize

.. doxygenfunction:: Qrack::QEngine::QueueSetRunningNorm
