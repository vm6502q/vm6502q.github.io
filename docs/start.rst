Getting Started
---------------

Accelerate Qiskit workloads in 3 steps! (PyQrack)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We encourage all users to build the Qrack framework from source as primary installation method, for maximum configurability, system compatibility, and application suitability. However, you don't have to!

Many users can benefit from an installation method designed to replace a Qiskit simulator with a Qrack simulator one-for-one, without little or no Qiskit user code modification. We provide such an installation method, typically in 3 simple steps. We assume that you've already installed a Qiskit environment and have a script that you'd like to try running with Qrack instead of Aer, with little or no modification.

1. Install the qiskit-qrack-provider Python package, which will also install PyQrack as a dependency:
.. code-block:: bash

        pip install qiskit-qrack-provider`

2. In the Qiskit script you'd like to run, replace all instances of "Aer" and "AerProvider" with "Qrack" and "QrackProvider" from a subset of classes among the following import statements:
.. code-block:: python

        from qiskit.providers.qrack import Qrack, QrackProvider
        from qiskit.providers.qrack.backends import QasmSimulator

3. If you must, adapt your script to use the "qasm_simulator" backend API. (You might not need to modify anything at all, for this step!) Qrack currently only provides a "qasm_simulator" backend API, because Qrack's novel simulation method optimizations don't necessarily internally track a traditional state vector representation at all, except when expedient. Hence, Qrack benefits from an API formulated in terms of gate primitives and measurement distributions that resembling a native quantum hardware interface.

That's it! Enjoy GPU-acceleration and Qrack's "transparent" novel optimization layers! (For example, try writing Clifford group stabilizer programs to run with the default simulator backend configuration, and watch Qrack automatically detect that the program should execute as stabilizer simulation!)

The rest of the document explains the "advanced" (but honestly also not difficult) C++ build requirements and process.

Prerequisites
~~~~~~~~~~~~~

Qrack compiles with a C++11 compiler, such as g++ or clang++, with any required compilation flags to enable the C++11 standard.

You also need CMake to build. `CMake installation instructions can be found here. <https://cmake.org/install/>`_

Optional GPU support is provided by OpenCL development libraries. See :ref:`installing-opencl-reference` for further instructions.

Checking Out
~~~~~~~~~~~~

Clone the repository with git:

.. code-block:: bash

          / $ mkdir qc
          / $ cd qc
        qc/ $ git clone https://github.com/vm6502q/qrack.git

Compiling
~~~~~~~~~

The ``qrack`` project supports two primary implementations: OpenCL-optimized and software-only.  See :doc:`opencl` for details on installing OpenCL on some platforms, or your appropriate OS documentation. If you do not have OpenCL or do not wish to use it, supply the ``-DENABLE_OPENCL=OFF`` build option to ``cmake`` when building qrack the first time.

.. code-block:: bash
    qc/ $ mkdir qrack/build
    qc/ $ cd qrack/build && cmake [-DENABLE_OPENCL=OFF] [-DENABLE_COMPLEX_X2=OFF] [-DENABLE_RDRAND=ON] [-DQBCAPPOW=5-31] [-DFPPOW=4-6] ..

Then ``make all`` to compile (with ``-j8`` for 8 parallel build cores, or as appropriate) or (``sudo``) ``make install`` to compile and install into a shared location in your system. ``make install`` would be needed if you're going to use this version of the qrack library with `pyqrack <https://github.com/vm6502q/pyqrack>`_.

Qrack compiles with either double (``FPPOW=6``) or single (``FPPOW=5``) accuracy complex numbers, for most OpenCL-enabled builds. 16 bit half accurracy (``FPPOW=4``) is available for CPU-only builds and for a limited selection of NVIDIA GPUs. Single float accuracy is used by default. Single float accuracy uses almost exactly half as much RAM as double accuracy, allowing one additional qubit. Single accuracy may also be faster or the only compatible option for certain OpenCL devices.

.. note:: If your GPU or accelerator supports 16-bit floating point, but 16-bit OpenCL kernels do not compile or run, then a Qrack build for your device likely needs simply a different ``#pragma`` line, which is a tiny change to the OpenCL code. We suggest you open an issue on the Qrack GitHub repository to direct the developers' attention to the likely very small change that will support your device, and permanently fix the issue for other users. However, it's difficult to anticipate these cases, particularly without your help to actually test the modified build for a device to which the developers don't have access.

Vectorization (``ENABLE_COMPLEX_X2=ON``) of doubles uses AVX, while single accuracy vectorization uses SSE 1.0. Turning vectorization off at compile time removes all SIMD vectorization.

``QBCAPPOW`` takes an integer "n" between 5 and 31, such that maximum addressable qubits in a QInterface instance is 2^n. n=5 would be 32 qubits per QInterface instance, n=6 is the defaullt at 64 qubits per, n=7 addresses up to 128 qubits per, and so on up to n=31. "Addressable" qubits does not mean that the qubits can necessarily by allocated on the particular system. However, ``QUnit`` Schmidt decomposition optimizations and/or sparse state vector optimizations do render certain very high-qubit-width circuits tractable, when they stay well below the limit of total arbitrary entanglement. (Reducing representational entanglement happens almost entirely "under-the-hood," in ``QUnit``.)

Many OpenCL devices that don't support double accuracy floating point operations still support 64-bit integer types. If a device doesn't support 64-bit integer types, ``UINTPOW=5`` with ``ENABLE_COMPLEX_X2=OFF`` will disable all 64-bit types in OpenCL kernels, as well as SIMD. This theoretically supports the OpenCL standard on a device such as a Raspberry Pi 3.

