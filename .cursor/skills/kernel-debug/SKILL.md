---
name: kernel-debug
description: "分析内核 oops、panic、lockdep、KASAN、hung task、软锁或驱动超时时使用。用户贴出 dmesg、Call Trace、RIP、寄存器，或要求定位内核崩溃时也要使用。"
---

# 读内核故障

先把日志读完，再提出改哪一行。

1. 记下内核版本、架构、是否打开了 KASAN 或 lockdep，以及触发步骤。对照的源码必须是这棵树，不是最新主线。
2. 从 `Call Trace` 最上面属于本仓库的函数开始。有 vmlinux 时用 `scripts/decode_stacktrace.sh` 或 `faddr2line` 把 RIP 偏移对到行。
3. 先判定类型：空指针、释放后使用、死锁、原子上下文睡眠、超时、硬件无响应。只沿这一类的证据往下看。
4. 锁问题先写清持锁顺序，对照 lockdep 报告里的锁类。不要用关抢占或加延时把报告盖住。
5. 驱动超时先看完成量、中断有没有进来、复位路径能不能重入。日志用文件里已有的 `dev_dbg`、`pr_debug` 或 `trace_printk`。热路径不要留无条件 `printk`。
6. 动态跟踪优先用这棵树里已经定义的 tracepoint。没有这个事件名就说明没有，不要编一个。
7. 修复要说明这条路径为什么不再出错，以及出错时锁和资源是否都放下了。
