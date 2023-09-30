:orphan:

.. Copyright (c) 2017-2023

QTensorNetwork
========================

Defined in `QTensorNetwork.hpp <https://github.com/vm6502q/qrack/blob/main/include/qtensornetwork.hpp>`_.

The API is provided by Qrack::QInterface. (It introduces no new constructor arguments.) Qrack::QTensorNetwork attempts "local simplification" of input gates by collecting them in a series of `QCircuit` and measurement result layers. It can calculate on the "past light cone" of specific qubits to measure, controlled by the environment variable `QRACK_QTENSORNETWORK_THRESHOLD_QB`. (Any simulation with qubit width higher than the threshold integer will calculate on the past light cone.)
