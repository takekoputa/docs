### Docs

- Bringing up QEMU: [https://wiki.ubuntu.com/ARM64/QEMU](https://wiki.ubuntu.com/ARM64/QEMU)
- Creating a cloud image: [https://gist.github.com/oznu/ac9efae7c24fd1f37f1d933254587aa4](https://gist.github.com/oznu/ac9efae7c24fd1f37f1d933254587aa4)
- Playing around with cloud-init: [https://cloudinit.readthedocs.io/en/latest/topics/examples.html](https://cloudinit.readthedocs.io/en/latest/topics/examples.html)

### Cloudimg base

[http://cloud-images.ubuntu.com/focal/current/](http://cloud-images.ubuntu.com/focal/current/)

### Installing cloud utils (for booting cloud disk image)

```sh
sudo apt-get install cloud-utils
```

### Creating a cloud config file

Creating a `cloud.txt` file with the following content,
```
#cloud-config
users:
  - name: ubuntu
    ssh-authorized-keys:
      - ssh-rsa AAABBBCCCASDWOIQUOUOIUDOSA... <- this key will be used for ssh-ing
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    groups: sudo
    shell: /bin/bash
```



### Bringing up QEMU

```sh
sudo apt-get install qemu-system-arm qemu-efi
dd if=/dev/zero of=flash0.img bs=1M count=64
dd if=/usr/share/qemu-efi/QEMU_EFI.fd of=flash0.img conv=notrunc
dd if=/dev/zero of=flash1.img bs=1M count=64
wget 
qemu-system-aarch64 -m 4096 -smp 2 -cpu cortex-a57 -M virt -nographic -pflash flash0.img -pflash flash1.img \
                    -drive if=none,file=focal-server-cloudimg-arm64.img,id=hd0 -device virtio-blk-device,drive=hd0 \
                    -drive if=none,id=cloud,file=cloud.img,format=raw -device virtio-blk-device,drive=cloud \
                    -netdev user,id=user0 -device virtio-net-device,netdev=eth0 \
                    -netdev user,id=eth0,hostfwd=tcp::5556-:22
```

### Modifying the disk image

Login to guest,
```sh
ssh -p 5556 ubuntu@localhost
```

On guest,
```sh
sudo touch /etc/cloud/cloud-init.disabled
```
