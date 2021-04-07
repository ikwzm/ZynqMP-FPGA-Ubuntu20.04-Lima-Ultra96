Build Linux Kernel 
====================================================================================

There are two ways

1. run scripts/build-linux-xlnx-v2020.2-zynqmp-fpga-lima.sh (easy)
2. run this chapter step-by-step (annoying)

## Download ATWILC3000 Linux Driver for Ultra96-V2

```console
shell$ git clone https://github.com/Avnet/u96v2-wilc-driver.git
```
## Download Linux Kernel Source

### Clone from linux-xlnx.git

```console
shell$ git clone --depth 1 -b xilinx-v2020.2 https://github.com/Xilinx/linux-xlnx.git linux-xlnx-v2020.2-zynqmp-fpga-lima
```

### Make Branch linux-xlnx-v2020.2-zynqmp-fpga

```console
shell$ cd linux-xlnx-v2020.2-zynqmp-fpga-lima
shell$ git checkout -b linux-xlnx-v2020.2-zynqmp-fpga-lima refs/tags/xilinx-v2020.2
```

## Patch for linux-xlnx-v2020.2-zynqmp-fpga

```console
shell$ patch -p1 < ../files/linux-xlnx-v2020.2-zynqmp-fpga.diff
shell$ git add --update
shell$ git add arch/arm64/boot/dts/xilinx/zynqmp-uz3eg-iocc.dts
shell$ git commit -m "[patch] for linux-xlnx-v2020.2-zynqmp-fpga."
```

## Patch for linux-xlnx-v2020.2-builddeb

```console
shell$ patch -p1 < ../files/linux-xlnx-v2020.2-builddeb.diff
shell$ git add --update
shell$ git commit -m "[update] scripts/package/builddeb to add tools/include and postinst script to header package."
```

## Add ATWILC3000 Linux Driver for Ultra96-V2

```console
shell$ cp -r ../u96v2-wilc-driver/wilc drivers/staging/wilc3000/
shell$ patch -p1 < ../files/linux-xlnx-v2020.2-zynqmp-fpga-wilc3000.diff
shell$ git add --update
shell$ git add drivers/staging/wilc3000
shell$ git commit -m "[add] drivers/staging/wilc3000"
```

## Patch for Ultra96-V2

```console
shell$ patch -p1 < ../files/linux-xlnx-v2020.2-zynqmp-fpga-ultra96v2.diff
shell$ git add --update
shell$ git add arch/arm64/boot/dts/xilinx/avnet-ultra96v2-rev1.dts 
shell$ git commit -m "[add] devicetree for Ultra96-V2."
```

## Patch for SMB3 and CIFS

```console
shell$ patch -p1 < ../files/linux-xlnx-v2020.2-zynqmp-fpga-cifs.diff
shell$ git add --update
shell$ git commit -m "[add] SMB3 and CIFS."
```

## Patch for Xilinx APF Driver

```console
shell$ patch -p1 < ../files/linux-xlnx-v2020.2-zynqmp-fpga-apf.diff
shell$ git add --update
shell$ git commit -m "[add] Xilinx APF driver."
```

## Patch for Lima Driver

```console
shell$ patch -p1 < ../files/linux-xlnx-v2020.2-zynqmp-fpga-lima.diff
shell$ git add --update
shell$ git commit -m "[add] Lima driver."
```

## Patch for Xilinx DRM KMS Driver for Lima

```console
shell$ patch -p1 < ../files/linux-xlnx-v2020.2-zynqmp-fpga-xlnx_gem_alignment_size.diff
shell$ git add --update
shell$ git commit -m "[add] module_param(gem_alignment_size) to Xilinx DRM KMS Driver for Lima."
```

## Create tag and .version

```console
shell$ git tag -a xilinx-v2020.2-zynqmp-fpga-lima-1 -m "release xilinx-v2020.2-zynqmp-fpga-lima-1"
shell$ echo 1 > .version
```

## Setup for Build 

```console
shell$ cd linux-xlnx-v2020.2-zynqmp-fpga-lima
shell$ export ARCH=arm64
shell$ export CROSS_COMPILE=aarch64-linux-gnu-
shell$ make xilinx_zynqmp_defconfig
```

## Build Linux Kernel and device tree

```console
shell$ export DTC_FLAGS=--symbols
shell$ make deb-pkg
```

## Copy linux kernel image

### Ultra96

```console
shell$ cp arch/arm64/boot/Image ../target/Ultra96/boot/image-5.4.0-xlnx-v2020.2-zynqmp-fpga-lima
shell$ cp arch/arm64/boot/dts/xilinx/avnet-ultra96-rev1.dtb ../target/Ultra96/boot/devicetree-5.4.0-xlnx-v2020.2-zynqmp-fpga-lima-ultra96.dtb
shell$ ./scripts/dtc/dtc -I dtb -O dts --symbols -o ../target/Ultra96/boot/devicetree-5.4.0-xlnx-v2020.2-zynqmp-fpga-lima-ultra96.dts ../target/Ultra96/boot/devicetree-5.4.0-xlnx-v2020.2-zynqmp-fpga-lima-ultra96.dtb
```

### Ultra96-V2

```console
shell$ cp arch/arm64/boot/Image ../target/Ultra96-V2/boot/image-5.4.0-xlnx-v2020.2-zynqmp-fpga-lima
shell$ cp arch/arm64/boot/dts/xilinx/avnet-ultra96v2-rev1.dtb ../target/Ultra96-V2/boot/devicetree-5.4.0-xlnx-v2020.2-zynqmp-fpga-lima-ultra96v2.dtb
shell$ ./scripts/dtc/dtc -I dtb -O dts --symbols -o ../target/Ultra96-V2/boot/devicetree-5.4.0-xlnx-v2020.2-zynqmp-fpga-lima-ultra96v2.dts ../target/Ultra96-V2/boot/devicetree-5.4.0-xlnx-v2020.2-zynqmp-fpga-lima-ultra96v2.dtb
```

