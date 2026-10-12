# 无 root 部署：emulator-5558

测试包：`com.sunborn.girlsfrontline.pt`。不会覆盖 `com.sunborn.girlsfrontline.jp`。

在本目录执行：

```powershell
./install_rootless.ps1 -Device emulator-5558 -Archive "D:\Picture\少女前线\日服\com.sunborn.girlsfrontline.jp.tar"
```

脚本安装 APK、启动内置资源导入页，并通过临时 ADB 端口转发传输原压缩包。APK 自己解包、创建目录、逐文件计算 SHA-256；与电脑计算的公开资源摘要一致后自动进入游戏。首次正常输入本人 PC 继承版账号，之后使用加密会话恢复。其他设备登录导致会话失效时，仍需正常重新登录；应用不会自动抢回账号。

也可仅安装 APK，将完整 `.tar` 放到模拟器 Download 等文件选择器可访问的位置，然后在首次启动的导入页选择它。读取使用 Android 系统文件选择器授权，不需要 root、Frida 或广泛存储权限。运行游戏不需要电脑或安装脚本。

直接把原压缩包的 `com.sunborn.girlsfrontline.jp` 文件夹解到测试包外部数据目录不可作为部署方法：包名不匹配，且 Android 14 上 ADB 解包创建的目录归 shell 所有，应用无权访问。内置导入器仅导入 `files/Android/` 的公开资源，并映射到测试包自己的目录；不导入原日服的私有账号、stc 或 il2cpp 缓存。

本例资源约 12 GB；保留原 `.tar`。新安装建议有至少 16 GB 可用空间；通过模拟器内 Download 选择完整压缩包，需要同时容纳压缩包和解包资源，建议至少 28 GB。导入期间保持应用打开。中断后留在导入页，重新执行脚本或重新选择压缩包即可，未完成导入不会进入游戏。

复现冷启动（仅在完成首次登录后）：

```powershell
$env:GF_COMPAT_DEVICE='emulator-5558'
$env:GF_COMPAT_EVIDENCE_PREFIX='rootless'
python ./session_cold_test.py
```

APK 保留会话版同一签名，升级安装保留测试包数据。回退到前版可使用 `archive/session_467144/JAPANESE_BASE_PC_INHERITANCE_TEST.apk`；新模拟器无需回退原日服。卸载测试包会删除其会话和公开资源，原压缩包不受影响。该客户端仍为日语测试版。
