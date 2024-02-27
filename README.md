## Environment
Ubunu linux run on a Lenovo laptop
```
$lsb_release -a

Distributor ID:	Ubuntu
Description:	Ubuntu 22.04.3 LTS
Release:	22.04
Codename:	jammy
```

## Steps to run
0. Setup Keystone Enclave env by following: https://gist.github.com/dw2008/a9a67f943f39e770dc2e987ed9d86c61#file-keystoneinstall-md
1. Download the latest code in `my-hello` directory, copy it to keystone example directory.
2. Go to example directory then add following line at the end of the CMakeLists.txt
```
add_subdirectory(my-hello)
```
This will let keystone to build my-hello as an enclave.

3. Go to keystone root directory the build by running:  `make -j$(nproc)`
4. Start QEMU by `make run`
5. Start Keystone by `modprobe keystone-driver`
6. Go to `/usr/share/keystone/examples`
7. Run Enclave `./my-hello`


I am currently have following error even just put hello world in hello1.cpp file. 

Error log when run `my-hello.ke`
```
# ./my-hello.ke 
Verifying archive integrity... MD5 checksums are OK. All good.
Uncompressing Keystone Enclave Package
[   67.436817] ------------[ cut here ]------------
[   67.437271] WARNING: CPU: 3 PID: 204 at mm/page_alloc.c:5535 __alloc_pages+0x1e8/0xad2
[   67.438378] Modules linked in: keystone_driver(O)
[   67.439090] CPU: 3 PID: 204 Comm: hello-runner Tainted: G           O       6.1.32 #2
[   67.439697] Hardware name: riscv-virtio,qemu (DT)
[   67.440142] epc : __alloc_pages+0x1e8/0xad2
[   67.440520]  ra : __dma_direct_alloc_pages.constprop.0+0x1a4/0x23a
[   67.440960] epc : ffffffff80122484 ra : ffffffff80069a72 sp : ff20000010933990
[   67.441470]  gp : ffffffff812ea838 tp : ff6000000298a040 t0 : ff20000010933cb8
[   67.441984]  t1 : 00aaaaaae3317f10 t2 : 00aaaaaae32ff9e0 s0 : ff20000010933af0
[   67.442467]  s1 : 00000000ffffffff a0 : 0000000000000cc4 a1 : 000000000000000b
[   67.442938]  a2 : 0000000000000000 a3 : 0000000000000000 a4 : 0000000000000001
[   67.443418]  a5 : ffffffff812d40f1 a6 : 00000000000271a8 a7 : 00ffffffe9dfea60
[   67.443889]  s2 : 0000000000000000 s3 : 00000000005ab000 s4 : 000000000000000b
[   67.444534]  s5 : 00000000005aafff s6 : 00ffffffe9dfeee8 s7 : 00ffffffe9dfe9a0
[   67.445230]  s8 : 00ffffffe9dfe9d0 s9 : 00aaaaaac2f3e1a0 s10: 00aaaaaac2f3df60
[   67.445718]  s11: 0000000000000000 t3 : 0000000000300000 t4 : 00aaaaaae3317ff0
[   67.446210]  t5 : 0000000000000050 t6 : 0000000000040000
[   67.446578] status: 0000000200000120 badaddr: 0000000000000000 cause: 0000000000000003
[   67.447260] [<ffffffff80122484>] __alloc_pages+0x1e8/0xad2
[   67.447777] [<ffffffff80069a72>] __dma_direct_alloc_pages.constprop.0+0x1a4/0x23a
[   67.448298] [<ffffffff80069be8>] dma_direct_alloc+0x40/0x13e
[   67.448687] [<ffffffff80068fdc>] dma_alloc_attrs+0x70/0x7e
[   67.449290] [<ffffffff013501ca>] epm_init+0xae/0x19a [keystone_driver]
[   67.450159] [<ffffffff013509f8>] create_enclave+0x6a/0xc0 [keystone_driver]
[   67.450640] [<ffffffff0135085a>] keystone_ioctl+0x152/0x1d0 [keystone_driver]
[   67.451125] [<ffffffff80150a10>] sys_ioctl+0x76/0x88
[   67.451500] [<ffffffff80003412>] ret_from_syscall+0x0/0x2
[   67.451977] ---[ end trace 0000000000000000 ]---
[   67.452874] keystone_enclave: failed to allocate 1451 page(s)
[   67.453335] keystone_enclave: failed to initialize epm
[   67.454323] Unable to handle kernel paging request at virtual address 02ddd0007c649190
[   67.455273] Oops [#1]
[   67.455465] Modules linked in: keystone_driver(O)
[   67.455842] CPU: 3 PID: 204 Comm: hello-runner Tainted: G        W  O       6.1.32 #2
[   67.456589] Hardware name: riscv-virtio,qemu (DT)
[   67.457029] epc : __free_pages+0x10/0xc6
[   67.457484]  ra : dma_free_contiguous+0x74/0xa0
[   67.457933] epc : ffffffff80120bd8 ra : ffffffff8006a8a4 sp : ff20000010933b20
[   67.458607]  gp : ffffffff812ea838 tp : ff6000000298a040 t0 : 656e6f747379656b
[   67.459270]  t1 : 000000000000006b t2 : 5f656e6f74737965 s0 : ff20000010933b50
[   67.459827]  s1 : 00000000836fb000 a0 : 02ddd0007c649190 a1 : 0000000000000014
[   67.460510]  a2 : 836fa00000000000 a3 : ff60000000000000 a4 : 0000000000000000
[   67.461220]  a5 : 0000000000000002 a6 : 0000000000000008 a7 : 0000000000000038
[   67.461852]  s2 : 02ddd0007c649190 s3 : 0001000100000010 s4 : 00000000836fb000
[   67.462497]  s5 : 00000000836fb000 s6 : 00ffffffe9dfeee8 s7 : 00ffffffe9dfe9a0
[   67.463158]  s8 : 00ffffffe9dfe9d0 s9 : 00aaaaaac2f3e1a0 s10: 00aaaaaac2f3df60
[   67.463858]  s11: 0000000000000000 t3 : ffffffff812fc4e7 t4 : ffffffff812fc4e7
[   67.464577]  t5 : ffffffff812fc4e8 t6 : ff20000010933a18
[   67.465091] status: 0000000200000120 badaddr: 02ddd0007c649190 cause: 000000000000000d
[   67.465743] [<ffffffff80120bd8>] __free_pages+0x10/0xc6
[   67.466248] [<ffffffff8006a8a4>] dma_free_contiguous+0x74/0xa0
[   67.466768] [<ffffffff80069d5c>] dma_direct_free+0x76/0xc2
[   67.467232] [<ffffffff80069106>] dma_free_attrs+0x84/0xb2
[   67.467750] [<ffffffff013500f8>] epm_destroy+0x2a/0x4e [keystone_driver]
[   67.468416] [<ffffffff0135094c>] destroy_enclave+0x28/0x6a [keystone_driver]
[   67.469050] [<ffffffff01350a38>] create_enclave+0xaa/0xc0 [keystone_driver]
[   67.469704] [<ffffffff0135085a>] keystone_ioctl+0x152/0x1d0 [keystone_driver]
[   67.470363] [<ffffffff80150a10>] sys_ioctl+0x76/0x88
[   67.470772] [<ffffffff80003412>] ret_from_syscall+0x0/0x2
[   67.471571] ---[ end trace 0000000000000000 ]---
./my-hello.ke: line 704:   204 Segmentation fault      "./hello-runner" my-hello hello1 eyrie-rt "$@"

```
