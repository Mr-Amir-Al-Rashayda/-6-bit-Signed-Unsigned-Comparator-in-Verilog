# 6-bit Signed/Unsigned Comparator in Verilog

## Overview

This project implements a **6-bit digital comparator** in Verilog, capable of performing both **signed** (2's complement) and **unsigned** comparisons. The comparator takes two 6-bit inputs and a mode-select signal to determine if the first input is **equal to**, **greater than**, or **smaller than** the second input, asserting the corresponding output signal. The design is implemented at the **gate-level** using standard logic gates and D flip-flops. A comprehensive testbench is included to verify the functionality across all possible input combinations and modes.

## Files

*   `comparator_6bit.v`: Contains the Verilog module for the 6-bit comparator. (Note: The original file `Project+Advance+Comparator.txt` should be renamed to `comparator_6bit.v`).
*   `comparator_tb2.v`: Contains the Verilog testbench module for verifying the `comparator_6bit` module. (Note: The original file `TB.txt` should be renamed to `comparator_tb2.v`).
*   (`Report.pdf`): An external PDF report detailing the design philosophy, theoretical overview, gate-level implementation, testing framework, simulation results, and performance analysis (propagation delay, clock period, frequency).

## Functionality

The `comparator_6bit` module has the following inputs and outputs:

*   **Inputs:**
    *   `clk`: Clock signal for input/output registration.
    *   `A [5:0]`: The first 6-bit input bus.
    *   `B [5:0]`: The second 6-bit input bus.
    *   `S`: Mode-select signal.
        *   `S = 0`: Unsigned comparison.
        *   `S = 1`: Signed (2's complement) comparison.
*   **Outputs:**
    *   `Equal`: Asserted high if `A` is equal to `B` in the selected mode.
    *   `Greater`: Asserted high if `A` is greater than `B` in the selected mode.
    *   `Smaller`: Asserted high if `A` is smaller than `B` in the selected mode.

Only one of the output signals (`Equal`, `Greater`, `Smaller`) should be high at any given time.

## Design Implementation

The comparator is designed structurally at the gate level, instantiating basic logic gates:

*   `INV`: Inverter (Delay: 3 ns)
*   `NAND2`: 2-input NAND gate (Delay: 5 ns)
*   `NOR2`: 2-input NOR gate (Delay: 5 ns)
*   `AND2`: 2-input AND gate (Delay: 8 ns)
*   `OR2`: 2-input OR gate (Delay: 8 ns)
*   `XNOR2`: 2-input XNOR gate (Delay: 10 ns)
*   `XOR2`: 2-input XOR gate (Delay: 12 ns)
*   `DFF`: D Flip-flop (Used for registering inputs A, B, S and outputs Equal, Greater, Smaller based on the clock edge).

The logic implements the necessary boolean expressions for equality, unsigned greater-than, and signed greater-than, and derives the smaller-than output.

## Testing

A thorough testbench (`comparator_tb2.v`) is provided to ensure the correctness and robustness of the comparator.

*   **Comprehensive Coverage:** The testbench iterates through all possible combinations of 6-bit inputs for both `A` and `B` (2^6 * 2^6 = 4096 combinations).
*   **Dual-Mode Testing:** For *each* input pair, both unsigned (`S=0`) and signed (`S=1`) comparisons are tested. This results in a total of 8192 test cases.
*   **Validation:** For each test case, the testbench calculates the expected outputs (`ExpEqual`, `ExpGreater`, `ExpSmaller`) based on the input values and the comparison mode, and then compares these expected values against the actual outputs from the Unit Under Test (UUT).
*   **Error Reporting:** Detailed messages are displayed in the simulation console for any mismatch between expected and actual outputs, indicating the inputs and the failing mode.
*   **Summary:** A final summary reports the total number of tests run, the number of errors detected, and the overall success rate.
*   **Clocking:** The testbench provides a clock signal (`clk`) and ensures that sufficient time (multiple clock cycles) is allowed after changing inputs and the mode select (`S`) before checking the outputs to account for the gate propagation delays and DFF settling time.

## Performance Considerations

*(Detailed performance analysis is available in the accompanying Report.pdf)*

Based on the gate delays and the structural implementation, the critical path delay (longest combinational path) is analyzed. The minimum recommended clock period is determined to be twice the maximum propagation delay to ensure stable operation and avoid race conditions. The testbench timing is configured based on these considerations.

## Requirements

*   A Verilog simulator (e.g., ModelSim, QuestaSim, Vivado Simulator, Icarus Verilog).
*   The source files (`comparator_6bit.v`, `comparator_tb2.v`) and the gate definitions (these are usually included in standard simulation environments, but the delays might need to be defined or the gates inferred by the simulator based on their function).

## Simulation

To simulate the design and run the testbench:

1.  Rename the provided files: `Project+Advance+Comparator.txt` to `comparator_6bit.v` and `TB.txt` to `comparator_tb2.v`.
2.  Open your Verilog simulator.
3.  Compile the Verilog source files (`comparator_6bit.v` and `comparator_tb2.v`).
4.  Start the simulation using the testbench module (`comparator_tb2`).
5.  Observe the output in the simulation console to see the test cases running and the final summary. You can also view the waveforms to analyze signal behavior.

Example commands (using Icarus Verilog):

```bash
iverilog -o comparator.vvp comparator_6bit.v comparator_tb2.v
vvp comparator.vvp
