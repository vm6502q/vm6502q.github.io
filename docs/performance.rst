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
are suitable as "high performance" candidates in the >32 qubit range. Many 
rely on the common approach often described as the "Schrödinger method," 
doubling RAM usage by a factor of 2 per fully interoperable qubit, or else 
Feynman path integrals, which can become intractible at arbitrary circuit depth.
Attempting to build on the work of IBM's `Breaking the 49-Qubit Barrier in the Simulation of Quantum Circuits` [Pednault2017]_ paper, 
with more recent attention to potential improvements inspired by Gottesman-Knill stabilizer simulators,
Qrack can execute surprisingly general circuits up to 63 qubits in width on modest single nodes, 
such as constrained cases of the QFT.

Qrack is an open-source quantum computer simulator option, implemented in C++, 
supporting integration into other popular compilers and interfaces, suitable for utilization in a wide variety
of projects.  As such, it is an ideal test-bed for establishing a set of
benchmarks useful for comparing performance between various quantum
simulators.

Qrack provides a "QEngineCPU" and a "QEngineOCL" that represent non-OpenCL and 
OpenCL base implementations, which could also be the most efficient types to 
use directly in the limit of maximal entanglement. For general use cases, 
the "QUnit" layer provides explicit Schmidt decomposition on top of another 
engine type (per [Pednault2017]_) and "QFusion" provides a "gate fusion" 
layer. A "QEngine" type is always the base layer, and QUnit and QFusion types 
may be layered over these, and over each other.

This version of the Qrack benchmarks contains comparisons against other
publicly available simulators, specifically QCGPU and ProjectQ (with its
default simulator). Qrack has been incorporated as an optional
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

100 timed trials of single and parallel gates were run for each qubit count between 5 and 28 qubits. The benchmarking code is available at `https://github.com/vm6502q/qrack/blob/master/test/benchmarks.cpp <https://github.com/vm6502q/qrack/blob/master/test/benchmarks.cpp>`_, in which qubit count can be set and tests can be selected on the command line. (Specifically, the branch "benchmarks_2019-10-19" was used.)

CPU and GPU benchmarks were run on two respective systems that could represent realistic use cases for each engine type. An AWS p3.2xlarge running Ubuntu Server 18.04LTS was used for GPU benchmarks. An Alienware 17 R5 with an Intel(R) Core(TM) i7-8750H running Ubuntu 18.04LTS was used for Qrack CPU benchmarks and FFTW3. QFT Benchmarks for FFTW3 were collected May 27th, 2019. QFT benchmarks for Qrack, ProjectQ, and QCGPU were collected on October 20th, 2019. Limited testing of Qrack for QFTs in excess of 32 qubits was done on both of these systems from October 17th through 20th. These results were combined with single gate, N-width gate and Grover's search benchmarks for Qrack, collected overnight from December 19th, 2018 into the morning of December 20th. (The potential difference since December in the particular Qrack tests reused from then should be insignificant. We took care to try to report fair tests, within cost limitations, but please let us know if you find anything that appears misrepresentative.)

The average and quartile boundary values of each set of 100 were recorded and graphed. Grover's search to invert a black box subroutine, or "oracle," was similarly implemented for trials between 5 and 20 qubits, for QEngineOCL with and without QUnit and QFusion layers. Grover's algorithm was iterated an optimal number of times, vs. qubit count, to maximize probability on a half cycle of the algorithm's period, being :math:`floor\left[\frac{\pi}{4asin^2\left(1/\sqrt{2^N}\right)}\right]` iterations for :math:`N` qubits.

A quantum Fourier transform was run for 100 trials between 5 and 28 qubits on the GPU engine types. ProjectQ, QCGPU, and FFTW3 were also benchmarked for this test. ProjectQ and QCGPU were tested on an AWS p3.2xlarge running Ubuntu Server 18.04LTS, with the benchmarking code provided by QCGPU, accessed from its Github repository on October 20th, 2019, in Python 3. The QCGPU benchmark script was modified, however, specifically in only the following ways:

