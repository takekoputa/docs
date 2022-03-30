### Expanding a disk image

```
[bash] qemu-img resize <image_name> +30G
[bash] parted /dev/vda
[parted] print all # getting the new end block
[parted] resizepart 1 # resizing /dev/vda1
[bash] resize2fs /dev/vda1
```

### Remove old linux kernels

https://unix.stackexchange.com/questions/605806/can-i-remove-all-the-recent-kernel-versions-at-lib-modules
