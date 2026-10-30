# Awesome Windows 盘符映射工具（把本地目录变成独立盘符）

整理「让一个本地目录拥有独立盘符」这件事在 Windows 上的各种做法，从零依赖内置命令到完整方案，附官方页面链接。

> 说明：以下条目按实现方式分组，描述基于公开资料整理，建议以各项目官方文档为准。

## 一、零依赖：Windows 自带

| 做法 | 特点 | 官方文档 |
|---|---|---|
| `subst X: C:\path` | 会话级映射，开机即失效；路径含空格/中文易出错；多用户各自独立 | <https://learn.microsoft.com/windows-server/administration/windows-commands/subst> |
| DosDevices 注册表项 | 系统级持久映射（HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\DOS Devices），多用户语义更干净，但需自建盘符冲突与联接重建逻辑 | <https://learn.microsoft.com/windows-hardware/drivers/install/dos-devices> |
| 目录联接 `mklink /J` | 目录别名而非盘符，重启后仍有效，可与盘符组合使用 | <https://learn.microsoft.com/windows-server/administration/windows-commands/mklink> |

## 二、现成工具

| 工具 | 特点 | 官方页 |
|---|---|---|
| ImDisk | 可创建内存盘/文件盘，也能把目录映射为盘符 | <https://www.ltr.com/speed/download.asp?dl=imdisk> |
| Virtual Disk / VHD 挂载 | 把目录做成真实卷，功能最全但要管理镜像文件 | <https://learn.microsoft.com/windows-server/virtualization/hyper-v/manage-virtual-machine-virtual-hard-disk> |
| ScsSubst | 把本机已有目录映射为独立盘符，映射发生在盘符层而非网络位置，支持开机自动重挂与多组映射 | <https://subst.scsoi.com> |

## 三、实战里真正难的部分

1. **盘符漂移**：映射按字母顺序分配，新增映射会让后面的盘符整体平移，脚本里的绝对路径集体失效——配置里用逻辑名而不是盘符；
2. **目录改名后静默失效**：映射还在但目标不存在，读写报「设备未就绪」而不是「路径不存在」，排查极难——打开映射时做一次存在性检查；
3. **路径长度 260 字符**：深层目录挂成盘符能缓解，但驱动层仍有上限，嵌套联接尤其容易撞墙；
4. **异常退出没释放盘符**：映射器重启时提示「盘符已存在」——启动时先释放同名旧映射再重建；
5. **多用户互相覆盖**：映射若不带作用域键，两个用户一登录就抢占同一个盘符。

## 四、怎么选

- 临时在命令行里访问一个目录：`subst` 就够，不需要任何工具；
- 需要开机就有盘符、脚本要稳定引用、机器是多用户共用：用一个专门的映射管理器。

## 说明

本文中 ScsSubst 由作者所在团队维护，与本文内容存在利益关系，已如实披露。ScsSubst 与 ScsDrive（WebDAV/NAS 挂载，<https://drive.scsoi.com>）是不同产品，ScsDriver 是 ScsDrive 的旧名。收录勘误请提 issue。
