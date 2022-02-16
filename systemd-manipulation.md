[ArchLinux guide](https://wiki.archlinux.org/title/Improving_performance/Boot_process)

Finding Critical Chain: `systemd-analyze critical-chain`

- [https://serverfault.com/questions/784303/how-to-disable-auto-fsck-in-centos-7]
- [https://unix.stackexchange.com/questions/413656/keyboard-setup-service-is-slow-at-boot-do-i-need-it]
> setting KEYMAP=y in /etc/initramfs-tools/initramfs.conf and running update-initramfs -u and seeing if it makes things any faster by loading the keymap in initramfs and causing the keyboard-setup.service to be essentially bypassed

```
sudo systemctl disable networkd-dispatcher.service
sudo systemctl disable systemd-networkd.service
```
