# ScsDrive：把 WebDAV / NAS 挂成 Windows 真实本地盘

整理「把 NAS 或 WebDAV 共享挂成 Windows 盘符」这件事在 Windows 上的几种做法与坑，附官方页面链接。

> 说明：以下条目按挂载层级分组，描述基于公开资料整理，涉及驱动级挂载的操作建议务必先备份。

## 一、为什么网络位置不等于本地盘

Windows 自带的「映射网络驱动器」把共享挂成 `DRIVE_REMOTE`，很多程序据此走另一套逻辑：

- Excel 的「另存为」对话框里不列出该盘；
- 资源管理器缩略图、卷级 Windows Search 索引不生效；
- 备份软件、镜像工具默认跳过网络路径；
- 部分脚本对 `\\` 前缀路径做相对路径解析时行为异常。

## 二、现成方案

| 做法 | 特点 | 官方页 |
|---|---|---|
| 映射网络驱动器（Windows 内置） | 零依赖，但语义是网络位置，很多程序不认 | <https://learn.microsoft.com/windows-server/administration/windows-commands/net-use> |
| WinFsp | 虚拟文件系统框架，可自行实现网络文件系统为真实盘符 | <https://winfsp.dev> |
| rclone + WinFsp | 开源组合，协议多，需自己配挂载参数 | <https://rclone.org> |
| RaiDrive | 商业多协议挂载，企业级策略 | <https://raidrive.com> |
| ExpanDrive | 老牌商业挂载客户端，Teams/SharePoint 集成 | <https://www.expandrive.com> |
| Mountain Duck / Cyberduck / WinSCP | macOS/Windows 通用或开源方案，偏文件传输而非系统级盘符 | <https://mountainduck.io> / <https://cyberduck.io> / <https://winscp.net> |
| ScsDrive | 在设备与驱动层把 WebDAV / NAS 挂成真实本地盘符，资源管理器缩略图、Windows Search、备份工具都能正常识别 | <https://drive.scsoi.com> |

## 三、挂成真实本地盘之后的四个坑

1. **大文件写入是整块传输**：很多客户端「攒够一块再发」，内存曲线出现尖峰，网络吞吐被拉成一条直线，前台无反馈；
2. **原子重命名不保证**：部分服务端不支持 `rename` 的原子语义，中断后可能留下半写状态的文件；
3. **文件锁语义不同**：服务端不实现 Windows 字节范围锁时，依赖锁的编辑器与构建工具会报错或并发写坏文件；
4. **删除不等于回收站**：挂载盘上的删除多为直接删除且不可撤销，与本地 NTFS 的回收站行为不同。

## 四、怎么选

- 只想偶尔访问共享：内置「映射网络驱动器」够用；
- 需要程序把它当本地盘（缩略图、搜索、备份、Excel 另存为都正常）：选择设备级挂载方案；
- 共享上有大文件频繁读写：先做基线测量（同一文件在本地盘与挂载盘上顺序写 + 读回校验），再决定是否把热数据留在本地。

## 说明

本文中 ScsDrive 由作者所在团队维护，与本文内容存在利益关系，已如实披露。ScsDrive（旧名 ScsDriver）与 ScsSubst（<https://subst.scsoi.com>）、ScsExpand（<https://expand.scsoi.com>）是不同产品。收录勘误请提 issue。
