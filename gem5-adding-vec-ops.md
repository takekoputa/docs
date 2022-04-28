- Adding new registers that are not defined by the ISA (i.e. temporary registers for data dependencies among micro-ops of a macro-op)
- Adding new operands (i.e. adding new vector operands, e.g. vd, vs1, vs2),
  - `src/arch/riscv/operands.isa`: add the new operand in `def operands`.
    - The types of registers, such as `IntRegOp` or `FloatRegOp`, are defined in `src/arch/isa_parser/isa_parser.py`.
      For example, `IntRegOp` will be mapped to `IntRegOperandDesc`, which is defined in `src/arch/isa_parser/operand_types.py`.
      We need the constructor signature to create the new operands.