1. Qiskit benchmarks were disabled. (On the test system, Qiskit took in excess of 7 seconds per QFT iteration at 20 qubits, and further testing would have quickly become cost and time prohibitive.)
2. State initialization for QCGPU and ProjectQ was moved *outside* of the timed portions of the benchmark loop, for equivalence to Qrack benchmarks.
3. A Hadamard gate call in the ProjectQ benchmarks was indented to repeat once per outer loop, rather than once in an entire trial. (It seemed clear to us that this was an indentation typo in the original script, as the "textbook" form of the QFT uses one "H" gate per bit, not one in total.)


The benchmarking script available from QCGPU iterates at random between the engine options, and 200 trials were collected between the two libraries it was used to benchmark. FFTW3 was tested on the aforementioned 17 R5, in "in-place" mode, since all other DFT tests are effectively "in-place" method, here.

Qrack's QUnit was analyzed both on its direct and sole execution of the QFT, as well as compared to half its round trip time for the application of the QFT followed by its inverse. Performance is greatly dependent on input state; the domain was restricted to states formed from random application of H gates, with 50% probability, to every bit of a randomly selected permutation. These two tests are reasonably representative of QUnit performance, because the forward QFT is generally much faster than the inverse operation applied to the QFT's result, or following the QFT with general circuits of significant depth. Initial or post-operation state considerations do not affect maximally entangled Schrödinger method representations, as are all other simulators tested here, (though ProjectQ includes a compilation layer on top of its default simulator, which is not targeted by these tests).

Qrack QEngine type heap usage was established as very closely matching theoretical expections, in earlier benchmarks, and this has not fundamentally changed. QUnit type heap usage varies greatly dependent on use case, though not in significant excess of QEngine types. No representative RAM benchmarks have been established for QUnit types, yet. QEngine Heap profiling was carried out with Valgrind Massif. Heap sampling was limited but ultimately sufficient to show statistical confidence.

Results
*******

We observed extremely close correspondence with Schrödinger method theoretical complexity and RAM usage considerations for the behavior of QEngine types. QEngineCPU and QEngineOCL require exponential time for a single gate on a coherent unit of N qubits. QUnit types with explicitly separated subsystems as per [Pednault2017]_ show constant time requirements for the same single gate.

.. image:: performance/x_single.png

.. image:: performance/cnot_single.png

QEngineCPU and QEngineOCL can perform many identical gates in parallel across entangled subsystems for an approximately constant costs, when total qubits in the engine are held fixed as breadth of the parallel gate application is varied. To test this, we can apply parallel gates at once across the full width of a coherent array of qubits. (CNOT is a two bit gate, so :math:`(N-1)/2` gates are applied to odd numbers of qubits.) Notice in these next graphs how QEngineCPU and QEngineOCL have similar scaling cost as the single gate graphs above, while QUnit types show a linear trend (appearing logarithmic on an exponential axis scale):

.. image:: performance/x_all.png

.. image:: performance/cnot_all.png

Heap sampling supports theoretical expecations to high confidence. Complex numbers are represented as 2 single (32-bit) or 2 double (64-bit) accuracy floating point types, for real and imaginary components. The use of double or single precision is controlled by a compilation flag. There is one complex number per permutation in a separable subsystem of qubits. QUnit explicitly separates subsystems, while QEngine maintains complex amplitudes for all :math:`2^N` permutations of :math:`N` qubits. QEngines duplicate their state vectors once during many gates, like arithmetic gates, for speed and simplicity where it eases implementation.

.. image:: performance/qrack_ram.png

QUnit explicitly separates its representation of the quantum state and may operate with much less RAM, but QEngine's RAM usage represents approximately the worst case for QUnit, of maximal entanglement. OpenCL engine types attempt to use memory on the accelerator device instead of general heap when a QEngineOCL instance can fit a single copy of its state vector in a single allocation on the device. On many modern devices, state vectors up to about 1GB in size can be allocated directly on the accelerator device instead of using general heap. A auxiliary buffer used for normalization is half the size of the state vector, and this buffer is always allocated in general heap.

Grover's algorithm is a relatively ideal test case, in that it allows a modicum of abstraction in implementation while representing an ostensibly practical and common task for truly quantum computational hardware. For 1 expected correct function inversion result, there is a well-defined highest likelihood search iteration count on half a period of the algorithm for a given number of oracle input permutations to search. Time required to run the test for QEngineCPU up to 20 qubits became prohibitive, hence we leave its variants off the graph. However, we can establish baseline for OpenCL performance, with Grover's search, and compare the performance of a "QEngine-method" to a "QUnit-method." This graphs shows average time against qubit count for an optimal half period search:

