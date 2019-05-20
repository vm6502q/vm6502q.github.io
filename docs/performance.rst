#################
Qrack Performance
#################

Abstract
********

The Qrack quantum simulator is an open-source C++ high performance, general
purpose simulation supporting arbitrary numbers of entangled qubits.  While
there are a variety of other quantum simulators such as [QSharp]_, [QHiPSTER]_,
and others listed on [Quantiki]_, Qrack represents a unique offering suitable
for applications across the field.

A selection of performance tests are identified for creating comparisons
between various quantum simulators.  These metrics are implemented and
analyzed for Qrack.  These experimentally derived results compare favorably
against theoretical boundaries, and out-perform naive implementations for many
scenarios.

Introduction
************

There are a growing number of quantum simulators available for research and
industry use.  Many of them perform quite well for smaller number of qubits,
and are suitable for non-rigorous experimental explorations.  Fewer projects
are suitable for the growing mid-tier range of experimentation in the 20-30
qubit range.

Despite the availability of a selection of implementations, very little has
been established when comparing the performance between different simulators.
Broadly, the substantial bottlenecks around memory and IO utilization have
largely preempted analysis into CPU efficiencies and algorithmic
optimizations.  There are some exceptions, such as IBM's `Breaking the
49-Qubit Barrier in the Simulation of Quantum Circuits` [Pednault2017]_ paper.

Qrack provides high performance up to at least the 30 qubit range, particularly as an
open-source option, implemented in C++, suitable for utilization in a wide variety
of projects.  As such, it is an ideal test-bed for establishing a set of
benchmarks useful for comparing performance between various quantum
simulators.

Qrack provides a "QEngineCPU" and a "QEngineOCL" that represent non-OpenCL and 
OpenCL base implementations, which could also be the most efficient types to 
use directly in the limit of maximal entanglement. For general use cases, 
the "QUnit" layer provides explicit Schmidt decomposition on top of another 
engine type, (per [Pednault2017]_,) and "QFusion" provides a "gate fusion" 
layer. A "QEngine" type is always the base layer, and QUnit and QFusion types 
may be layered over these, and over each other.

This version of the Qrack benchmarks contains comparisons against other
publicly available simulators, specifically QCGPU and ProjectQ (with its
default simulator). Note that Qrack has been incorporated as an optional
back end for ProjectQ, in a fork maintained by the developers of Qrack, and
benchmarks for its performance will follow.

Reader Guidance
===============

This document is largely targeted towards readers looking for a quantum
simulator that desire to establish the expected bounds for various use-cases
prior to implementation.

Disclaimers
===========

* Your Mileage May Vary - Any performance metrics here are the result of
  experiments executed with selected compilation and execution parameters on a
  system with a degree of variability; execute the supplied benchmarks on the
  desired target system for accurate performance assessments.

* Benchmarking is Hard - While we've attempted to perform clean and accurate
  results, bugs and mistakes do occur.  If flaws in process are identified,
  please let us know!

Method
******

100 timed trials of single and parallel gates were run for each qubit count between 4 and 28 qubits. The benchmarking code is available at `https://github.com/vm6502q/qrack/blob/master/test/benchmarks.cpp <https://github.com/vm6502q/qrack/blob/master/test/benchmarks.cpp>`_, in which qubit count can be set and tests can be commented out to reduce to the relevant subset.

CPU and GPU benchmarks were run on two respective systems that could represent realistic use cases for each engine type. An AWS p3.2xlarge running Ubuntu Server 18.04LTS was used for GPU benchmarks. An Alienware 17 R5 with an Intel(R) Core(TM) i7-8750H running Ubuntu 18.04LTS was used for Qrack CPU benchmarks. Updated Qrack GPU benchmarks were collected May 19th, 2019. These results were combined with an earlier set of test results collected starting on the night of December 19th, 2018 into the morning of December 20th, and additional quantum Fourier transform ("QFT") benchmarks were collected on the night of January 5th, 2019 into the morning of January 6th. To reduce cost, code revisions on Github were compared between December 19th, 2018 and May 19th 2019, for QCGPU and ProjectQ, and lower qubit counts were run to determine that there is no significant change in the set of benchmarks from December and January. (If this is in error, we took care to try to report fair tests, within cost limitations, but please let us know.)

