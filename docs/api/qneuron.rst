:orphan:

.. Copyright (c) 2017-2026

QNeuron
========================

Defined in `qneuron.hpp <https://github.com/unitaryfoundation/qrack/blob/main/include/qneuron.hpp>`_.

.. doxygenclass:: Qrack::QNeuron
    :project: qrack

Activation Functions
--------------------

.. doxygenenum:: QNeuronActivationFn


Constructors
------------

.. doxygenfunction:: Qrack::QNeuron::QNeuron(QInterfacePtr, const std::vector<bitLenInt>&, bitLenInt, QNeuronActivationFn, real1_f, real1_f)
.. doxygenfunction:: Qrack::QNeuron::QNeuron(const QNeuron&)

Getters and Setters
-------------------

.. doxygenfunction:: Qrack::QNeuron::SetSimulator
.. doxygenfunction:: Qrack::QNeuron::GetSimulator
.. doxygenfunction:: Qrack::QNeuron::SetAlpha
.. doxygenfunction:: Qrack::QNeuron::SetActivationFn
.. doxygenfunction:: Qrack::QNeuron::GetActivationFn
.. doxygenfunction:: Qrack::QNeuron::SetAngles
.. doxygenfunction:: Qrack::QNeuron::GetAngles
.. doxygenfunction:: Qrack::QNeuron::GetInputCount
.. doxygenfunction:: Qrack::QNeuron::GetInputPower

Learning and Prediction
-----------------------

.. doxygenfunction:: Qrack::QNeuron::Predict
.. doxygenfunction:: Qrack::QNeuron::Unpredict
.. doxygenfunction:: Qrack::QNeuron::LearnCycle
.. doxygenfunction:: Qrack::QNeuron::Learn
.. doxygenfunction:: Qrack::QNeuron::LearnPermutation