.. image:: performance/grovers.png

[Broda2016]_ discusses how Grover's might be adapted in practicality to actually "search an unstructured database," or search an unstructured lookup table, and Qrack is also capable of applying Grover's search to a lookup table with its IndexedLDA, IndexedADC, and IndexedSBC methods. Benchmarks are not given for this arguably more practical application of the algorithm, because few other quantum computer simulator libraries implement it, yet.

The "quantum" (or "discrete") Fourier transform (QFT/DFT) is another realistic test case. Other simulators were also tested on their QFT execution time, including QCGPU, ProjectQ, and FFTW3, (which is not explicitly quantum computer simulator software). For ease of comparison, in consideration of realistic use cases, the only Qrack options include are the (Schrödinger method) QEngineOCL and the one thought to be best for general use, "QUnit" (Schmidt decomposition) layered on "QFusion" ("gate fusion") layered on a collection of "QEngineOCL" instances (OpenCL Schrödinger method base engines).

.. image:: performance/qft.png

Both QEngineOCL and QUnit generally outperform the default simulator "backend" for ProjectQ. However, Qrack has also been wrapped as an optional backend for ProjectQ, and benchmarks for this layering of both projects will follow.

Though we are comparing CPU to GPU, CPU-based FFTW3 is clearly the best suited for low numbers of qubits, in general. After FFTW3, QEngineOCL outperforms QCGPU for low qubit widths. Both OpenCL simulators follow a trend that appears to reach a knee of faster exponential growth. The "knee" comes at a lower number of qubits for QEngineOCL than for QCGPU, at about 19 or 18 qubits, versus 25 or 24 qubits for QCGPU.

QUnit was analyzed based both on its QFT time alone, and on half its round-trip time for the QFT followed by its inverse. The distribution of its times was log normal for the random input state distributions selected, so the times given are "exponent-mean-log," 2 raised to the mean of the log base 2 of the trial times, for which the mean closely matches the median and the standard deviations are consistently very small. QUnit represents its state vector in terms of decomposed subsystems, when possible and efficient. On user and internal probability checks, QUnit will attempt to separate the representations of independent subsystems effectively by Schmidt decomposition. Further, it proactively attempts to avoid entanglement of representation in controlled and arithmetic gates. For each bit whose representation is separated this way, we recover a factor of close to or exactly 1/2 the subsystem RAM and gate execution time. Under the domain constraints, QUnit outperforms all other simulators analyzed, one-way at high qubit counts.

Since QUnit does not necessarily require the exponentially scaling resources of the "Schrödinger method," the authors thought to test it informally at much higher qubit counts. Qrack was initially designed in expectation that it might scale past 32 qubits, and 64 qubits was consciously chosen as the maximum potential allocation limit per single coherent instance, with easy ways to expand should 128 integral types become commonly available as a standard. A recently resolved issue opened by a third party on the Qrack Github repository prompted the authors to test and ensure support above the 32 qubit level, giving us confidence that addressing is now reliable up to 63 qubits. (Our tests do point to instability specifically in the 64th bit, commonly reserved for numerical sign in the 64 bit integral types Qrack relies on for qubit addressing.) One-way QFT benchmarks as in the graph above were left to run overnight starting October 17th, 2019 on the same Alienware 17 as other reported benchmarks, and no instability was detected from 4 to 63 qubits, for 100 random trials apiece. Execution time appears to plateau around the mid-30 qubit range; for quantitative point of reference, we report that 100 random trials of the 63 qubit QFT on an AWS p3.2xlarge completed on average (log-normal) in 19.0 seconds, the fastest taking 13.5 seconds and the slowest taking 28.3 seconds. (Remember that the domain was limited to states constructed from random Hadamard gates on bits of a random permutation.)

Discussion
**********

Up to a consistent deviation at low qubit counts, speed and RAM usage for Schrödinger method "QEngine" types is well predicted by theoretical complexity considerations of the gates, up to about a factor of 2 on heap usage for duplication of the state vector, with additional 1/2 the size of state vector allocated by QEngineOCL for an auxiliary normalization buffer. For the additional overhead in the comparative QFT benchmarks, the difference between QCGPU and QEngineOCL might come down to Qrack's support for a more general API and set of compatible systems.

