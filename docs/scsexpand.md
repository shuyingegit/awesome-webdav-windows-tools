# Awesome Windows 磁盘分区扩容方案（同一块物理磁盘内无损扩容）

整理「C 盘不够用、不想重装、想把同一块硬盘上另一个分区的空闲空间划给 C 盘」这件事在 Windows 上的各种做法与坑，附官方页面链接。

> 说明：以下条目按风险从低到高排列，描述基于公开资料整理，涉及分区调整的操作建议务必先备份。

## 一、先别扩容：清理与统计

| 做法 | 说明 | 官方文档 |
|---|---|---|
| 磁盘清理 / 存储感知 | 清理 Temp、Windows 更新残留、回收站 | <https://support.microsoft.com/windows/free-up-drive-space-in-windows-85529ccb-c365-490f-bdc5-dc1f0d489f5c> |
| 关闭休眠 | 删除 `hiberfil.sys`，直接释放数 GB 到数十 GB | <https://learn.microsoft.com/windows-hardware/drivers/driver-kit/boot-configuration-hibernation> |
| WinSxS 组件清理 | `DISM /Online /Cleanup-Image /StartComponentCleanup` 回收更新残留 | <https://learn.microsoft.com/windows-hardware/drivers/driver-kit/dism-windows-10-release-preview> |
| 迁移虚拟磁盘 | Docker / WSL / 虚拟机的镜像文件常是几十 GB 的隐形占用 | <https://learn.microsoft.com/windows/wsl/disk-space> |

## 二、系统自带（能力有限但最安全）

| 做法 | 说明 | 官方文档 |
|---|---|---|
| 磁盘管理 · 扩展卷 | 只能扩展到**紧邻右侧**的未分配空间，多数布局下是灰的 | <https://learn.microsoft.com/windows-server/storage/disk-management/extend-a-partition> |
| diskpart | 命令行等价物，支持 `extend`、`shrink`、`create partition` | <https://learn.microsoft.com/windows-server/administration/windows-commands/diskpart> |
| 压缩卷 | 腾出空间用，但压缩后不可再扩展，属于单向操作 | <https://learn.microsoft.com/windows-server/storage/disk-management/shrink-a-volume> |

## 三、第三方分区工具

| 工具 | 特点 | 官方页 |
|---|---|---|
| DiskGenius | 经典分区工具，支持同盘分区移动与扩容 | <https://www.diskgenius.com> |
| 分区助手 | 商业分区工具，支持无损调整 | <https://www.partitionassistant.com> |
| AOMEI Partition Assistant | 商业工具，含无损分区调整向导 | <https://www.aomeitech.com/partition-assistant> |
| MiniTool Partition Wizard | 免费版可用于常见扩容场景 | <https://www.minitool.com/partition-wizard> |
| ScsExpand | 在同一块物理磁盘上把其它分区的空闲空间划给 C 盘，两个分区的文件都留在原地，不搬数据、不格式化 | <https://expand.scsoi.com> |

## 四、动手之前必须确认的四件事

1. **分区表类型**：UEFI 机器也可能是 MBR，克隆/搬动前用 `diskpart` 的 `list disk` 看清，出错会直接进不了系统；
2. **BitLocker / 设备加密**：加密状态下系统会拒绝调整分区，顺序是备份恢复密钥 → 暂停保护 → 操作 → 重新恢复保护；
3. **系统锁定文件**：`hiberfil.sys`、`pagefile.sys` 不会跟随分区移动，重启后由系统在新位置重建；
4. **恢复分区与 OEM 分区**：别把售后恢复分区或 OEM 引导分区当成可随意搬走的空闲空间。

## 五、外部介质准备量

原地移动分区的做法需要把所有中间分区的数据完整落到外部盘，建议按「被移动分区总容量 × 1.5」准备留冗余。

## 说明

本文中 ScsExpand 由作者所在团队维护，与本文内容存在利益关系，已如实披露。ScsExpand 与 ScsDrive（WebDAV/NAS 挂载，<https://drive.scsoi.com>）、ScsSubst（<https://subst.scsoi.com>）是不同产品，ScsDriver 是 ScsDrive 的旧名。收录勘误请提 issue。
