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

Future publications will compare the performance of Qrack against other
publicly available simulators, as rigorous implementations can be implemented.

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

100 timed trials of single and parallel gates were run for each qubit count between 3 and 28 qubits. CPU and GPU benchmarks were run on two respective systems that could represent realistic use cases for each engine type. An AWS p3.2xlarge running Ubuntu Server 18.04LTS was used for GPU benchmarks. An Alienware 17 R5 with an Intel(R) Core(TM) i7-8750H running Ubuntu 18.04LTS was used for CPU benchmarks. The average and quartile boundary values of each set of 100 were recorded and graphed. Grover's search to invert a black box subroutine, or "oracle," was similarly implemented for trials between 3 and 18 qubits. Grover's algorithm was iterated an optimal number of times, vs. qubit count, to maximize probability on a half cycle of the algorithm's period, being :math:`floor\left[\frac{\pi}{4asin\left(1/\sqrt{2^N}\right)}\right]` iterations for :math:`N` qubits. A quantum fourier transform was run for 100 trials between 3 and 26 qubits on the GPU engine types.

Heap profiling was carried out with Valgrind Massif. Heap sampling was limited but ultimately sufficient to show statistical confidence.

Results
*******

We observed extremely close correspondence with theoretical complexity and RAM usage considerations for the behavior of all engine types. QEngineCPU and QEngineOCL require exponential time for a single gate on a coherent unit of N qubits. QUnit types with explicitly separated subsystems as per [Pednault2017]_ show constant time requirements for the same single gate.

.. image:: performance/x_single.png

.. image:: performance/cnot_single.png

QEngineCPU and QEngineOCL can perform many identical gates in parallel across entangled subsystems for an approximately constant costs, when total qubits in the engine are held fixed as breadth of the parallel gate application is varied. To test this, we can apply parallel gates at once across the full width of a coherent array of qubits. (CNOT is a two bit gate, so :math:`(N-1)/2` gates are applied to odd numbers of qubits.) Notice in these next graphs how QEngineCPU and QEngineOCL have similar scaling cost as the single gate graphs above, while QUnit types show a linear trend (appearing logarithmic on an exponential axis scale):

.. image:: performance/x_all.png

.. image:: performance/cnot_all.png

Heap sampling showed high confidence adherence to theoretical expecations. Complex numbers are represented as 2 double (64-bit) or 2 single (32-bit) accuracy floating point types, for real and imaginary components. The use of double or single precision is controlled by a compilation flag. There is one complex number per permutation in a separable subsystem of qubits. QUnit explicitly separates subsystems, while QEngine maintains complex amplitudes for all :math:`2^N` permutations of :math:`N` qubits. QEngines duplicate their state vectors once during most gates for speed and simplicity where it eases implementation.

.. image:: performance/qrack_ram.png

Grover's algorithm is a relatively ideal test case, in that it allows a modicum of abstraction in implementation while representing an ostensibly practical and common task for truly quantum computational hardware. For 1 expected correct function inversion result, there is a well-defined highest likelihood search iteration count on half a period of the algorithm for a given number of oracle input permutations to search. This graphs shows average time against qubit count for an optimal half period search:

.. image:: performance/grovers.png

[Broda2016]_ discusses how Grover's might be adapted in practicality to actually "search an unstructured database," or search an unstructured lookup table, and Qrack is also capable of applying Grover's search to a lookup table with its IndexedLDA, IndexedADC, and IndexedSBC methods. Benchmarks are not given for this arguably more practical application of the algorithm, because few other quantum computer simulator libraries implement it, yet.

The Quantum Fourier transform (QFT) is another realistic test case. QEngineCPU took approximately 100 seconds per 1 trial (of 100) for 22 qubits and approximately 200 seconds for a 23 qubit QFT, and testing the QEngineCPU type therefore become prohibitive, for the full range of qubits between 3 and 26. To avoid confusion in the graph, and since QEngineCPU might therefore be impractical for large QFTs, we leave both it and its QUnit/QFusion variant off the graph. At the end of every QFT trial, explicit separability was reset with a measurement operation across the entire Fourier-transformed unit of qubits, i.e. every trial was the ideal case for QUnit of entirely separable qubits, which might not always be realistic. However, the whole set is fairly representative of the difference between best case, of total initial separability for QUnit, and worst case, of total entanglement, with QUnit's performance limiting to the QEngine case, plus modest wrapping and separability testing overhead from the QUnit layer. In the best case for QUnit, significant additional overhead is incurred in combining an additional qubit into the bulk QEngine at each step.

.. image:: performance/qft.png

Discussion
**********

Up to a consistent deviation at low qubit counts, speed and RAM usage is well predicted by theoretical complexity considerations of the gates, up to a factor of 2 on heap usage for duplication of the state vector.

Further Work
************

Qrack contains an experimental multiprocessor type, previously "QEngineOCLMulti" based on the algorithms developed in Intel's [QHiPSTER]_, currently replaced in favor of the simpler QUnitMulti type, which dispatches different separable subsystems to different processors. Current and previous generation multiprocessor types fail to outperform the single processor QEngineOCL. We include it in the current release to help the open source community realize a practical multiprocessor implementation in the context of Qrack.

Qrack has been successfully run on multiple processors at once, and even on clusters, but not with practical performance for real application; a good next step is to redesign the multiprocessor engine type(s) to actually outperform the single device engine. Also, CPU "software" implementation parallelism relies on certain potentially expensive standard library functionality, like lambda expressions parallel "futures," and might still be optimized. Further, there is still opportunity for better explicit qubit subsystem separation in QUnit.

With a new generation of "VPU" processors coming in 2019, (for visual inference,) it might be possible to co-opt VPU capabilities for inference of raw state vector features, such as Schmidt separability, to improve the performance of QUnit. The authors of Qrack have just started looking at this hardware for this purpose.

We will also develop and maintain systematic comparisons to published benchmarks of quantum computer simulation standard libraries, as they arise.

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
