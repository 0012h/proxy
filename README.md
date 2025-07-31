好的，这就为您将这篇关于“小新Pad Pro 12.7 2025 (TB375FC) 全球固件安装方法”的帖子翻译成中文，并整理好每一步，以md格式呈现给您。

小新Pad Pro 12.7 2025 (TB375FC) 全球固件安装教程

本文原始出处为小米爱好者社区，由用户“mistq”发布。

据称，安装全球固件后，Widevine L1 认证仍可正常工作，并且安装的版本与官方发售的全球版完全相同。

免责声明： 对于在尝试安装全球固件过程中可能出现的任何问题，本文作者概不负责。

前言

小新Pad Pro 12.7 2025 (TB375FC) 这款型号，并未采用之前普遍使用的高通骁龙芯片，而是搭载了联发科的天玑芯片。因此，之前使用QFIL等工具安装全球固件的方法已不再适用。不过，现在发现可以使用 SP Flash Tool 工具来安装全球固件，本文将对此方法进行介绍。

注意事项

OTA 更新警告： 虽然尚未最终确认，但在进行OTA（在线升级）时，设备有很大概率会变砖。强烈建议在日常使用中关闭OTA更新功能。

软件版本： 请尽量在已完成所有OTA更新的最新版官方系统上进行此操作。（尚未在旧版系统上进行测试。）

技术说明： 经确认，设备在启动时，lk (little_kernel) 会在 dtbo 分区及其他分区中对设备是否为全球版进行验证。如果在OTA更新时，这些分区由中国版 (PRC) 变更为全球版 (ROW)，可能会导致无法开机。

如果出现这种情况，只需将 lk 和 dtbo 分区刷回中国版 (PRC) 即可正常启动。利用此原理，或许可以修复因OTA更新导致的变砖问题。

数据清除： 操作过程中，设备内部的所有用户数据都将被清除。

WideVine 及安全 (SafetyNet) 相关

经确认，Widevine 等级可维持在 L1。

SafetyNet 相关认证也均可通过。

关于 Bootloader 解锁

本教程无需解锁 Bootloader。

关于国家代码更改

可以更改国家代码。虽然无法像以前那样通过在设置中输入代码来更改，但可以通过提取 proinfo 分区，将 CN 修改为 KR (或其他代码) 来实现。

准备材料

SDK 平台工具 (ADB): 从 Android 开发者官网下载

SP Flash Tool: Google Drive 下载链接

MTK 驱动程序: 官方 MTK 驱动程序下载

全球固件 (ZUI 16): 从 mirrors.lolinet.com 下载

Scatter 文件 (重要！):

重要提示： 请根据您设备的存储容量下载对应的 scatter 文件。

128GB 型号请下载 全球_128GB

256GB 型号请下载 全球_256GB

Scatter 文件下载 (Google Drive)

各种驱动程序及初始设置

重要提示： 所有下载文件的存放路径中 不能包含中文字符，例如 “桌面” 或 “文档” 等。请将文件移动到纯英文路径下再进行操作。

下载 MTK 驱动程序后，请立即安装。

请根据相关教程设置好 SDK 平台工具 (ADB) 的环境变量。

#设备整体备份

为防万一，我们先使用 SP Flash Tool 备份原始分区。

进入 PreLoader 模式：

将设备关机。

按住 音量上键 的同时，通过数据线将设备连接到电脑。

在电脑的 “设备管理器” 中进行确认。如果看到 PreLoader 设备被识别，即可继续。

使用 SP Flash Tool 进行备份：

运行 SP Flash Tool。

在 Download-XML 选项中，点击 choose，选择与您平板存储容量相符的文件夹（例如，128GB 选择 128GB 文件夹），然后进入 download_agent 文件夹，选择 flash.xml 文件。

之后，切换到 ReadBack 选项卡，选择 Auto 菜单，然后点击 ReadPT 按钮。

此时，工具会加载出设备的分区列表。

勾选 除 userdata 之外的所有分区。

点击 Read Back 按钮开始备份。此过程大约需要10分钟，请耐心等待。

完成后，同时按住 电源键 + 音量上键 约15秒，强制重启设备。

备份完成。备份文件会保存在 ReadBack 文件夹中。这些数据对于恢复、救砖、修复序列号等至关重要，请妥善保管。

#安装全球固件

注意： 从此步骤开始，设备数据将被完全清除。请务必提前备份好您的数据。

再次进入 PreLoader 模式。

进入之前下载的全球固件文件夹 (TB373FU_ROW_OPEN_USER_M1.491_U_ZUI_16.0.20.022_ST_241022_SVC) 内的 image 文件夹。

将您下载的对应存储容量的 scatter 文件重命名为 MT6897_Android_scatter.xml，并将其移动到 image 文件夹内。

运行全球固件文件夹内的 SPFlashToolV6。

在工具界面中：

将 Download-XML 设置为 \image\download_agent\flash.xml。

将 Authentication File 设置为 \image\da.auth。

极其重要： 在下方的分区列表中，取消勾选 lk_a、lk_b、dtbo_a 和 dtbo_b 这四个分区。 （否则会导致设备变砖！）

最后，勾选上方的 Auto Reboot，并确保刷写选项为 Download Only（选择其他选项可能导致难以恢复）。

点击 Download 按钮开始刷写。整个过程大约需要10分钟。

#刷写之后

如果刷写后设备能够正常开机，则可以跳过此部分。如果无法开机，请执行以下步骤。

进入 fastboot 模式：

关机 => 开机 => 在看到 Logo 的瞬间按住 音量上键。

（如果处于无限重启状态，只需一直按住音量上键即可）。

在出现的菜单中，使用音量下键选择 fastboot mode 并进入。

为了能启动全球固件，需要检查当前的启动槽位 (boot slot)。在电脑的命令行/终端窗口中输入以下命令：

Generated code
fastboot getvar all


查看 current-slot 字段的值。如果值为 b，则需要切换到 a；如果为 a，则切换到 b。

（根据原作者经验，切换到 a 槽位时成功启动）。

使用以下命令切换启动槽位：

Generated code
fastboot --set-active=你要切换到的槽位(a/b)
```6.  切换成功后，使用以下命令重启设备：
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
IGNORE_WHEN_COPYING_END

fastboot reboot

Generated 7. 设备重启后，即可进入全球固件系统。
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
7. 设备重启后，即可进入全球固件系统。
IGNORE_WHEN_COPYING_END
Google Search Suggestions
Display of Search Suggestions is required when using Grounding with Google Search. Learn more
小新Pad Pro 12.7 2025 (TB375FC) 全球固件安装教程
translate korean to chinese online free
샤오신패드 프로 12.7 2025 (TB375FC) 글로벌롬 설치방법 chinese translation
