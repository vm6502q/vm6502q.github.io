Theory
======

Foundational Material
---------------------

A certain amount of prerequisite knowledge is necessary to utilize and understand quantum computational algorithms and processes.  Someday this material may be substantially diminished by intelligently chosen abstractions, but today quantum systems are still heavily dependent on an understanding of the underlying mathematical principles.

It is outside the scope of this document to cover that material. However, a set of references have been collected here.  These materials provide a sufficient foundation for onboarding a new engineer or scientist.

Quantum Computational Basics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. [Susskind] `Modern Physics: Quantum Mechanics, by Dr. Leonard Susskind <https://www.youtube.com/watch?v=2h1E3YJMKfA>`_
.. [WiredSummary] `Wired's Overview of the Industry <https://www.wired.com/story/the-era-of-quantum-computing-is-here-outlook-cloudy/>`_
.. [AlgoZoo] `Quantum Algorithm Zoo <https://math.nist.gov/quantum/zoo/>`_
.. [QC10th] `Quantum Computing 10th Edition - Nielson and Chuang <http://mmrc.amss.cas.cn/tlb/201702/W020170224608149940643.pdf>`_
.. [QAVLA] `Quantum Algorithms via Linear Algebra: A Primer - Lipton and Regan <http://mmrc.amss.cas.cn/tlb/201702/W020170224608149911380.pdf>`_

Grover Search Algorithm
~~~~~~~~~~~~~~~~~~~~~~~

.. [Grover] `Grover Search Algorithm <https://en.wikipedia.org/wiki/Grover%27s_algorithm>`_
.. [GroverSummary] `Introduction to Implementing Grover's Search Algorithm <http://twistedoakstudios.com/blog/Post2644_grovers-quantum-search-algorithm>`_
.. [GroverVisual] `Visualization of Grover's Search Algorithm <http://davidbkemp.github.io/animated-qubits/grover.html>`_
.. [Broda2016] `Broda, Bogusław. "Quantum search of a real unstructured database." The European Physical Journal Plus 131.2 (2016): 38. <https://arxiv.org/abs/1502.04943>`_

Quantum Bit Simulation
----------------------

Quantum bits are simulated by recording the complex number amplitude of a wave function solution to Schrödinger's equation. All this means is, we record one complex number with legnth between 0 and 1 correspendonding to each possible permutation of bits in the "coherent" set of quantum bits. The sum of the norms of all permutation amplitudes must sum to 1. The norm is given by the complex number times its "complex conjugate." The complex conjugate of a number is given by flipping the plus/minus sign on its imaginary component. Any possible state of a three qubit system can be expressed as follows:

.. math::
   :label: 3qb_amp

    \lvert\psi\rangle = x_0 \lvert 000\rangle + x_1 \lvert 001\rangle + x_2 \lvert 010\rangle + x_3 \lvert 011\rangle + \
                        x_4 \lvert 100\rangle + x_5 \lvert 101\rangle + x_6 \lvert 110\rangle + x_7 \lvert 111\rangle

Each of the :math:`x_n` are complex numbers. These are the amplitudes of the quantum system, multiplied times the "eigenstates." If all bits are measured simultaneously to check if they are 0 or 1, the norm of an amplitude gives its probability on measurement, resulting in a particular bit pattern based on the amplitudes and a randomly generated number.  While a real quantum mechanical system would produce probabilistic measurements based on these amplitudes, a simulation like this is deterministic, but pseudo-random.

.. note:: **Probability vs Amplitude**
	It is a common misconception that the defining characteristic of a quantum computer, compared to a classical computer, is that a quantum computer is probabilistic. Except, an :math:`x_n` `eigenstate <http://farside.ph.utexas.edu/teaching/qmech/Quantum/node40.html>`_ has both *probability* and a *phase.* It is not a bit with just a dimension of probability, but rather a bit with two dimensions, one of probablity and one of phase, an "amplitude."  The root of this misconception lies in the measurement operation, which can have a probabilistic outcome.  But the :math:`x_n` coefficients are not probabilistic values - rather, they are the amplitude of complex number wave function. If we both represent and measure the state as a permutation of 0 and 1 bits, the value of the wave function for any state is the square root of its probability times a `phase factor <https://en.wikipedia.org/wiki/Phase_factor>`_.

By collecting :math:`x_n` into a complex number array (called :cpp:member:`Qrack::QInterface::stateVec`), the full quantum representation of the system can be recorded using :math:`2^N` *complex number* variables for N quantum bits:

.. code-block:: cpp

    std::unique_ptr<Complex16[]> sv(new Complex16[1 << qBitCount]);

Given a standard :math:`X` gate matrix,

.. math::
    :label: x_gate

    \begin{bmatrix}
      0 & 1 \\
      1 & 0
    \end{bmatrix}

one might ask, how can this :math:`2\times2` matrix be applied against the :math:`1xN` vector for N arbitrary entangled qubits, where the vector is the :math:`x_n` amplitudes from :eq:`3qb_amp`?

