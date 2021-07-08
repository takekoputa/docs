## Compiling tools
### Compiling the riscv toolchain
Docs: [https://github.com/riscv/riscv-gnu-toolchain]
```sh
git clone https://github.com/riscv/riscv-gnu-toolchain --recursive -j 10
cd riscv-gnu-toolchain
./configure --prefix=$HOME/opt/riscv
make linux -j 100

# adding riscv binaries to path
echo "export PATH=$PATH:$HOME/opt/riscv/bin" > ~/.bashrc
```
### Compiling qemu
Docs: [https://github.com/qemu/qemu]
Docs: [https://wiki.qemu.org/Documentation/Platforms/RISCV]
```sh
sudo apt-get install ninja-build
git clone https://github.com/qemu/qemu
cd qemu
 ./configure --target-list=riscv64-softmmu
 make -j 100
```

### Getting bios and bootloader for booting Ubuntu in qemu
Docs: [https://wiki.ubuntu.com/RISC-V]
The doc says:
> Prerequisites:  
>    apt install qemu-system-misc opensbi u-boot-qemu qemu-utils  
> Hirsute's version of u-boot-qemu is required at the moment to boot hirsute images.  

If the host machine is not Ubuntu 21.04 (Hirsute Hippo), the following steps will install the above packages
using Hirsute's version.  

Note: the "Hirsute's version" can be obtained here: [https://packages.ubuntu.com/hirsute/u-boot-qemu]  

```sh
wget http://mirrors.kernel.org/ubuntu/pool/main/u/u-boot/u-boot-qemu_2021.01+dfsg-3ubuntu9_all.deb
sudo dpkg -i u-boot-qemu_2021.01+dfsg-3ubuntu9_all.deb
sudo apt-get install -f
```
The files of interest are:
  * `/usr/lib/riscv64-linux-gnu/opensbi/generic/fw_jump.elf`
  * `/usr/lib/u-boot/qemu-riscv64_smode/uboot.elf`

## Disk image stuff
### Obtaining the disk image
Docs: [https://wiki.ubuntu.com/RISC-V]
```sh
wget https://cdimage.ubuntu.com/releases/21.04/release/ubuntu-21.04-preinstalled-server-riscv64+unmatched.img.xz    # downloading the disk image
xz -dk ubuntu-21.04-preinstalled-server-riscv64+unmatched.img.xz                                                    # unpacking/decompressing the disk image
mv ubuntu-21.04-preinstalled-server-riscv64+unmatched.img ubuntu.img                                                # renaming the disk image
qemu-img resize -f raw ubuntu.img +30G                                                                              # adding 30GB to the disk
```
We'll have to boot the disk image to resize `/dev/vda` later.

## Booting Ubuntu with QEMU
Docs: [https://wiki.ubuntu.com/RISC-V]
### Normal booting
```
./qemu/build/qemu-system-riscv64 -machine virt -nographic \
     -m 16384 -smp 8 \
     -bios /usr/lib/riscv64-linux-gnu/opensbi/generic/fw_jump.elf \
     -kernel /usr/lib/u-boot/qemu-riscv64_smode/uboot.elf \
     -device virtio-net-device,netdev=eth0 \
     -netdev user,id=eth0,hostfwd=tcp::5555-:22 \
     -drive file=ubuntu.img,format=raw,if=virtio 
```
The above command will forward the port 22 of guest to port 5555 of host.  
After running the ssh server, one can ssh to the guest by,
```sh
ssh ubuntu@localhost -p 5555
```
Launching the ssh server,
```sh
/etc/init.d/ssh start
```
### First time booting
```
Username: ubuntu
Password: ubuntu
```
#### Stop cloud-init, install tools
Docs: [https://askubuntu.com/questions/539277/how-to-get-rid-of-cloud-init]
```sh
sudo touch /etc/cloud/cloud-init.disabled # stop cloud-init
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential gparted
```
#### Resize the data partition
```sh
resize2fs /dev/vda1
```
