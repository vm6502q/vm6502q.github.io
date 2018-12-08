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

Qrack provides high performance in the 20-30 qubit range, as well as an
open-source implementation in C++ suitable for utilization in a wide variety
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

100 timed trials of single and parallel gates were run for each qubit count between 3 and 28 qubits. The benchmarking code is available at `https://github.com/vm6502q/qrack/blob/master/test/benchmarks.cpp <https://github.com/vm6502q/qrack/blob/master/test/benchmarks.cpp>`_, in which qubit count can be set and tests can be commented out to reduce to the relevant subset. CPU and GPU benchmarks were run on two respective systems that could represent realistic use cases for each engine type. An AWS p3.2xlarge running Ubuntu Server 18.04LTS was used for GPU benchmarks. An Alienware 17 R5 with an Intel(R) Core(TM) i7-8750H running Ubuntu 18.04LTS was used for Qrack CPU benchmarks. The test results were collected from 11/25/18 to 12/8/18. The average and quartile boundary values of each set of 100 were recorded and graphed. Grover's search to invert a black box subroutine, or "oracle," was similarly implemented for trials between 3 and 18 qubits. Grover's algorithm was iterated an optimal number of times, vs. qubit count, to maximize probability on a half cycle of the algorithm's period, being :math:`floor\left[\frac{\pi}{4asin\left(1/\sqrt{2^N}\right)}\right]` iterations for :math:`N` qubits.

A quantum fourier transform was run for 100 trials between 3 and 26 qubits on the GPU engine types. ProjectQ and QCGPU were also benchmarked for this test, on an AWS p3.2xlarge running Ubuntu Server 18.04LTS, with the benchmarking code provided by QCGPU, accessed from its Github repository on 12/1/18, in Python 3. The benchmarking script from QCGPU iterates at random between the engine options. 200 total trials were run for the set of QCGPU and ProjectQ, selected by approximately equal probability random chance in each of the 200 iterations.

QFT iterations were "chained" in a unitary manner starting from the |0> permutation state, such that every other step entangled and unentangled the full set of qubits. This is a representative test of Qrack::QUnit performance, because it varies largely with degree of representational entanglement. QUnit explicitly separates the representation of qubit subsystems when it can, such that performance differs greatly between entirely separable and entirely entangled cases, hence we need to show best and worst cases. Separability considerations do not affect maximally entangled representations, as are all other simulators tested here, (though ProjectQ includes a compilation layer on top of its default simulator). 

Heap profiling was carried out with Valgrind Massif. Heap sampling was limited but ultimately sufficient to show statistical confidence.

Results
*******

We observed extremely close correspondence with theoretical complexity and RAM usage considerations for the behavior of all engine types. QEngineCPU and QEngineOCL require exponential time for a single gate on a coherent unit of N qubits. QUnit types with explicitly separated subsystems as per [Pednault2017]_ show constant time requirements for the same single gate.

.. image:: performance/x_single.png

.. image:: performance/cnot_single.png

QEngineCPU and QEngineOCL can perform many identical gates in parallel across entangled subsystems for an approximately constant costs, when total qubits in the engine are held fixed as breadth of the parallel gate application is varied. To test this, we can apply parallel gates at once across the full width of a coherent array of qubits. (CNOT is a two bit gate, so :math:`(N-1)/2` gates are applied to odd numbers of qubits.) Notice in these next graphs how QEngineCPU and QEngineOCL have similar scaling cost as the single gate graphs above, while QUnit types show a linear trend (appearing logarithmic on an exponential axis scale):

.. image:: performance/x_all.png

.. image:: performance/cnot_all.png

Heap sampling showed high confidence adherence to theoretical expecations. Complex numbers are represented as 2 single (32-bit) or 2 double (64-bit) accuracy floating point types, for real and imaginary components. The use of double or single precision is controlled by a compilation flag. There is one complex number per permutation in a separable subsystem of qubits. QUnit explicitly separates subsystems, while QEngine maintains complex amplitudes for all :math:`2^N` permutations of :math:`N` qubits. QEngines duplicate their state vectors once during most gates for speed and simplicity where it eases implementation.

.. image:: performance/qrack_ram.png

QUnit explicitly separates its representation of the quantum state and may operate with much less RAM, but QEngine's RAM usage represents approximately the worst case for QUnit, of maximal entanglement.

Grover's algorithm is a relatively ideal test case, in that it allows a modicum of abstraction in implementation while representing an ostensibly practical and common task for truly quantum computational hardware. For 1 expected correct function inversion result, there is a well-defined highest likelihood search iteration count on half a period of the algorithm for a given number of oracle input permutations to search. This graphs shows average time against qubit count for an optimal half period search:

.. image:: performance/grovers.png

[Broda2016]_ discusses how Grover's might be adapted in practicality to actually "search an unstructured database," or search an unstructured lookup table, and Qrack is also capable of applying Grover's search to a lookup table with its IndexedLDA, IndexedADC, and IndexedSBC methods. Benchmarks are not given for this arguably more practical application of the algorithm, because few other quantum computer simulator libraries implement it, yet.