The average and quartile boundary values of each set of 100 were recorded and graphed. Grover's search to invert a black box subroutine, or "oracle," was similarly implemented for trials between 4 and 20 qubits, for QEngineOCL with and without QUnit and QFusion layers. Grover's algorithm was iterated an optimal number of times, vs. qubit count, to maximize probability on a half cycle of the algorithm's period, being :math:`floor\left[\frac{\pi}{4asin^2\left(1/\sqrt{2^N}\right)}\right]` iterations for :math:`N` qubits.

A quantum Fourier transform was run for 100 trials between 4 and 28 qubits on the GPU engine types. ProjectQ and QCGPU were also benchmarked for this test, on an AWS p3.2xlarge running Ubuntu Server 18.04LTS, with the benchmarking code provided by QCGPU, accessed from its Github repository on 12/1/18, in Python 3. The benchmarking script available from QCGPU iterates at random between the engine options, but it was slightly modified to alternate between trials of ProjectQ and QCGPU, to get exactly 100 samples apiece per qubit count.

QUnit was analyzed based on half its round trip time, for the application of the QFT followed by its inverse. This is a representative test of Qrack::QUnit performance, because QFT of a permutation basis eigenstate is generally much faster than the inverse operation applied to the result. QUnit explicitly separates the representation of qubit subsystems when it can, such that performance differs greatly between entirely separable and entirely entangled cases, hence we need to show various representative cases. Separability considerations do not affect maximally entangled representations, as are all other simulators tested here, (though ProjectQ includes a compilation layer on top of its default simulator). 

Heap usage was established as very closely matching theoretical in eariler benchmarks, and this has not fundamentally changed. Heap profiling was carried out with Valgrind Massif. Heap sampling was limited but ultimately sufficient to show statistical confidence.

Results
*******

We observed extremely close correspondence with theoretical complexity and RAM usage considerations for the behavior of all engine types. QEngineCPU and QEngineOCL require exponential time for a single gate on a coherent unit of N qubits. QUnit types with explicitly separated subsystems as per [Pednault2017]_ show constant time requirements for the same single gate.

.. image:: performance/x_single.png

.. image:: performance/cnot_single.png

QEngineCPU and QEngineOCL can perform many identical gates in parallel across entangled subsystems for an approximately constant costs, when total qubits in the engine are held fixed as breadth of the parallel gate application is varied. To test this, we can apply parallel gates at once across the full width of a coherent array of qubits. (CNOT is a two bit gate, so :math:`(N-1)/2` gates are applied to odd numbers of qubits.) Notice in these next graphs how QEngineCPU and QEngineOCL have similar scaling cost as the single gate graphs above, while QUnit types show a linear trend (appearing logarithmic on an exponential axis scale):

.. image:: performance/x_all.png

.. image:: performance/cnot_all.png

Heap sampling showed high confidence adherence to theoretical expecations. Complex numbers are represented as 2 single (32-bit) or 2 double (64-bit) accuracy floating point types, for real and imaginary components. The use of double or single precision is controlled by a compilation flag. There is one complex number per permutation in a separable subsystem of qubits. QUnit explicitly separates subsystems, while QEngine maintains complex amplitudes for all :math:`2^N` permutations of :math:`N` qubits. QEngines duplicate their state vectors once during many gates, like arithmetic gates, for speed and simplicity where it eases implementation.

.. image:: performance/qrack_ram.png

