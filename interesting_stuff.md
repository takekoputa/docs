### Ever wondered why older version of RISC-V gcc always used v24 and v25 first?

This is the change fixing the order of vector registers https://gcc.gnu.org/git/?p=gcc.git;a=blobdiff;f=gcc/config/riscv/riscv.h;h=13038a39e5c2faa23df30090564d41ff991ad292;hp=66fb07d66521843fcab130c7640f483f8a4516ec;hb=7b206ae7f17455b69349767ec48b074db260a2a7;hpb=9fde76a3be8e1717d9d38492c40675e742611e45.
Previously, v24-v31 were higher in the picking order than all other vector registers for some reason.
