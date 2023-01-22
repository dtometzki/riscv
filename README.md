```
make ARCH=riscv CROSS_COMPILE=riscv64-linux-gnu- -j8
cd ../Tools/spl_tool/
./create_sbl /home/admin/riscv/u-boot/spl/u-boot-spl.bin 0x01010101
cd /home/admin/opensbi
make PLATFORM=generic FW_PAYLOAD_PATH=/home/admin/riscv/u-boot/u-boot.bin FW_FDT_PATH=/home/admin/riscv/u-boot/arch/riscv/dts/starfive_visionfive2.dtb FW_TEXT_START=0x40000000
cd ../Tools/uboot_its/
cp /home/admin/riscv/opensbi/build/platform/generic/firmware/fw_payload.bin ./
/home/admin/riscv/u-boot/tools/mkimage -f visionfive2-uboot-fit-image.its -A riscv -O u-boot -T firmware visionfive2_fw_payload.img
cp visionfive2_fw_payload.img /home/admin/riscv/starfive
cd ../spl_tool/
cp u-boot-spl.bin.normal.out /home/admin/riscv/starfive
```
# Some u-boot commands
```
fatload mmc 1:2 0xb0000000 /uEnv.txt
env import 0xb0000000 17c
fatload mmc 1:2 0x46000000 /dtbs/starfive/jh7110-visionfive-v2.dtb 
fdt addr 0x46000000
fatsize mmc 1:2 /dtbs/starfive/jh7110-visionfive-v2.dtb
sysboot mmc 1:2 fat c0000000 extlinux/extlinux.conf;

env set bootargs root=/dev/mmcblk1p3 root=/dev/mmcblk1p3 rw console=tty0 console=ttyS0,115200 earlycon rootwait
mmc rescan
load mmc 1:2 0xb0000000 /vmlinuz-5.15.0-starfive
load mmc 1:2 0x46000000 /dtbs/starfive/jh7110-visionfive-v2.dtb
booti 0xb0000000 - 0x46000000
sysboot mmc 1:2 fat c0000000 /extlinux/extlinux.conf
printenv loadaddr
printenv fdtaddr
load mmc 1:2 0x40200000 /vmlinuz-5.15.0-starfive
"run load_vf2_env;run importbootenv;run load_distro_uenv;run boot2;run distro_bootcmd"


load mmc 1:2 a0000000 uEnv.txt 
env import a0000000 17c
# setenv fdtfile dtbs/starfive/jh7110-visionfive-v2.dtb 
sysboot mmc 1:2 any b0000000 /extlinux/extlinux.conf
```
