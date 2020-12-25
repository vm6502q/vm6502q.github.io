Examples
--------

Qrack Examples
~~~~~~~~~~~~~~

Check the Qrack repository ``examples`` directory for compiled single file source examples of Qrack usage. (These build with ``make all`` or ``make [example-name]``, with the name of the source file). This is an overview of their content:

grovers - Runs a canonical Grover's search

grovers_lookup - Runs a variant of Grover's search that searches a lookup table for a target value

ordered_list_search - Runs Grover's search base case iterations on an ordered list, potentially recovering a theoretical "big-O" complexity reduction of x2 over the canonical binary search.

pearson32 - Hashes a value according to the Pearson 32 bit hashing algorithm, and searches a superposed set of hashes for a target quality, (like "proof-of-work")

qneuron_classification - Runs a simple quantum neuron classification model task

quantum_associative_memory - Creates an example of an associative memory via a simple quantum neuron model

quantum_perceptron - Demonstrates the single neuron "atom" of Qrack's quantum neural net examples

shors_factoring - Carries out a simulation of Shor's factoring algorithm integers

teleport - Demonstrates quantum teleportation

VM6502Q Examples
~~~~~~~~~~~~~~~~

The `quantum enabled cc65 <https://github.com/vm6502q/cc65>`_ compiler provides a mechanism to both compile the `examples <https://github.com/vm6502q/examples>`_ as well as develop new programs to execute on the *vm6502q* virtual machine.  These changes live on the *6502q* branch.

Start by compiling the *cc65* repository and the *vm6502q* virtual machine:

.. code-block:: bash

      cc65/ $ git checkout 6502q
      cc65/ $ make
            ...
   vm6502q/ $ make

Then, make the various examples:

.. code-block:: bash

  examples/ $ cd hello_c && make
            # OR if to directly execute within the emulator
  examples/ $ cd hello_c && make run
            ...

.. code-block:: text

            hello world
            ^C
            Interrupted at e002

            Emulation performance stats is OFF.

            *-------------*-----------------------*----------*----------*
            |  PC: $e002  |  Acc: $0e (00001110)  |  X: $55  |  Y: $0c  |
            *-------------*-----------------------*----------*----------*
            |  NVQBDIZC   | :
            |  00000100   | :
            *-------------*

            Stack: $f7
                   [03 04 03 04 e2 00 fe 01 ]

            I/O status: enabled,  at: $e000,  local echo: OFF.
            Graphics status: disabled,  at: $e002
            ROM: disabled. Range: $d000 - $dfff.
            Op-code execute history: disabled.
            ------------------------------------+----------------------------------------
               C - continue,  S - step          |    A - set address for next step
               G - go/cont. from new address    |    N - go number of steps, P - IRQ
               I - toggle char I/O emulation    |    X - execute from new address
               T - show I/O console             |    B - blank (clear) screen
               E - toggle I/O local echo        |    F - toggle registers animation
               J - set animation delay          |    M - dump memory, W - write memory
               K - toggle ROM emulation         |    R - show registers, Y - snapshot
               L - load memory image            |    O - display op-code exec. history
               D - disassemble code in memory   |    Q - quit, 0 - reset, H - help
               V - toggle graphics emulation    |    U - enable/disable exec. history
               Z - enable/disable debug traces  |    1 - enable/disable perf. stats
               2 - display debug traces         |    ? - show this menu
            ------------------------------------+----------------------------------------
            > q
            Thank you for using VM65.
             
Use *Ctrl-C* to bring up the in-VM menu, and *q* to exit.

Creating a new example
======================

 * Copy the ``prototype/`` directory to your example name, renaming the ``.cfg`` file to match the source file.
 * Change ``prototype`` in ``Makefile`` to be the basename of your cfg and source file.
 * Adjust the ``project.cfg`` file as necessary for memory sizing.

