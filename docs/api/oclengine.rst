:orphan:

.. Copyright (c) 2017-2020

OCLEngine
========================

Defined in `common/oclengine.hpp <https://github.com/vm6502q/qrack/blob/master/include/common/oclengine.hpp>`_.

This provides a basic interface with a wide-ranging set of functionality.

.. doxygenclass:: Qrack::OCLEngine
    :project: qrack

Creating an OCLEngine
-----------------------

OCLEngine is a singleton class that manages all OpenCL devices and supported objects, for use in ``QEngineOCL`` and ``QEngineOCLMulti``.

.. doxygenfunction:: Qrack::OCLEngine::Instance()


Configuration Methods
---------------------------------

.. doxygenfunction:: Qrack::OCLEngine::GetDeviceContextPtr(const int64_t&)
.. doxygenfunction:: Qrack::OCLEngine::GetDeviceContextPtrVector()
.. doxygenfunction:: Qrack::OCLEngine::SetDeviceContextPtrVector(std::vector<DeviceContextPtr>, DeviceContextPtr)
.. doxygenfunction:: Qrack::OCLEngine::GetDeviceCount()
.. doxygenfunction:: Qrack::OCLEngine::SetDefaultDeviceContext(DeviceContextPtr);
