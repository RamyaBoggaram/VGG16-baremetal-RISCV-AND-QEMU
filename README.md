# VGG16-baremetal-RISCV-AND-QEMU
These are the steps to build vgg16 in baremetal RISCV and QEMU configurations.
# 1. Copy the code
Paste all the files in the directory.
```
cd directory_name
```
# 2. Build baremetal binary
```
/opt/riscv/bin/riscv64-unknown-linux-gnu-gcc tinyNN-predict.c -o tinyNN-predict-riscv
```
# 3. Start riscv-ubuntu live server
```
/usr/bin/qemu-system-riscv64 -machine virt -m 4G -smp cpus=2 -nographic     -bios /usr/lib/riscv64-linux-gnu/opensbi/generic/fw_jump.bin     -kernel /usr/lib/u-boot/qemu-riscv64_smode/u-boot.bin     -netdev user,id=net0,hostfwd=tcp::5555-:22     -device virtio-net-device,netdev=net0     -drive file=disk,format=raw,if=virtio     -device virtio-rng-pci
```
# 4. Start sifive server:
```
qemu-system-riscv64 -machine virt -nographic -m 2048 -smp 4 -bios /usr/lib/riscv64-linux-gnu/opensbi/generic/fw_jump.bin -kernel /usr/lib/u-boot/qemu-riscv64_smode/uboot.elf -netdev user,id=net0,hostfwd=tcp::5555-:22 -device virtio-net-device,netdev=net0 -device virtio-rng-pci -drive file=img/ubuntu-22.04.4-preinstalled-server-riscv64+unmatched.img,format=raw,if=virtio
```
# 5. Upload binary
```
scp  -P 5555 ./tinyNN-predict-riscv ubuntu@localhost:~/
```
# 6. Inside the server after uploading the binary run the binary using
```
./tinyNN-predict-riscv
```
