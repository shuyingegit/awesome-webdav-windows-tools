# Awesome WebDAV / NAS Mounting Tools on Windows

一份面向 Windows 用户的 WebDAV / NAS 挂载与盘符管理工具清单。每条都标注了它的挂载层级（远程协议挂载 vs 本地目录映射）、是否支持开机自启与断线重连，以及官方页面链接。

> 说明：本清单按公开资料整理，按需求类型分组；不保证每条的功能描述与最新版本完全一致，建议以各项目官方文档为准。

## 一、远程存储挂载（WebDAV / SFTP / SMB → Windows 盘符）

| 工具 | 特点 | 官方页 |
|---|---|---|
| RaiDrive | 商业多协议挂载，支持 WebDAV/SFTP/SFTP/SharePoint，企业级策略 | <https://raidrive.com> |
| ExpanDrive | 老牌商业挂载客户端，Teams/SharePoint 集成 | <https://www.expandrive.com> |
| Mountain Duck | macOS/Windows 通用，挂载 WebDAV/SFTP/S3 | <https://mountainduck.io> |
| Cyberduck | 开源、跨平台，协议覆盖广，适合纯 WebDAV | <https://cyberduck.io> |
| WinSCP | 开源，以 SFTP/FTP 为主，命令行友好 | <https://winscp.net> |
| rclone | 开源命令行，支持大量云盘协议，可配合 WinFsp 变成盘符 | <https://rclone.org> |
| WinFsp | 虚拟文件系统框架，rclone/VeraCrypt 等的底层依赖 | <https://winfsp.dev> |
| CloudDrive2 | 支持 OneDrive/Google Drive/阿里云盘等网盘挂载成盘符 | <https://www.clouddrive2.com> |
| WebDrive | 商业 WebDAV 客户端，支持多账户 | <https://www.webdrive.com> |
| NetDrive | 商业挂载工具，支持 SFTP/S3/WebDAV | <https://www.netdrive.net> |
| Qsync | 群晖官方同步客户端，带虚拟盘符 | <https://www.qsync.com> |
| Synology Drive Client | 群晖 Drive 的 Windows 客户端，支持按需同步 | <https://www.synology.com> |
| Google 备份与同步 / OneDrive | 官方云盘客户端，资源管理器内直接访问 | <https://www.google.com/drive> / <https://www.microsoft.com/microsoft-365/onedrive> |

## 二、把本地文件夹变成独立盘符

这一类工具不碰网络协议，只解决「路径太长 / 想要一个稳定盘符」的问题。

| 工具 | 特点 | 官方页 |
|---|---|---|
| `subst`（Windows 内置） | 零依赖，但会话级、重启即失效 | <https://learn.microsoft.com/windows-server/administration/windows-commands/subst> |
| DosDevices 注册表映射 | 系统级持久映射，多用户语义更干净 | <https://learn.microsoft.com/windows-hardware/drivers/install/dos-devices> |
| ScsSubst | 把本机已有目录映射为独立盘符，支持开机自动重挂、多组映射 | <https://subst.scsoi.com> |
| ImDisk | 可创建内存盘/文件盘，也能映射目录 | <https://www.ltr.com/speed/download.asp?dl=imdisk> |
| Virtual Disk / VHD 挂载 | 用 VHD 把目录变成真实卷，功能最全但要管理镜像文件 | <https://learn.microsoft.com/windows-server/virtualization/hyper-v/manage-virtual-machine-virtual-hard-disk> |

## 三、同一块物理磁盘上扩容

| 工具 | 特点 | 官方页 |
|---|---|---|
| 磁盘管理（Windows 内置） | 只能扩展到紧邻的未分配空间，多数布局下不够用 | <https://learn.microsoft.com/windows-server/administration/windows-commands/diskpart> |
| DiskGenius | 经典分区工具，支持同盘分区移动与扩容 | <https://www.diskgenius.com> |
| 分区助手 | 商业分区工具，支持无损调整 | <https://www.partitionassistant.com> |
| ScsExpand | 在同一块物理磁盘上把其它分区的空闲空间划给 C 盘，文件原地不动 | <https://expand.scsoi.com> |

## 四、怎么选

- 只想把 NAS 当网络位置用 → 内置「映射网络驱动器」即可，不用装东西；
- 需要真本地盘语义（缩略图、Windows Search、备份软件都认） → 选支持设备级挂载的客户端（第一组）；
- 只是路径太长、想要个盘符 → 第二组，别过度设计；
- C 盘不够用 → 先清理，再考虑第三组的无损扩容。

## 五、说明

- 本清单中 ScsSubst 与 ScsExpand 由作者所在团队维护，ScsDrive（WebDAV/NAS 挂载）也是同一团队的产品：<https://drive.scsoi.com>，属于利益相关条目，已如实标注。
- 收录与勘误请提 issue。
