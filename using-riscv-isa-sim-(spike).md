# Compilation

We will need to compile,
- `riscv-gnu-toolchain` with newlib (i.e., we are targeting `riscv64-unknown-elf`).
- `riscv-pk` using `riscv64-unknown-elf-gcc`.
I.e., the `../configure` step must be called correctly, or we can modify the `Makefile`
file to use the correct compiler after calling `../configure`.
`riscv-pk` is obviously the proxy kernel used in the `riscv-isa-sim` example command.
- `riscv-isa-sim` using the host compiler.

Note: when to use which compiler,
- (https://riscv.org/wp-content/uploads/2015/01/riscv-software-stack-bootcamp-jan2015.pdf)
- (https://www.cl.cam.ac.uk/~jrrk2/docs/riscv_compile/)

# Running spike

Typically,

```sh
build/spike ../riscv-pk/build/pk ../dinocpu/src/test/resources/risc-v/lw
```

This will result in an error where spike executes an illegal instruction.
This is because the binary is designed to be run only for the first few instructions,
and it doesn't actually have an end point.
So, we will want to run the binary for a certain number of instructions.

We can indeed tell spike to run N instructions in the interactive debugging mode,

```sh
build/spike -d ../riscv-pk/build/pk ../dinocpu/src/test/resources/risc-v/lw
```

There is a problem: spike, initially, will run a lot instructions to execute
the bbl bootloader.
However, we know that the `lw` binary will be placed at `0x0`, while the bootloader
is placed at `0x1000` during the simulation (the first PC is at `0x1000`).
So, we can tell spike interactive debugging tool to run until `PC == 0`. I.e.,

```sh
(spike) until pc 0 0
```

This means we are telling spike to keep running until the `PC` of hart 0 equals to 0.
After this, we can run the simulation instruction-by-instruction via,

```sh
(spike) run 1
```

# Some Important Functions/Structs

| Functions / Structs | Descriptions |
|-|-|
|`sim_t::interactive_reg()`| This function prints integer register content at the end of simulation |
|`struct state_t` | Contain the architectural state |
|`struct regfile_t` | Contain a register file; there are separate register files for integer registers, floating point registers, etc. |

# Instructions Implementation

Follow this file to keep track of all instructions and header files, [https://github.com/riscv-software-src/riscv-isa-sim/blob/master/riscv/riscv.mk.in](https://github.com/riscv-software-src/riscv-isa-sim/blob/master/riscv/riscv.mk.in).
