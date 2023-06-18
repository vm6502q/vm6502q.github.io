Hardware Compilation
---------------

Efficient Unitary Clifford+RZ Simulation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
`QStabilizerHybrid` is classically efficient for the gate set "Clifford+RZ," (or "Clifford+T,") except for measurement, since v8.12. The entire unitary portion of circuit simulation, before measurement, has a polynomial-complexity simulation algorithm, in space and time requirements. If measuring across the full width of the simulator, or sampling, measurement (alone) scales exponentially in space requirements proportional to (less than or up to) the number of non-Clifford RZ (or T) gates, and exponentially in time requirements proportional to base logical qubit count in the simulator instance.

No special considerations are necessary to engage this simulation mode: simply restrict your gate set to Clifford+RZ, when using any simulator that properly includes the `QStabilizerHybrid` layer, such as the default optimal simulator stack.

Output Unitary Clifford+RZ Simulation For Quantum Hardware
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
It is theoretically possible to use the Clifford+RZ improvements of v8.12 to compile for hardware. (`QUnit` cannot be used over `QStabilizerHybrid`, for this.)

Since v8.13, it is now possible to pipe any output `QStabilizerHybrid` state to an output stream with the `<<` operator, (not while using `QUnit`). The files have the following format, by line:

[Logical qubit count]

[Stabilizer qubit count, including ancillae]

[Stabilizer x/z/r generators, one row per line, "tableau" format, repeated for logical qubit count of rows x2]

[Per-qubit MPS buffers, 2x2 complex matrices, row-major order, one matrix per line, repeated for stabilizer qubit count of rows]


For example:

3

3

1 1 0 0 1 0 2

0 1 0 1 0 0 0

0 0 0 0 0 1 0

0 0 0 1 0 1 2

0 0 0 1 1 0 0

1 1 1 0 1 0 0

(1,0) (0,0) (0,0) (1,0)

(1,0) (0,0) (0,0) (1,0)

(0,0) (0.707107,-0.707107) (0,1) (0,0)

is a valid file, with 0 ancillae. It is theoretically relatively easy to prepare this result of unitary circuit simulation on a quantum hardware device: first prepare the stabilizer state, (with purely Clifford gates,) then apply the (potentially non-Clifford) 2x2 matrices over the same sequential qubit index order. This can represent a universal quantum state of the logical qubits.

`QInterface` now has a method void `SetStabilizerHardwareEncoded(bool)`. The default value of this setting is false, which causes ancilla "magic state" qubit "channels" to be encoded depending on post-selection. If this setting is false, hardware decoding depends on the ancilla qubits all measuring as \|0>, for the correct overall state preparation. However, if this setting is true, then every other ancilla qubit (starting with the second-occurring ancilla) is an "open channel" that starts out coding an identity gate, (or "no operation,") but can be re-encoded to avoid the post-selection requirement. To do so, after preparing the state as described in the file, perform `H` gate on all auxiliary, identity-encoding ancilla channels, act `CZ` from each "coding" ancilla to its "identity" partner, then act `H` again on the auxiliary, identity-encoding ancilla. Now, terminal measurement can occur without post-selection, and all logical qubits are deterministically in the intended state, in the ideal.

It might be helpful to look at Qiskit's `Clifford <https://qiskit.org/documentation/stubs/qiskit.quantum_info.Clifford.html>_` and `Clifford.to_circuit() <https://qiskit.org/documentation/stubs/qiskit.quantum_info.Clifford.to_circuit.html#qiskit.quantum_info.Clifford.to_circuit>_` to convert this stabilizer state representation to a state preparation circuit.
