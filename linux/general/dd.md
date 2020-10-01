---
layout: default
title: Make bootable USB drive from ISO file
parent: General
grand_parent: Linux
nav_order: 1
---

# Make bootable USB drive from ISO file using `dd` command

* Find out the name of your USB drive with `lsblk`. Make sure that it is **not** mounted.
* Run the following command, replacing <code>/dev/<b>sdx</b></code> with your drive, e.g. `/dev/sdb`. (Do **not** append a partition number, so do **not** use something like <code>/dev/sdb<b>1</b></code>):
  * As root user:
      ```
      # dd bs=4M if=path/to/archlinux.iso of=/dev/sdx status=progress oflag=sync
      ```
  * Using `sudo`:
      ```
      $ sudo dd bs=4M if=path/to/archlinux.iso of=/dev/sdx status=progress oflag=sync
      ```
 
 Source: [ArchWiki](https://wiki.archlinux.org/index.php/USB_flash_installation_medium#Using_basic_command_line_utilities)
