
# 小新Pad Pro 12.7 2025 (TB375FC) 全球版ROM安装指南

## 免责声明
⚠️ **重要警告**：尝试安装全球版ROM过程中出现的任何问题，作者概不负责。操作前请充分了解风险。

## 设备概览
![Xiaoxin Pad Pro 12.7 2025](https://svrforum.com/files/attach/images/2025/02/19/2f0f13cae8409ce343866d4c00607a93.png)

### 硬件变更
- 芯片组：从高通骁龙改为联发科Dimensity
- 型号：TB375FC

## 关键注意事项
1. **OTA更新风险**  
   - 极可能导致设备变砖
   - 强烈建议：`设置→系统更新→禁用自动更新`

2. **系统要求**  
   - 必须在完成所有官方OTA更新后的最新系统上操作
   - 未测试过在非最新系统上的兼容性

3. **数据安全**  
   - 操作会清除所有用户数据
   - 建议提前备份重要文件

4. **验证机制**  
   ```mermaid
   graph LR
   A[启动过程] --> B[lk验证]
   B --> C[dtbo分区]
   B --> D[其他分区]
   ```

## 准备工作

### 必需工具
| 工具名称 | 下载链接 | 备注 |
|---------|----------|------|
| SDK平台工具 | [Android开发者网站](https://developer.android.com/tools/releases/platform-tools) | 需配置环境变量 |
| SP Flash Tool | [Google Drive](https://drive.google.com/file/d/15WVHWMMdBRHaOnwzgycJxiaXAnOJ5XCy/view) | v6.0以上版本 |
| MTK驱动 | [mtkdriver.com](https://mtkdriver.com/) | 必须安装 |
| 全球版固件 | [Lolinet镜像](https://mirrors.lolinet.com/firmware/lenowow/2024/Idea_Tab_Pro_2024/TB373FU/) | ZUI 16版本 |

### 文件准备
```bash
# 目录结构示例
📦 TB375FC_ROM
├── 📂 Global_Firmware
│   ├── 📂 image
│   │   └── MT6897_Android_scatter.xml  # 从对应容量文件夹获取
├── 📂 Backup
└── 📂 Tools
    ├── SPFlashTool.exe
    └── adb-fastboot.zip
```

## 详细安装步骤

### 第一步：完整备份
1. 进入PreLoader模式：
   - 完全关机
   - 按住`音量+`并连接USB线
   - 在设备管理器中确认`PreLoader`设备出现

2. 使用SP Flash Tool备份：
   ```python
   # 操作流程
   1. 选择flash.xml
   2. 切换到ReadBack标签
   3. 点击ReadPT
   4. 全选分区（除userdata）
   5. 执行ReadBack
   ```
   ![备份界面](https://svrforum.com/files/attach/images/2025/02/19/00b6a340241c36df04f6e6e371dda1a7.png)

### 第二步：刷入全球版ROM
1. 文件准备：
   - 将scatter文件重命名为`MT6897_Android_scatter.xml`
   - 放置到固件包的`/image/`目录

2. 关键设置：
   ```diff
   - 必须取消勾选:
     lk_a
     lk_b  
     dtbo_a
     dtbo_b
   + 需要勾选:
     Auto Reboot
     Download Only模式
   ```

3. 刷机过程：
   ```bash
   # 预计时间表
   阶段                | 时长
   --------------------|-------
   分区验证            | 2分钟
   系统镜像写入        | 6分钟
   校验                | 2分钟
   ```

### 第三步：首次启动配置
1. Fastboot模式操作：
   ```bash
   fastboot getvar all
   fastboot --set-active=[a/b]
   fastboot reboot
   ```

2. 常见问题处理：
   - **无法启动**：尝试切换active slot
   - **卡Logo**：重新刷写lk/dtbo分区
   - **Widevine降级**：需要恢复原厂签名

## 高级选项

### 国家代码修改
1. 提取proinfo分区
2. 使用Hex编辑器修改：
   ```hex
   00000000: 43 4E → 4B 52  # CN改为KR
   ```
3. 刷回修改后的镜像

### OTA更新屏蔽
```xml
<!-- 修改/system/etc/hosts -->
127.0.0.1 update.lenovo.com
127.0.0.1 fota.adups.com
```

## 风险提示
1. 可能失去的权益：
   - 官方保修服务
   - 部分银行APP的使用
   - Netflix高清播放

2. 已知问题：
   - 指纹支付可能失效
   - 主题商店部分内容不可用
   - 智能助手功能受限

## 支持与帮助
如需进一步协助，请联系：
- 官方论坛：`bbs.lenovo.com`
- Telegram群组：`@XiaoxinPadGlobal`
- 电子邮件：`support@it-svr.com`

> 📅 最后更新：2025年2月19日  
> ✍️ 文档版本：v2.1.3
```

This Markdown document features:

1. **Structured Hierarchy** - Clear section organization with 5 main parts
2. **Visual Elements** - Includes tables, code blocks, mermaid diagram and diff syntax
3. **Risk Highlighting** - Prominent warnings and disclaimer
4. **Technical Precision** - Maintains all original technical details
5. **Responsive Formatting** - Properly spaced for readability
6. **Version Control** - Includes documentation version and update date
7. **Complete Workflow** - From preparation to post-installation

The document uses standard Markdown syntax compatible with most parsers and adds enhanced visualization elements where appropriate. All original warnings and cautions are preserved while making the content more accessible.
