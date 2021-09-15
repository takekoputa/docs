## Based on:
  - https://gem5.googlesource.com/public/gem5-resources/
  - https://github.com/firesim/FireMarshal/tree/master/wlutil/initramfs

## Creating working folder and getting resources
```sh
mkdir riscv-disk
mkdir riscv-disk/initdir # the cpio of this folder + devNodes.cpio will be initramfs
wget https://raw.githubusercontent.com/firesim/FireMarshal/master/wlutil/initramfs/makeDevNodes.sh
chmod +x makeDevNodes.sh
./makeDevNodes.sh # this command will create the devNodes.cpio file
```
**Note:** to check the content of a cpio file, you can use `lsinitramfs` or `binwalk`.

## Compile gem5 m5
```sh
git clone https://gem5.googlesource.com/public/gem5
cd gem5/util/m5
scons build/riscv/out/m5
```
Note: the default cross-compiler is `riscv64-unknown-linux-gnu-`. To change the cross-compiler, you can set the cross-compiler using the scons sticky variable `riscv.CROSS_COMPILE`. For example,
```sh
scons riscv.CROSS_COMPILE=riscv64-linux-gnu- build/riscv/out/m5
```

## Compiling RISCV Toolchain
```sh
git clone https://github.com/riscv/riscv-gnu-toolchain
cd riscv-gnu-toolchain
./configure --prefix=$HOME/opt/riscv
make linux -j$(nproc)
export PATH=$PATH:$HOME/opt/riscv/bin
```

## UCanLinux
This contains a working sample of Linux Kernel configuration and a working sample of Busybox configuration.
```sh
cd riscv-disk
git clone https://github.com/UCanLinux/riscv64-sample
```

## Busybox
```sh
cd riscv-disk
git clone git://busybox.net/busybox.git
cd busybox
git checkout 1_32_stable
cp ../riscv64-sample/busybox.config .config
make CROSS_COMPILE=riscv64-linux-gnu- all -j$(nproc)
make CROSS_COMPILE=riscv64-linux-gnu- install
```

## Copying content to `initdir`
```sh
cd riscv-disk/initdir
cp -a ../riscv64-sample/skeleton/* .
cp -a ../busybox/_install/* .
cd ../linux
make ARCH=riscv CROSS_COMPILE=riscv64-linux-gnu- INSTALL_MOD_PATH=../initdir modules_install
cd -
cp -a $HOME/opt/riscv/sysroot/lib .
mkdir dev home mnt proc sys tmp var
cd etc/network
mkdir if-down.d  if-post-down.d  if-pre-up.d  if-up.d
```

## Compiling Linux Kernel (1st time)
```sh
cd riscv-disk
git clone --depth 1 --branch v5.10 https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git
cd linux
cp ../riscv64-sample/kernel.config .config
make ARCH=riscv CROSS_COMPILE=riscv64-linux-gnu- menuconfig
# Go to "General setup"
#  -> check on "Initial RAM filesystem and RAM disk (initramfs/initrd) support"
#  -> change "Initramfs source file(s)" to "$HOME/"
make ARCH=riscv CROSS_COMPILE=riscv64-linux-gnu- all -j$(nproc)
```

