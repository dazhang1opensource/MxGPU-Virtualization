---
name: lore-research
description: "查 Linux、KVM、QEMU、Xen、DRM 或 NVDIMM 的邮件列表结论时使用。用户提到 lore、补丁讨论、maintainer 意见、邮件线程或某次提交的背景时也要使用。"
---

# 查 lore.kernel.org

项目配置了 MCP 服务器 `kernel-lore`，数据来自 https://lore.kernel.org 。先搜到邮件再下结论，不要引用一封没有 URL 的邮件。

默认列表在 `.cursor/mcp.json` 的 `LORE_SCOPE`。要查别的列表时，把列表名传给工具的 scope。常用列表：

| 主题 | 列表 |
| --- | --- |
| 内核通用 | linux-kernel |
| KVM | kvm |
| QEMU | qemu-devel |
| Xen | xen-devel |
| DRM | dri-devel |
| AMD GPU | amd-gfx |
| 持久内存 | linux-nvdimm |

搜索语法是 public-inbox 的：`s:` 主题，`b:` 正文，`f:` 发件人，`dfn:` 补丁里的文件名。先用 `search_lore` 拿列表，再用 `get_thread_summary` 或 `get_patchset` 看具体线程。

引用时带上邮件标题和 lore URL。邮件讨论的是更新版本、而这棵树更旧时，以这棵树的代码为准，并说明版本差在哪里。
