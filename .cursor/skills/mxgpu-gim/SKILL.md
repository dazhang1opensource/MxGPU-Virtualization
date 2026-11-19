---
name: mxgpu-gim
description: "修改 AMD MxGPU 的 GIM 内核模块、SR-IOV、world switch、FLR 或 PF/VF 握手时使用。用户提到 gim.ko、S7150、GPU IOV、虚拟功能或 MxGPU 时也要使用。"
---

# GIM（MxGPU 主机驱动）

GIM 是树外内核模块，源码在 `drv/`，只支持 AMD S7150。它负责 GPU IOV 初始化、VF 配置、world switch 调度、挂死检测、VF 级 FLR，以及 PF 和 VF 的握手。已测主机是 Ubuntu 16.04 的 4.4 内核，KVM 和 Xen 都要能编过。

- 风格跟 `drv/` 里现有文件走：内核 tab 缩进，错误处理和日志宏用文件里已经在用的那套。
- 配置写在 `drv/Makefile` 的 `KBUILD_CFLAGS`，例如 `CONFIG_GIM_HEARTBEAT_TIMER`。不要做成上游 Kconfig。
- 寄存器以 `drv/asic_reg/` 和 `gim_s7150_reg.h` 里已有的定义为准。不要编造 S7150 的寄存器偏移。
- world switch、调度定时器和 FLR 会互相打断。改其中一条时，写明另外两条在复位过程中看到的状态。
- 内核兼容垫层在 `gim_kcl_*`。新的内核 API 先放进 kcl，业务代码不要直接堆版本宏。
- `gim_vbios_patch.h` 是大块 VBIOS 数据。没有明确的字节级修改要求就不要动它。
- 在 `drv/` 下 `make` 生成 `gim.ko`，或用根目录的 `gim.sh`。安装位置是 `/lib/modules/$(KERNELRELEASE)/GIM/`。
- `patch/` 里是旧内核缺 SR-IOV 行为时的主机补丁。改它时写清对应的内核版本。
