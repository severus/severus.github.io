---
layout: default
title: Error runing Docker on Fedora 31
parent: Fedora
grand_parent: Linux
nav_order: 1
---

# Error runing Docker on Fedora 31

Fedora 31 is the first major Linux dustribution that comes with cgroup v2 enabled by default. However, Docker still do not support cgroup v2.
{: .fs-6 .fw-300 }

To start Docker on Fedora 31 run the following command and reboot:

```
$ sudo dnf install -y grubby && \
  sudo grubby \
  --update-kernel=ALL \
  --args="systemd.unified_cgroup_hierarchy=0"
```

This command reverts the systemd configuration to use cgroup v1.
