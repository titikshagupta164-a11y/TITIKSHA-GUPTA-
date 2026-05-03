# SKY130 RTL Design and Synthesis Workshop

## Tools Used

- Yosys – Logic synthesis
- Icarus Verilog – Simulation
- GTKWave – Waveform viewer
- Sky130 Standard Cell Library – Technology mapping
## Design Flow

RTL Design (Verilog)
        ↓
Simulation (Icarus Verilog + GTKWave)
        ↓
Synthesis (Yosys)
        ↓
Technology Mapping (Sky130 Library)
        ↓
Gate-Level Netlist + Visualization

## Key Learnings

- Difference between combinational and sequential circuits
- Importance of sensitivity list (`always @(*)`)
- Role of standard cell libraries in synthesis
- Optimization techniques (e.g., shift instead of multiplication)
- Hierarchical vs flattened design
- Asynchronous vs synchronous reset behavior
---

## 1. MUX Synthesis (good_mux)
Description:
A 2:1 multiplexer is designed and synthesized. Output y selects between i0 and i1 based on sel.
Commands Used:
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd

yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show

![MUX](./MUX.png)

![MUX Waveform](./MUX_WAVE.png)

---

## 2. Hierarchical Design – Multiple Modules
Description:
Design consists of multiple submodules connected hierarchically. Yosys flattens and synthesizes the complete design.
Commands Used:
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show

![Multiple Modules](./MULTIPLE_MODULE.png)

---

## 3. Submodule Synthesis (AND Gate)
Description:
A simple submodule implementing AND logic is synthesized independently.
Commands Used:
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog sub_module.v
synth -top sub_module1
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
![Submodule](./SUB_MODULE.png)

---

## 4. Sequential Circuit – Basic D Flip-Flop

![DFF](./DFF.png)

---

## 5. Sequential Circuit – D Flip-Flop with Asynchronous Reset
Description:
D Flip-Flop resets immediately when reset signal is active, independent of clock.
Commands Used:
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
![DFF Reset](./DFF_RESET.png)

---

## 6. Sequential Circuit – D Flip-Flop with Asynchronous Set
Description:
Reset is applied only on the clock edge, not immediately.

Commands Used:
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show

![DFF Async Set](./DFF_ASYNC_SET.png)

---

## 7. Sequential Circuit – Flip-Flop Implementation
Description:
Basic D Flip-Flop implemented and synthesized using standard cells.
Commands Used:
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff.v
synth -top dff
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show

![Flip Flop](./FLIP_FLOP.png)

---

## 8. Optimization Case – mul2
Description:
Multiplication by 2 is optimized by shifting left (y = a << 1). No multiplier hardware needed.
Commands Used:
yosys
read_verilog mul2.v
synth -top mul2
show
![mul2](./MUL2.png)

---

## 9. Combinational Logic – mult8
Description:
Multiplication by 8 implemented using bit concatenation (y = {a, a}), optimized during synthesis.
Commands Used:
yosys
read_verilog mult8.v
synth -top mult8
show
![mult8](./MULT8.png)

## Observations

- All designs were successfully synthesized using Sky130 library
- Yosys mapped RTL to standard cells like AND, OR, DFF
- Sequential circuits resulted in flip-flop cells
- Optimization reduced hardware complexity in mul2 and mult8

  ## Conclusion

This project demonstrates the complete RTL to gate-level synthesis flow using open-source tools. It provides hands-on understanding of digital design, optimization, and real-world standard cell mapping.

## Future Scope

- Timing analysis
- FPGA implementation
- Power optimization
