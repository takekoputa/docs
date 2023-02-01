Refs,
- https://discourse.ubuntu.com/t/ubuntu-installation-on-a-risc-v-virtual-machine-using-a-server-install-image-and-qemu/27636
- https://cloudinit.readthedocs.io/en/latest/reference/examples.html

Command,

```sh
/usr/local/bin/qemu-system-riscv64 \
  -machine virt -smp 8 -m 8G -nographic \
  -bios /usr/lib/riscv64-linux-gnu/opensbi/generic/fw_jump.bin \
  -kernel /usr/lib/u-boot/qemu-riscv64_smode/u-boot.bin \
  -device virtio-net-device,netdev=net0 -name vm_name_here \
  -netdev user,id=net0,hostfwd=tcp::2598-:22 \
  -drive file=ubuntu-22.04.1-live-server-riscv64.img,format=raw,if=virtio \
  -drive file=ubuntu-riscv64/vm_name_here,if=virtio,cache=writeback,discard=ignore,format=raw \
  -device virtio-rng-pci
```