Qrack is written for scalable work distribution in the OpenCL kernels. QEngineOCL will distribute work among an arbitrarily small number of processing elements and max work item size, smaller than state vector size. Max work item size is a device-specific hardware parameter limiting how many work items may be dispatched in an OpenCL kernel call. QEngineOCL can distribute large numbers of probability amplitude transformations to small numbers of work items, incurring additional looping overhead, whereas QCGPU is written to dispatch one work item to one processing element. QCGPU requires a large enough hardware max work item size to add higher numbers of qubits. Whereas QCGPU might not be, Qrack is theoretically compatible with OpenCL devices with smaller maximum work item counts, such as CPUs like the ARM7, on which Qrack has been regularly unit tested.

Qrack gives the option to normalize its state vector at on-the-fly opportunities, to correct for float rounding error, incurring overhead costs but benefiting the accuracy of the simulation over very long strings of gate applications. (Normalization was off in all benchmarks, but "host code" must switch between these options.) QEngineOCL was designed to support access by separate QEngineOCL instances in different threads to shared OpenCL devices, as well as optional out-of-order OpenCL queue execution, when available. QEngineOCL can also dispatch a queue of gates completely asynchronously, without blocking the main execution thread. Runtime options and design features to support a broad range of platforms do add to Qrack's execution overhead, but these make Qrack the best all-around quantum computer simulator for personal and heterogeneous hardware. 

Further Work
************

Qrack previously contained two experimental multiprocessor types, "QEngineOCLMulti" based on the algorithms developed in Intel's [QHiPSTER]_, and the simpler QUnitMulti type, which dispatches different separable subsystems to different processors. These failed to outperform the single processor QEngineOCL. However, as Qrack has added optional support as a simulator for ProjectQ, we have effectively gained access to the quantum network simulator "SimulaQron" by SoftwareQuTech. At least one Qrack user is experimenting with scaling deployments of containers loaded with Qrack, ProjectQ, and SimulaQron as an effective solution for multiprocessor and cluster operations, and the Qrack team is looking at this and related approaches for this purpose. An asynchronous quantum P2P model, for effective multiprocessor support, should hopefully reduce inter-device communication overhead bottlenecks.

Qrack seems to nearly be a strict super set of Gottesman-Knill "classically efficient" stabilizer simulators. Qracks supports freely invoking gates outside of a stabilizer's efficient Clifford algebra, as well as fundamental algorithmic improvements outside of any Clifford algebra. It is a high and immediate priority for the authors to handle the one or few remaining cases of CNOT gate optimization needed for a classically efficient strict super set of a Clifford algebra.

With the new generation of "VPU" processors coming in 2019 and 2020, (for visual inference,) it might be possible to co-opt VPU capabilities for inference of raw state vector features, such as Schmidt separability, to improve the performance of QUnit. The authors of Qrack have started looking at this hardware for this purpose.

We will maintain systematic comparisons to published benchmarks of quantum computer simulation standard libraries, as they arise.

Conclusion
**********

Per [Pednault2017]_, explicitly separated subsystems of qubits in QUnit have a significant RAM and speed edge in many cases over the "Schrödinger algorithm" of QEngineCPU and QEngineOCL. Qrack gives very efficient performance on a single node past 32 qubits, up to the limit of maximal entanglement.

Citations
*********

.. target-notes::

.. [Broda2016] `Broda, Bogusław. "Quantum search of a real unstructured database." The European Physical Journal Plus 131.2 (2016): 38. <https://arxiv.org/abs/1502.04943>`_
.. [Pednault2017] `Pednault, Edwin, et al. "Breaking the 49-qubit barrier in the simulation of quantum circuits." arXiv preprint arXiv:1710.05867 (2017). <https://arxiv.org/abs/1710.05867>`_
.. [QSharp] `Q# <https://www.microsoft.com/en-us/quantum/development-kit>`_
.. [QHiPSTER] `QHipster <https://github.com/intel/Intel-QS>`_
.. [Quantiki] `Quantiki: List of QC simulators <https://www.quantiki.org/wiki/list-qc-simulators>`_