Running Tests
~~~~~~~~~~~~~

To run unit tests, run the following in the build directory:

.. code-block:: bash

    ./unittest [-?] [--layer-...] [--proc-...] [specific_test_name]

See the ``-?`` help instructions for option details, and Qrack "layer" and "processor type" choices.

The benchmarks respect the same parameters:

.. code-block:: bash

    ./benchmarks [-?] [--max-qubits=-1] [--layer-...] [--proc-...] [specific_test_name]

``--max-qubits`` will automatically size with ``-1`` as given argument, or otherwise up to the number of qubits specified for this parameter.


Using the API
~~~~~~~~~~~~~

Qrack API methods operate on "QEngine" and "QUnit" objects. ("QUnit" objects are a specific optional optimization on "QEngine" objects, with the same API interface.) These objects are organized as 1-dimensional arrays of coherent qubits which can be arbitrarily entangled within the QEngine or QUnit. These object have methods that act like quantum gates, for a specified qubit index in the 1-dimensional array, as well as any analog parameters needed for the gate (like for variable angle rotation gates). Many fundamental gate methods have variants that are optimized to act on a contiguous length of qubits in the array at once. For OpenCL ``QEngineOCL`` objects, the preferred OpenCL device can be specified in the constructor. For multiprocessor ``QEngineOCLMulti`` engines, you can specify distribution of equal-sized sub-engines between available OpenCL devices. See the API reference for more details.

To create a QEngine or QUnit object, you can use the factory provided in include/qfactory.hpp. The easiest way to choose an optimal "layer stack" is to use ``QINTERFACE_OPTIMAL`` for a single OpenCL device simulator, and use ``QINTERFACE_OPTIMAL_MULTI`` for a multi-device simulator:

.. code-block:: c

    QInterfacePtr qftReg = CreateQuantumInterface(QINTERFACE_OPTIMAL, qubitCount, intPerm, rng);
    QInterfacePtr qftReg2 = CreateQuantumInterface(QINTERFACE_OPTIMAL_MULTI, qubitCount, intPerm, rng);

By default, the ``Qrack::OCLEngine`` singleton attempts to compile kernels and initialize supporting OpenCL objects for all devices on a system. You can strike devices from the list to free their OpenCL resources, usually before initializing OpenCL QEngine objects:

.. code-block:: c

    // Initialize the singleton and get the list of devices
    std::vector<Qrack::OCLDeviceContext> devices = OCLEngine::Instance()->GetDeviceContextPtrVector();
    std::vector<Qrack::OCLDeviceContext> filteredDevices;

    // Iterate through the list with cl::Device::getInfo to check devices for desirability
    std::string devCheck("HD");
    for (int i = 0; i < devices.size(); i++) {
        // From the OpenCL C++ API headers:
        string devName = std::string(devices[i].getInfo<CL_DEVICE_NAME>());
        // Check properties...
        if (devName.find(devCheck) != string::npos) {
            // Take or remove devices selectively
            filteredDevices.push_back(devices[i]);
        }
    }

    // Replace the original list with the filtered one, and (with an optional argument) specify the default device.
    OCLEngine::Instance()->SetDeviceContextPtrVector(filteredDevices, filteredDevices[0]);

With or without this kind of filtering, the device or devices used by OpenCL-based engines can be specified explicitly in their constructors:

.. code-block:: c
    
    // "deviceID" is the (int) index of the desired device in the OCLEngine list:
    int deviceID = 0;
    QEngineOCL qEngine = QEngineOCL(qBitCount, initPermutation, random_generator_pointer, deviceID);

Optimal CreateQuantumInterface Factory Options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Qrack's most specifically optimized "layer" stack is also its best general use case simulator, (at this time):

.. code-block:: c

    QInterfacePtr qftReg = CreateQuantumInterface(QINTERFACE_QUNIT, QINTERFACE_STABILIZER_HYBRID, QINTERFACE_QPAGER, QINTERFACE_MASK_FUSION, QINTERFACE_HYBRID, qubitCount, intPerm, rng[, ...]);

``QUnit`` is Qrack's "novel optimization layer." ``QStabilizerHybrid`` is a "QUnit shard" that combines Gottesman-Knill stabilizer simulation with Dirac "ket" simulation. The "ket" simulation further "hybridizes" between asynchronous GPU and CPU workloads as is efficient for workloads. When ``QUnit`` can determine that levels of entanglement are low, it will maintain Schmidt decomposed representations of subunit (or sub-register) state, in an attempt to increase efficiency.

Embedding Qrack
~~~~~~~~~~~~~~~

For static linkage, the build process produces a ``libqrack.a`` archive, suitable for being linked into a larger binary.  See the :cpp:class:`Qrack::QInterface` documentation for API references, as well as the examples present in `the unit tests <https://github.com/vm6502q/qrack/blob/main/tests.cpp>`_.

For dynamic linkage, use ``libqrack_pinvoke.so``. (This is the shared object library upon which such wrapper projects as `PyQrack <https://github.com/vm6502q/pyqrack>`_ are based and linked from.)

Performance
~~~~~~~~~~~

See the extensive :doc:`performance analysis and graphs <performance>` section.

Contributing
~~~~~~~~~~~~

Pull requests and issues are happily welcome!

Please make sure ``make format`` (depends on `clang-format-5 <https://clang.llvm.org/docs/ClangFormat.html>`_) has been executed against any PRs before being published.

Community
~~~~~~~~~

Qrack and VM6502Q have a development community on the `Advanced Computing Topics <https://discord.gg/yDZBuhu>`_ discord server on channel #qrack.  Come join us!

