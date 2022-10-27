:orphan:

.. Copyright (c) 2017-2021

QBinaryDecisionTree
========================

Defined in `qbinary_decision_tree.hpp <https://github.com/vm6502q/qrack/blob/main/include/qbinary_decision_tree.hpp>`_ and `qbinary_decision_node.hpp <https://github.com/vm6502q/qrack/blob/main/include/qbinary_decision_node.hpp>`_.

The API is provided by Qrack::QInterface. Qrack::QBinaryDecisionTree is an attempt at an alternative representation of quantum pure states, in terms of binary decision trees, inspired by `other open source work <https://iic.jku.at/eda/research/quantum_dd/>`_.

.. doxygenfunction:: Qrack::QBinaryDecisionTree::QBinaryDecisionTree(bitLenInt, bitCapInt, qrack_rand_gen_ptr, complex, bool, bool, bool, int64_t, bool, bool, real1_f, std::vector<int64_t>, bitLenInt, real1_f)
.. doxygenfunction:: Qrack::QBinaryDecisionTree::QBinaryDecisionTree(std::vector<QInterfaceEngine>, bitLenInt, bitCapInt, qrack_rand_gen_ptr, complex, bool, bool, bool, int64_t, bool, bool, real1_f, std::vector<int64_t>, bitLenInt, real1_f)
