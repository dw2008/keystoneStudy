set(eapp_bin simple-vgg16)
set(eapp_src eapp/simple-vgg16/hello.c)
set(host_bin simple-vgg16-runner)
set(host_src host/host.cpp)
set(package_name "simple-vgg16.ke")
set(package_script "./simple-vgg16-runner simple-vgg16 eyrie-rt loader.bin")
set(eyrie_plugins "io_syscall linux_syscall env_setup")

# eapp

add_executable(${eapp_bin} ${eapp_src})
target_link_libraries(${eapp_bin} "-static")

# host

add_executable(${host_bin} ${host_src})
target_link_libraries(${host_bin} ${KEYSTONE_LIB_HOST} ${KEYSTONE_LIB_EDGE})

# add target for Eyrie runtime (see keystone.cmake)

set(eyrie_files_to_copy .options_log eyrie-rt loader.bin)
add_eyrie_runtime(${eapp_bin}-eyrie
  ${eyrie_plugins}
  ${eyrie_files_to_copy})

# add target for packaging (see keystone.cmake)

add_keystone_package(${eapp_bin}-package
  ${package_name}
  ${package_script}
  ${eyrie_files_to_copy} ${eapp_bin} ${host_bin})

add_dependencies(${eapp_bin}-package ${eapp_bin}-eyrie)

# add package to the top-level target
add_dependencies(examples ${eapp_bin}-package)
[    0.193173] nfs4flexfilelayout_init: NFSv4 Flexfile Layout Driver Registering...
[    0.193830] 9p: Installing v9fs 9p2000 file system support
[    0.195701] NET: Registered PF_ALG protocol family
[    0.196037] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 253)
[    0.196164] io scheduler mq-deadline registered
[    0.196234] io scheduler kyber registered
[    0.199736] pci-host-generic 30000000.pci: host bridge /soc/pci@30000000 ranges:
[    0.200402] pci-host-generic 30000000.pci:       IO 0x0003000000..0x000300ffff -> 0x0000000000
[    0.200801] pci-host-generic 30000000.pci:      MEM 0x0040000000..0x007fffffff -> 0x0040000000
[    0.200861] pci-host-generic 30000000.pci:      MEM 0x0400000000..0x07ffffffff -> 0x0400000000
[    0.201300] pci-host-generic 30000000.pci: Memory resource size exceeds max for 32 bits
[    0.202270] pci-host-generic 30000000.pci: ECAM at [mem 0x30000000-0x3fffffff] for [bus 00-ff]
[    0.203338] pci-host-generic 30000000.pci: PCI host bridge to bus 0000:00
[    0.203563] pci_bus 0000:00: root bus resource [bus 00-ff]
[    0.203639] pci_bus 0000:00: root bus resource [io  0x0000-0xffff]
[    0.203689] pci_bus 0000:00: root bus resource [mem 0x40000000-0x7fffffff]
[    0.203701] pci_bus 0000:00: root bus resource [mem 0x400000000-0x7ffffffff]
[    0.204686] pci 0000:00:00.0: [1b36:0008] type 00 class 0x060000
[    0.207411] pci 0000:00:01.0: [1af4:1005] type 00 class 0x00ff00
[    0.207777] pci 0000:00:01.0: reg 0x10: [io  0x0000-0x001f]
[    0.207853] pci 0000:00:01.0: reg 0x14: [mem 0x00000000-0x00000fff]
[    0.207963] pci 0000:00:01.0: reg 0x20: [mem 0x00000000-0x00003fff 64bit pref]
[    0.210027] pci 0000:00:01.0: BAR 4: assigned [mem 0x400000000-0x400003fff 64bit pref]
[    0.210227] pci 0000:00:01.0: BAR 1: assigned [mem 0x40000000-0x40000fff]
[    0.210268] pci 0000:00:01.0: BAR 0: assigned [io  0x0020-0x003f]
[    0.215803] virtio-pci 0000:00:01.0: enabling device (0000 -> 0003)
[    0.358051] Serial: 8250/16550 driver, 4 ports, IRQ sharing disabled
[    0.366335] printk: console [ttyS0] disabled
[    0.367211] 10000000.serial: ttyS0 at MMIO 0x10000000 (irq = 2, base_baud = 230400) is a 16550A
[    0.409932] printk: console [ttyS0] enabled
[    0.417322] [drm] radeon kernel modesetting enabled.
[    0.434428] loop: module loaded
[    0.435368] virtio_blk virtio0: 1