QUnit explicitly separates its representation of the quantum state and may operate with much less RAM, but QEngine's RAM usage represents approximately the worst case for QUnit, of maximal entanglement. OpenCL engine types attempt to use memory on the accelerator device instead of general heap when a QEngineOCL instance can fit a single copy of its state vector in a single allocation on the device, and when the total available device global memory is more than three times the size of the state vector. On many modern devices, state vectors up to about 1GB in size can be allocated directly on the accelerator device instead of using general heap. A auxiliary buffer used for normalization is half the size of the state vector, and this buffer is always allocated in general heap. 

Grover's algorithm is a relatively ideal test case, in that it allows a modicum of abstraction in implementation while representing an ostensibly practical and common task for truly quantum computational hardware. For 1 expected correct function inversion result, there is a well-defined highest likelihood search iteration count on half a period of the algorithm for a given number of oracle input permutations to search. Time required to run the test for QEngineCPU up to 20 qubits became prohibitive, hence we leave its variants off the graph. However, we can establish baseline for OpenCL performance, with Grover's search, and compare the performance of a "QEngine-method" to a "QUnit-method." This graphs shows average time against qubit count for an optimal half period search:

.. image:: performance/grovers.png

[Broda2016]_ discusses how Grover's might be adapted in practicality to actually "search an unstructured database," or search an unstructured lookup table, and Qrack is also capable of applying Grover's search to a lookup table with its IndexedLDA, IndexedADC, and IndexedSBC methods. Benchmarks are not given for this arguably more practical application of the algorithm, because few other quantum computer simulator libraries implement it, yet.

The quantum Fourier transform (QFT) is another realistic test case. Other simulators were also tested on the QFT. QFT operations were directly "chained," starting from the |0> permutation state. Qrack::QUnit was able to recover full (or virtually full) separability of qubits at every other step of 100 iterations, oscillating between modes of the "entangled" and "separable" QUnit median trends shown in the graph.

QEngineCPU took approximately 100 seconds per 1 trial (of 100) for 22 qubits and approximately 200 seconds for a 23 qubit QFT, and testing the QEngineCPU type therefore become prohibitive, for the full range of qubits between 4 and 28. To avoid confusion in the graph, and since QEngineCPU might therefore be impractical for large QFTs, we leave both it and its QUnit/QFusion variant off the graph.

.. image:: performance/qft.png

QEngineOCL generally outperforms the default simulator "backend" for ProjectQ. However, Qrack has also been wrapped as an optional backend for ProjectQ, and benchmarks for this layering of both projects will follow.

For lower numbers of qubits, QEngineOCL outperforms QCGPU. Both simulators follow a trend that appears to reach a knee of faster exponential growth. The "knee" comes at a lower number of qubits for QEngineOCL than for QCGPU, at about 19 or 18 qubits, versus 25 or 24 qubits for QCGPU.

QUnit was analyzed based on half its round trip time, for the application of the QFT followed by its inverse. The distribution of its times was log normal for the random input state distributions selected, so the times given are "exponent-mean-log," 2 raised to the mean of the log base 2 of the trial times, for which the mean closely matches the median and the standard deviations are consistently very small. QUnit represents its state vector in terms of decomposed subsystems, when possible and efficient. After an operation that should disentangle subsystems, QUnit can optionally try to separate the representations of independent subsystems, recovering a factor of 1/2, for each separated bit, of subsystem RAM and gate application time for further gates. QUnit outperforms all other simulators analyzed, when transforming a permutation basis eigenstate, without attempting to recover additional separability after the inverse transform. It fails to outperform QCGPU on half the round trip, if we attempt to separate QUnit's subsystems on the return trip, but further gate application will usually receive a large boost from this attempt at subsystem separation. A linear superposition of permutation states probably represents a realistic and fairly general set of inputs or outputs for the QFT; QUnit times are very close to QEngineOCL times for a random linear superposition of permutation basis input states.

Discussion
**********

