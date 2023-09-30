:orphan:

.. Copyright (c) 2017-2023

QCircuit
========================

Defined in `QCircuit.hpp <https://github.com/vm6502q/qrack/blob/main/include/qcircuit.hpp>`_.

This is a class that represents and optimizes a simple quantum "circuit," (without directly simulating it). Its constructor argument turns on-the-fly circuit simplification on or off.

Via `QCircuitGate`, the fundamental "atomic" gate of a `QCircuit` is "uniformly controlled." (See `Quantum circuits with uniformly controlled one-qubit gates <https://arxiv.org/abs/quant-ph/0410066>`_.) In other words, a different "target qubit payload" (as a 2x2 operator on a single qubit, controlled) is associated with every logical permutation of control qubits, which can greatly optimize performance through coalescence in an efficient form for state vector simulation.

.. doxygenfunction:: Qrack::QCircuit::QCircuit(bool)
.. doxygenfunction:: Qrack::QCircuit::QCircuit(bitLenInt, const std::list<QCircuitGatePtr>&, bool)
.. doxygenfunction:: Qrack::QCircuit::Clone()
.. doxygenfunction:: Qrack::QCircuit::Inverse()
.. doxygenfunction:: Qrack::QCircuit::Append(QCircuitPtr)
.. doxygenfunction:: Qrack::QCircuit::Combine(QCircuitPtr)
.. doxygenfunction:: Qrack::QCircuit::AppendGate(QCircuitGatePtr)
.. doxygenfunction:: Qrack::QCircuit::Run(QInterfacePtr)

.. doxygenfunction:: Qrack::QCircuit::GetQubitCount()
.. doxygenfunction:: Qrack::QCircuit::SetQubitCount(bitLenInt)
.. doxygenfunction:: Qrack::QCircuit::GetGateList()
.. doxygenfunction:: Qrack::QCircuit::SetGateList(std::list<QCircuitGatePtr>)

.. doxygenfunction:: Qrack::QCircuit::Swap(bitLenInt, bitLenInt)

.. doxygenfunction:: Qrack::QCircuit::IsNonPhaseTarget(bitLenInt)
.. doxygenfunction:: Qrack::QCircuit::DeletePhaseTarget(bitLenInt, bool)
.. doxygenfunction:: Qrack::QCircuit::PastLightCone(std::set<bitLenInt>&)

QCircuitGate
========================

.. doxygenfunction:: Qrack::QCircuitGate::QCircuitGate()
.. doxygenfunction:: Qrack::QCircuitGate::QCircuitGate(bitLenInt, bitLenInt)
.. doxygenfunction:: Qrack::QCircuitGate::QCircuitGate(bitLenInt, const complex[])
.. doxygenfunction:: Qrack::QCircuitGate::QCircuitGate(bitLenInt, const complex[], const std::set<bitLenInt>&, bitCapInt)
.. doxygenfunction:: Qrack::QCircuitGate::QCircuitGate(bitLenInt, const std::map<bitCapInt, std::shared_ptr<complex>>&, const std::set<bitLenInt>&)
