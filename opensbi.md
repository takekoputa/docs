- [https://github.com/riscv-software-src/opensbi/issues/137]
> The FW_PAYLOAD takes flat kernel image as payload (not vmlinux ELF). The BBL on other hand takes vmlinux ELF and converts it into flat kernel image before using as payload. In other words, OpenSBI FW_PAYLOAD and BBL deal with payload in same way.
