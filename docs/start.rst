Getting Started
---------------

Prerequisites
~~~~~~~~~~~~~

Qrack compiles with a C++11 compiler, such as g++ or clang++, with any required compilation flags to enable the C++11 standard.

You also need CMake to build. `CMake installation instructions can be found here. <https://cmake.org/install/>`_

Optional GPU support is provided by OpenCL development libraries. See :ref:`installing-opencl-reference` for further instructions.

Checking Out
~~~~~~~~~~~~

Check out each of the major repositories into a project branch:

.. code-block:: bash

          / $ mkdir qc
          / $ cd qc
        qc/ $ git clone https://github.com/vm6502q/qrack.git
        qc/ $ git clone https://github.com/vm6502q/vm6502q.git
        qc/ $ git clone https://github.com/vm6502q/examples.git
            # Note: the cc65 repository changes live in the 6502q branch
        qc/ $ git clone https://github.com/vm6502q/cc65.git -b 6502q

            # Add a necessary symlink connecting the vm6502q project with qrack
        qc/ $ cd vm6502q && ln -s ../qrack

            # vm6502q expects the qrack buildfiles to exist in qrack/build
        qc/ $ mkdir qrack/build
        qc/ $ cd qrack/build && cmake ..
            # OR if no OpenCL support is enabled
        qc/ $ cd qrack/build && cmake -DUSE_OPENCL=OFF ..

Compiling
~~~~~~~~~

The ``qrack`` project supports two primary implementations: OpenCL-optimized and software-only.  See :doc:`opencl` for details on installing OpenCL on some platforms, or your appropriate OS documentation. If you do not have OpenCL or do not wish to use it, supply the ``USE_OPENCL=OFF`` environment to ``cmake`` when building qrack the first time.

Qrack compiles with either double or single accuracy complex numbers. Doubles are used by default. Single float accuracy uses almost exactly half as much RAM, allowing one additional qubit. Single accuracy may also be faster or the only compatible option for certain OpenCL devices, such as accelerators. Double vectorization uses AVX, while single vectorization uses SSE 1.0.

To enable float accuracy as opposed to double, run CMake with the appropriate flag:

.. code-block:: bash

    qc/ $ cd qrack/build && cmake -DENABLE_COMPLEX8=ON ..

Using the API
~~~~~~~~~~~~~

Qrack API methods operate on "QEngine" and "QUnit" objects. ("QUnit" objects are a specific optional optimization on "QEngine" objects, with the same API interface.) These objects are organized as 1-dimensional arrays of coherent qubits which can be arbitrarily entangled within the QEngine or QUnit. These object have methods that act like quantum gates, for a specified qubit index in the 1-dimensional array, as well as any analog parameters needed for the gate (like for variable angle rotation gates). Many fundamental gate methods have variants that are optimized to act on a contiguous length of qubits in the array at once. For OpenCL ``QEngineOCL`` objects, the preferred OpenCL device can be specified in the constructor. For multiprocessor ``QEngineOCLMulti`` engines, you can specify distribution of equal-sized sub-engines between available OpenCL devices. See the API reference for more details.

To create a QEngine or QUnit object, you can use the factory provided in include/qfactory.hpp:

.. code-block:: c

    QInterfacePtr qftReg = CreateQuantumInterface(QINTERFACE_QUNIT, QINTERFACE_OPENCL, qubitCount, intPerm, rng);
    QInterfacePtr qftReg2 = CreateQuantumInterface(QINTERFACE_OPENCL, QINTERFACE_OPENCL, qubitCount, intPerm, rng);

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

    // "deviceIDs" is a std::vector<int> of indices of the desired device in the OCLEngine list.
    // This also might be used for a simple form of load-balancing, like so:
    std::vector<int> deviceIDs(4);
    // Three equally-sized sub-engines go to device index 1, and a fourth sub-engine goes to device index 0.
    // The number of sub-engines must be a power of two.
    deviceIDs[0] = 1;
    deviceIDs[0] = 1;
    deviceIDs[0] = 1;
    deviceIDs[0] = 0;
    QEngineOCL qEngineMulti = QEngineOCLMulti(qBitCount, initPermutation, deviceIDs, random_generator_pointer);

Testing
~~~~~~~

The qrack project has an extensive set of unittests for the various :cpp:class:`Qrack::QInterface` gates and simulator methods.  This can be executed through running the test suite in the ``qrack`` project:

.. code-block:: bash

     qrack/build/ $ make test

This may take a few minutes to complete, depending on the strength of the system executing the tests.

.. note::

    The unittests, by default, run against all supported engines.  If only a specific engine type is desired, the ``--disable-opencl`` or ``--disable-software`` command line parameters may be supplied to the ``unittest`` binary.


Embedding Qrack
~~~~~~~~~~~~~~~

The ``qrack`` project produces a ``libqrack.a`` archive, suitable for being linked into a larger binary.  See the :cpp:class:`Qrack::QInterface` documentation for API references, as well as the examples present in `the unit tests <https://github.com/vm6502q/qrack/blob/master/tests.cpp>`_.

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