The Quantum Fourier transform (QFT) is another realistic test case. Other simulators were also tested on the QFT. QFT operations were directly "chained," starting from the |0> permutation state. Qrack::QUnit was able to recover full (or virtually full) separability of qubits at every other step of 100 iterations, oscillating between modes of the "entangled" and "separable" QUnit median trends shown in the graph.

QEngineCPU took approximately 100 seconds per 1 trial (of 100) for 22 qubits and approximately 200 seconds for a 23 qubit QFT, and testing the QEngineCPU type therefore become prohibitive, for the full range of qubits between 3 and 26. To avoid confusion in the graph, and since QEngineCPU might therefore be impractical for large QFTs, we leave both it and its QUnit/QFusion variant off the graph.

For Qrack, both the amount of time the GPU was blocked for execution ("GPU Time") and the time for asynchronous gate methods to return, while work continues to run on the GPU ("Async Return") are shown. The difference between these two graphs is potentially recoverable for (light to moderate) auxiliary execution of work on the host CPU, while the GPU works asynchronously.

.. image:: performance/qft.png

For lower numbers of qubits, QEngineOCL outperforms QCGPU. Both simulators follow an exponential trend that appears to reach a knee of faster growth. The "knee" comes at a lower number of qubits for QEngineOCL than for QCGPU, at about 18 qubits of GPU time and 21 qubits of asynchronous host dispatch versus 25 or 24 qubits for QCGPU. There is no such "knee" apparent for the QUnit -> QFusion layers on top of a QEngine type. We will analyze the comparative results in the discussion section.

Discussion
**********

Up to a consistent deviation at low qubit counts, speed and RAM usage is well predicted by theoretical complexity considerations of the gates, up to a factor of 2 on heap usage for duplication of the state vector.

In the comparative QFT benchmarks, the difference between QCGPU and Qrack in the "knee" in the base engine might be partially due to scalable work distribution in the OpenCL kernels. QEngineOCL is written to distribute work among an arbitrarily small number of processing elements and max work item size. Max work item size is a device-specific hardware parameter limiting how many work items may be dispatched in an OpenCL kernel call. QEngineOCL can distribute large numbers of probability amplitude transformations to small numbers of work items, incurring additional looping overhead, whereas QCGPU is written to dispatch one work item to one processing element. QCGPU requires a large enough hardware max work item size to add higher numbers of qubits, which might or not might not prove prohibitive in addressing the largest possible amount of general RAM. Additionally, Qrack normalizes its state vector at on-the-fly opportunities, to correct for float rounding error, incurring overhead costs but benefiting the accuracy of the simulation over very long strings of gate applications. QEngineOCL was also designed to support thread-safe parallel access to the QEngine and the OpenCL device queues it uses, as well as out-of-order OpenCL queue execution, which might add host overhead.

Further Work
************

Qrack contains an experimental multiprocessor type, previously "QEngineOCLMulti" based on the algorithms developed in Intel's [QHiPSTER]_, currently replaced in favor of the simpler QUnitMulti type, which dispatches different separable subsystems to different processors. Current and previous generation multiprocessor types fail to outperform the single processor QEngineOCL. We include it in the current release to help the open source community realize a practical multiprocessor implementation in the context of Qrack.

Qrack has been successfully run on multiple processors at once, and even on clusters, but not with practical performance for real application; a good next step is to redesign the multiprocessor engine type(s) to actually outperform the single device engine. Also, CPU "software" implementation parallelism relies on certain potentially expensive standard library functionality, like lambda expressions parallel "futures," and might still be optimized. Further, there is still opportunity for better explicit qubit subsystem separation in QUnit.

With a new generation of "VPU" processors coming in 2019, (for visual inference,) it might be possible to co-opt VPU capabilities for inference of raw state vector features, such as Schmidt separability, to improve the performance of QUnit. The authors of Qrack have just started looking at this hardware for this purpose.

We will maintain systematic comparisons to published benchmarks of quantum computer simulation standard libraries, as they arise.

Conclusion
**********

Per [Pednault2017]_, explicitly separated subsystems of qubits in QUnit have a significant RAM and speed edge in many cases over the "Schrödinger algorithm" of QEngineCPU and QEngineOCL. One of Qrack's greatest new optimizations to either general algorithm is constant complexity or "free" scaling of bitwise parallelism in entangled subsystems, compared to linear complexity scaling without this optimization. Qrack gives very efficient performance on a single node up to at least about 30 qubits, in the limit of maximal entanglement.

Citations
*********

.. target-notes::

.. [Broda2016] `Broda, Bogusław. "Quantum search of a real unstructured database." The European Physical Journal Plus 131.2 (2016): 38. <https://arxiv.org/abs/1502.04943>`_
.. [Pednault2017] `Pednault, Edwin, et al. "Breaking the 49-qubit barrier in the simulation of quantum circuits." arXiv preprint arXiv:1710.05867 (2017). <https://arxiv.org/abs/1710.05867>`_
.. [QSharp] `Q# <https://www.microsoft.com/en-us/quantum/development-kit>`_
.. [QHiPSTER] `QHipster <https://github.com/intel/Intel-QS>`_
.. [Quantiki] `Quantiki: List of QC simulators <https://www.quantiki.org/wiki/list-qc-simulators>`_
