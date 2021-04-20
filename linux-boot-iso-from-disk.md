# Boot Linux ISO from disk

You can boot a Linux Live system from ISO which is located on disk. I use Linux Mint (other distro may work in a different way).

* Create s directory on the disk for ISO-s: `mkdir /iso`
* Download the Linux ISO into it.
* Edit **grub** config: `vi /etc/grub.d/40_custom`

```shell
#!/bin/sh
exec tail -n +3 $0
# This file provides an easy way to add custom menu entries.  Simply type the
# menu entries you want to add after this comment.  Be careful not to change
# the 'exec tail' line above.
iso=/iso/systemrescue-8.02-amd64.iso
menuentry "SystemRescue (isoloop)" {
    load_video
    insmod gzio
    insmod part_gpt
    insmod part_msdos
    insmod ext2
    #search --no-floppy --label boot --set=root
    search --no-floppy --fs-uuid --set=root 43c15e75-b71b-45c4-a8e0-8c8d762fb63a
    loopback loop $iso
    echo   'Loading kernel ...'
    linux  (loop)/sysresccd/boot/x86_64/vmlinuz img_label=root img_loop=$iso archisobasedir=sysresccd copytoram setkmap=hu
    echo   'Loading initramfs ...'
    initrd (loop)/sysresccd/boot/x86_64/sysresccd.img
}

menuentry 'Kali Linux Lve (isoloop)' --class os --class gnu-linux --class gnu --class os --group group_main {
                set isofile="/iso/kali-linux-2021.1-live-amd64.iso"
         load_video
         insmod ext2
         insmod loopback
         insmod iso9660      
                loopback loop (hd0,gpt2)$isofile      
                search --no-floppy --fs-uuid --set=root 43c15e75-b71b-45c4-a8e0-8c8d762fb63a
                linux (loop)/live/vmlinuz boot=live fromiso=/dev/nvme0n1p2/$isofile noconfig=sudo username=root hostname=kalilinux
                initrd (loop)/live/initrd.img
}

```

* Modify the file: `/etc/default/grub` to look like this:

```conf
GRUB_DEFAULT=0
GRUB_TIMEOUT_STYLE=menu
GRUB_TIMEOUT=3
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""
```

* Run **grub**: `update-grub2`