.. math::

    \begin{bmatrix}
      0 & 1 \\
      1 & 0
    \end{bmatrix} ???
    \begin{bmatrix}
      x_{000} \\
      x_{001} \\
      x_{010} \\
      x_{011} \\
      x_{100} \\
      x_{101} \\
      x_{110} \\
      x_{111}
    \end{bmatrix}

To do so, we apply a `Kronecker product <https://en.wikipedia.org/wiki/Kronecker_product>`_ to the gate matrix.  This expands the matrix out to the appropriate number of dimensions - in this case we would need to perform two Kronecker products for each of the two bits whose values are irrelevant to the result:

.. math::
    :label: x_3bit

    \left(X \otimes I \otimes I\right) \times M

.. math::
    :label: x_3bit_2

    \left(\begin{bmatrix}
      0 & 1 \\\
      1 & 0
    \end{bmatrix}
    \otimes
    \begin{bmatrix}
      1 & 0 \\\
      0 & 1
    \end{bmatrix}
    \otimes
    \begin{bmatrix}
      1 & 0 \\\
      0 & 1
    \end{bmatrix}\right) \times
    \begin{bmatrix}
      x_{000} \\
      x_{001} \\
      x_{010} \\
      x_{011} \\
      x_{100} \\
      x_{101} \\
      x_{110} \\
      x_{111}
    \end{bmatrix}

.. math::
    :label: x_3bit_3

    \begin{bmatrix}
      0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\
      1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
      0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 \\
      0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\
      0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
      0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
      0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 \\
      0 & 0 & 0 & 0 & 0 & 0 & 1 & 0
    \end{bmatrix}
    \times
    \begin{bmatrix}
      x_{000} \\
      x_{001} \\
      x_{010} \\
      x_{011} \\
      x_{100} \\
      x_{101} \\
      x_{110} \\
      x_{111}
    \end{bmatrix}

.. math::
  :label: x_3bit_final

    (X \otimes I \otimes I) \times 
    \begin{bmatrix}
      x_{000} \\
      x_{001} \\
      x_{010} \\
      x_{011} \\
      x_{100} \\
      x_{101} \\
      x_{110} \\
      x_{111}
    \end{bmatrix}
    = 
    \begin{bmatrix}
      x_{001} \\
      x_{000} \\
      x_{011} \\
      x_{010} \\
      x_{101} \\
      x_{100} \\
      x_{111} \\
      x_{110}
    \end{bmatrix}

The operation in :eq:`x_3bit` swaps the amplitudes of 0 and 1 for the first bit out of three, but leaves the second and third bits alone.  Using the identity matrix :math:`I` preserves the amplitudes of the :math:`x_{0nn}` and :math:`x_{1nn}` positions.  The expanded matrix in :eq:`x_3bit_3` now has the proper dimensionality to be multiplied directly against the amplitude vector.

.. note:: It's important to remember here that, unlike a classical :math:`NOT` which directly inverts a bit, the :math:`X` gate swaps the *amplitudes* for the states where the qubit is 1 with the amplitudes where the qubit is 0. If the value of :math:`M[0]` is :math:`\lvert100\rangle`, then a subsequent :math:`X[0]` gate would exchange :math:`x_{100}` and :math:`x_{000}` and therefore leave the state as :math:`\lvert000\rangle`.  See `Quantum Logic Gates <https://en.wikipedia.org/wiki/Quantum_logic_gate#Circuit_composition_and_entangled_states>`_ for more information.

Implementing this naively would require matrices sized at :math:`2^{2N}` complex numbers for :math:`N` bits (as illustrated above in :eq:`x_3bit_3`).  This rapidly grows prohibitive in memory usage, and this is the primary limitation for simulating quantum systems using classical components.  Fortunately, these types of matrix operations are easily optimized for both memory usage and parallelization.

There are two immediate optimizations that can be performed.  The first is an optimization on the matrix size: by performing the math with only a :math:`2\times2` matrix, the amount of memory allocated is substantially reduced. The :cpp:func:`Qrack::QInterface::Apply2x2()` method utilizes this optimization.

In shorthand for clarity, an optimized :math:`X` gate is calculated using the following linear algebra:

.. math::
  :label: x_3bit_opt

  \begin{bmatrix}
    {
       \begin{bmatrix}
          0 & 1 \\
          1 & 0
       \end{bmatrix}
       \times
        \begin{bmatrix}
            x_{000} \\
            x_{001}
        \end{bmatrix}
    }\\
    {
       \begin{bmatrix}
          0 & 1 \\
          1 & 0
       \end{bmatrix}
       \times
        \begin{bmatrix}
            x_{010} \\
            x_{011}
        \end{bmatrix}
    }\\
    {
       \begin{bmatrix}
          0 & 1 \\
          1 & 0
       \end{bmatrix}
       \times
        \begin{bmatrix}
            x_{100} \\
            x_{101}
        \end{bmatrix}
    }\\
    {
       \begin{bmatrix}
          0 & 1 \\
          1 & 0
       \end{bmatrix}
       \times
        \begin{bmatrix}
            x_{110} \\
            x_{111}
        \end{bmatrix}
    }
  \end{bmatrix}
  =
  \begin{bmatrix}
      {
        \begin{bmatrix}
          x_{001} \\
          x_{000}
        \end{bmatrix}
      } \\
      {
        \begin{bmatrix}
          x_{011} \\
          x_{010}
        \end{bmatrix}
      } \\
      {
        \begin{bmatrix}
          x_{101} \\
          x_{100}
        \end{bmatrix}
      } \\
      {
        \begin{bmatrix}
          x_{111} \\
          x_{110}
        \end{bmatrix}
      }
  \end{bmatrix}

