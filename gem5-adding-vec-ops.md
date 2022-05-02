- Adding new registers that are not defined by the ISA (i.e. temporary registers for data dependencies among micro-ops of a macro-op)
  - Adding temporary register definitions to `src/arch/riscv/regs/int.hh`.

- Adding new operands (i.e. adding new vector operands, e.g. vd, vs1, vs2),
  - `src/arch/riscv/operands.isa`: add the new operand in `def operands`.
    - The types of registers, such as `IntRegOp` or `FloatRegOp`, are defined in `src/arch/isa_parser/isa_parser.py`.
      For example, `IntRegOp` will be mapped to `IntRegOperandDesc`, which is defined in `src/arch/isa_parser/operand_types.py`.
      We need the constructor signature to create the new operands.

- Adding vector registers as defined by the ISA,
  - `src/arch/riscv/vecregs.hh`: add an include to `arch/riscv/regs/vec.hh`.
  - For content of `src/arch/riscv/regs/vec.hh`, look at other ISAs `vec.hh`.
  - Add the `VTYPE` register to `src/arch/riscv/regs/misc.hh`.
  - Add CSR registers to `src/arch/riscv/regs/misc.hh`.
  - Add misc reg names to `src/arch/riscv/isa.cc`.

- Adding bitfields to `src/arch/riscv/isa/bitfields.isa`.

- Adding `src/arch/riscv/isa/formats/vector.isa`: where instruction constructors at.

- Adding `src/arch/riscv/insts/vector.cc` and `src/arch/riscv/insts/vector.hh`: I don't know what they are for yet.

- Updating `src/arch/riscv/isa/decoder.isa` to decode vector insts.

- `src/arch/riscv/insts/SConscript`: add `vector.cc`.
- `src/arch/riscv/isa/formats/formats.isa`: add `vector.isa`.

- Should be fixed stuff,
  - Fixing VType in decoder
    - In `src/arch/isa_parser/isa_parser.py`, the function `p_top_level_decode_block` modified decodeInst() signature to add vtype and vl.
    - Similar things in `src/arch/riscv/decoder.cc` and `src/arch/riscv/decoder.hh`.
    - In `src/arch/generic/pcstate.hh`, this contains `vl` and `vtype`.
    - In `src/cpu/simple/base.cc`, `vl` and `vtype` are manually read from MiscReg of the execution thread.
