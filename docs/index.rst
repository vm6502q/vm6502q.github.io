VM6502Q and Qrack
=================

Introduction
------------

`Qrack <https://github.com/vm6502q/qrack>`_ is a C++ quantum bit and gate simulator, with the ability to support arbitrary numbers of entangled qubits - up to system limitations.  Suitable for embedding in other projects, the :cpp:class:`Qrack::QInterface` contains a full and performant collection of standard quantum gates, as well as variations suitable for register operations and arbitrary rotations.

The developers of Qrack maintain a `fork <https://github.com/vm6502q/ProjectQ>`_ of the `ProjectQ <https://github.com/ProjectQ-Framework/ProjectQ>`_ quantum computer compiler which can use Qrack as the simulator, generally. This stack is also compatible with the `SimulaQron <https://github.com/SoftwareQuTech/SimulaQron>`_ quantum network simulator. Further, we maintain a `QrackProvider <https://github.com/vm6502q/qiskit-qrack-provider>`_ for `Qiskit <https://qiskit.org/>`_. Both ProjectQ and Qiskit integrations for Qrack support the `PennyLane <https://pennylane.ai/>`_ stack. (For Qiskit, a `fork <https://github.com/vm6502q/pennylane-qiskit>`_ of the Qiskit plugin provides support for a "QrackDevice".) Qrack's developers are not directly affiliated with any of these projects, but we thank them for their contribution to the open source quantum computing community!

As a demonstration of the :cpp:class:`Qrack::QInterface` implementation, a MOS-6502 microprocessor [MOS-6502]_ virtual machine has been modified with a set of new opcodes (:ref:`mos-6502q-opcodes`) supporting quantum operations.  The `vm6502q <https://github.com/vm6502q/vm6502q>`_ virtual machine exposes new integrated quantum opcodes such as Hadamard transforms and an X-indexed LDA, with the X register in superposition, across a page of memory.  An assembly example of a Grover's search with a simple oracle function is demonstrated in the `examples <https://github.com/vm6502q/examples>`_ repository.

Copyright
---------

Copyright (c) Daniel Strano 2017-2020 and the Qrack contributors. All rights reserved.

Daniel Strano would like to specifically note that Benn Bollay is almost entirely responsible for the original implementation of QUnit and tooling, including unit tests, in addition to large amounts of work on the documentation and many other various contributions in intensive reviews. Also, thank you to Marek Karcz for supplying an awesome base classical 6502 emulator for proof-of-concept.

.. toctree::
    :maxdepth: 2
    :hidden:

    Introduction <self>
    start
    theory
    opencl
    examples
    implementation
    performance

.. toctree::
    :hidden:
    :caption: API
    :maxdepth: 2

    api/qinterface
    api/oclengine
    api/qengine
    api/qenginecpu
    api/qengineocl
    api/qhybrid
    api/qstabilizerhybrid
    api/qunit
    api/qbinarydecisiontree
    api/6502

.. The #http:// is a hack to get around Sphinx's re parser for links,
   see https://stackoverflow.com/a/31820846 for more details.

.. Additionally, the links are hard-coded to /en/latest because there's
   no known mechanism for linking from the top of the deployed directory,
   as Sphinx doesn't know about that when rendering the html.  Not ideal,
   but it will suffice until a more sophisticated user story is identified.
  
.. toctree::
    :hidden:
    :caption: Doxygen
    :maxdepth: 2

    QInterface </en/latest/_static/doxygen/classQrack_1_1QInterface.html#http://>
    QEngine </en/latest/_static/doxygen/classQrack_1_1QEngine.html#http://>
    QEngineCPU </en/latest/_static/doxygen/classQrack_1_1QEngineCPU.html#http://>
    QEngineOCL </en/latest/_static/doxygen/classQrack_1_1QEngineOCL.html#http://>
    QHybrid </en/latest/_static/doxygen/classQrack_1_1QHybrid.html#http://>
    QStabilizerHybrid </en/latest/_static/doxygen/classQrack_1_1QStabilizerHybrid.html#http://>
    QUnit </en/latest/_static/doxygen/classQrack_1_1QUnit.html#http://>
    QBinaryDecisionTree </en/latest/_static/doxygen/classQrack_1_1QBinaryDecisionTree.html#http://>