And, fully decomposing :eq:`x_3bit_opt`:

.. math::
    \begin{bmatrix}
      {
        \begin{bmatrix}
            0 & 1
        \end{bmatrix}
        \times
        \begin{bmatrix}
            x_{000} \\
            x_{001}
        \end{bmatrix}
      } \\
      {
        \begin{bmatrix}
            1 & 0
        \end{bmatrix}
        \times
        \begin{bmatrix}
            x_{000} \\
            x_{001}
        \end{bmatrix}
      } \\
      {
        \begin{bmatrix}
            0 & 1
        \end{bmatrix}
        \times
        \begin{bmatrix}
            x_{010} \\
            x_{011}
        \end{bmatrix}
      } \\
      {
        \begin{bmatrix}
            1 & 0
        \end{bmatrix}
        \times
        \begin{bmatrix}
            x_{010} \\
            x_{011}
        \end{bmatrix}
      } \\
      {
        \begin{bmatrix}
            0 & 1
        \end{bmatrix}
        \times
        \begin{bmatrix}
            x_{100} \\
            x_{101}
        \end{bmatrix}
      } \\
      {
        \begin{bmatrix}
            1 & 0
        \end{bmatrix}
        \times
        \begin{bmatrix}
            x_{100} \\
            x_{101}
        \end{bmatrix}
      } \\
      {
        \begin{bmatrix}
            0 & 1
        \end{bmatrix}
        \times
        \begin{bmatrix}
            x_{110} \\
            x_{111}
        \end{bmatrix}
      } \\
      {
        \begin{bmatrix}
            1 & 0
        \end{bmatrix}
        \times
        \begin{bmatrix}
            x_{110} \\
            x_{111}
        \end{bmatrix}
      }
    \end{bmatrix}
    =
    \begin{bmatrix}
      x_{001} \\
      x_{000} \\
      x_{011} \\
      x_{010} \\
      x_{101} \\
      x_{100} \\
      x_{111} \\
      x_{110}
    \end{bmatrix}

It's worth pointing out that the operation detailed in :eq:`x_3bit_opt` is heavily parallelize-able, yielding substantial benefits when working with gates spanning more than just one register (e.g. :math:`CNOT` and :math:`CCNOT` gates).  In C++, this would be implemented like so:

.. code-block:: cpp

    // Create a three qubit register.
    Qrack::QInterface qReg(3);

    // X-gate the bit at index 0
    qReg->X(0);

The second optimization is to maintain separability of state vectors between bits where entanglement is not necessary.  See IBM's `article <https://www.ibm.com/blogs/research/2017/10/quantum-computing-barrier/>`_ and related `publication <https://arxiv.org/abs/1710.05867>`_ for details on how to optimize these operations in more detail.  The :cpp:class:`Qrack::QUnit` and :cpp:class:`Qrack::QInterface` register-wide operations (e.g. :cpp:func:`Qrack::QInterface::X`) leverage these types of optimizations, with parallelization provided through threading and OpenCL, as supported.

LDA,X Unitary Matrix
~~~~~~~~~~~~~~~~~~~~

Note that the VM6502Q X-addressed LDA, ADC, and SBC operations can load, add, or subtract with a superposed X register. If the permutation states of the classical memory addressed by the X register are treated as quantum degrees of freedom, these operations are unitary. A simplified example of the unitary matrix or operator for 2 qubits and a "lookup table" of two independent bits is given below. The least significant bit is the index (or X register), the second least significant bit is the value (or accumulator), and the third and fourth bits are the 0 and 1 indexed classical bits in the "lookup table," treated as quantum degrees of freedom. The rows and columns of the matrix proceed in bit signifance permutation order from :math:`\lvert0000\rangle` to :math:`\lvert1111\rangle`.

.. math::

	\begin{bmatrix}
		1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
		0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
		0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
		0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
		0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
		0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
		0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
		0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
		0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
		0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 \\
		0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\
		0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\
		0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\
		0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 \\
		0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
		0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 
	\end{bmatrix}

This allows search of a "real unstructured database" or unstructured lookup table, per [Broda2016]. That paper also proposes a model for the memory of the lookup table.

6502 Reference Documents
------------------------

.. [MOS-6502] The 6502 CPU - https://en.wikipedia.org/wiki/MOS_Technology_6502
.. [6502ASM] 6502 Assembly Reference - http://www.6502.org/tutorials/6502opcodes.html

For details on the added opcodes supported by vm6502q, see :ref:`mos-6502q-opcodes`.