Up to a consistent deviation at low qubit counts, speed and RAM usage is well predicted by theoretical complexity considerations of the gates, up to about a factor of 2 on heap usage for duplication of the state vector, with additional 1/2 the size of state vector allocated by QEngineOCL for an auxiliary normalization buffer.

In the comparative QFT benchmarks, the difference between QCGPU and Qrack in the "knee" in the base engine might be partially due to scalable work distribution in the OpenCL kernels. QEngineOCL is written to distribute work among an arbitrarily small number of processing elements and max work item size. Max work item size is a device-specific hardware parameter limiting how many work items may be dispatched in an OpenCL kernel call. QEngineOCL can distribute large numbers of probability amplitude transformations to small numbers of work items, incurring additional looping overhead, whereas QCGPU is written to dispatch one work item to one processing element. QCGPU requires a large enough hardware max work item size to add higher numbers of qubits, which might or not might not prove prohibitive in addressing the largest possible amount of general RAM on typical GPUs. Whereas QCGPU might not be, Qrack is theoretically compatible with OpenCL devices with smaller maximum work item counts, potentially such as CPUs. Additionally, Qrack normalizes its state vector at on-the-fly opportunities, to correct for float rounding error, incurring overhead costs but benefiting the accuracy of the simulation over very long strings of gate applications. QEngineOCL was also designed to support access by separate QEngineOCL instances in different threads to shared OpenCL devices, as well as optional out-of-order OpenCL queue execution, when available, which might add host overhead.

Further Work
************

Qrack previously contained two experimental multiprocessor types, "QEngineOCLMulti" based on the algorithms developed in Intel's [QHiPSTER]_, and the simpler QUnitMulti type, which dispatches different separable subsystems to different processors. These failed to outperform the single processor QEngineOCL. However, as Qrack has added optional support as a simulator for ProjectQ, we have effectively gained access to the quantum network simulator "SimulaQron" by SoftwareQuTech. At least one Qrack user is experimenting with scaling deployments of containers loaded with Qrack, ProjectQ, and SimulaQron as effective solution for multiprocessor and cluster operations, and the Qrack Team is looking at this and related approaches for this purpose. An asynchronous quantum P2P model, for effective multiprocessor support, should hopefully reduce inter-device communication overhead bottlenecks.

With a new generation of "VPU" processors coming in 2019, (for visual inference,) it might be possible to co-opt VPU capabilities for inference of raw state vector features, such as Schmidt separability, to improve the performance of QUnit. The authors of Qrack have started looking at this hardware for this purpose.

We will maintain systematic comparisons to published benchmarks of quantum computer simulation standard libraries, as they arise.

Conclusion
**********

Per [Pednault2017]_, explicitly separated subsystems of qubits in QUnit have a significant RAM and speed edge in many cases over the "Schrödinger algorithm" of QEngineCPU and QEngineOCL. One of Qrack's most novel optimizations is constant complexity or "free" scaling of bitwise parallelism in entangled subsystems, compared to linear complexity scaling without this optimization. Qrack gives very efficient performance on a single node up to at least about 30 qubits, up to the limit of maximal entanglement.

Citations
*********

.. target-notes::

.. [Broda2016] `Broda, Bogusław. "Quantum search of a real unstructured database." The European Physical Journal Plus 131.2 (2016): 38. <https://arxiv.org/abs/1502.04943>`_
.. [Pednault2017] `Pednault, Edwin, et al. "Breaking the 49-qubit barrier in the simulation of quantum circuits." arXiv preprint arXiv:1710.05867 (2017). <https://arxiv.org/abs/1710.05867>`_
.. [QSharp] `Q# <https://www.microsoft.com/en-us/quantum/development-kit>`_
.. [QHiPSTER] `QHipster <https://github.com/intel/Intel-QS>`_
.. [Quantiki] `Quantiki: List of QC simulators <https://www.quantiki.org/wiki/list-qc-simulators>`_
