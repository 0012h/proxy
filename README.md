# 小新Pad Pro 12.7 2025 (TB375FC) 全球版ROM安装方法

## 警告
尝试安装全球版ROM过程中出现的问题，作者概不负责。

## 前言
![设备图片](https://svrforum.com/files/attach/images/2025/02/19/2f0f13cae8409ce343866d4c00607a93.png)

TB375FC(小新Pad Pro 12.7 2025)型号不再使用高通骁龙芯片组，而是搭载了新的联发科Dimensity芯片组。因此，之前使用QFIL等工具安装全球版ROM的方法变得困难，但发现可以使用SP_Flash_Tool安装全球版ROM。

## 注意事项
1. **OTA更新极可能导致设备变砖**，强烈建议禁用OTA更新后使用
2. 建议在所有OTA更新完成后进行此操作(未在非最新系统上测试过)
3. 启动时lk(little_kernel)会在dtbo分区和其他分区验证设备是否为全球版，如果OTA更新将这些分区从中国版(PRC)改为全球版(ROW)，设备将无法启动并显示错误信息(此时只需将lk和dtbo分区恢复为中国版即可正常启动)
4. 操作过程中所有用户数据将被清除

### 其他信息
- **WideVine及安全(SafetyNet)**: 确认可保持L1级别，SafetyNet也可全部通过
- **Bootloader解锁**: 不需要解锁
- **国家代码更改**: 可以通过修改proinfo分区将CN改为KR(无法像以前那样通过设置中输入代码更改)

## 准备工作
### 所需工具
1. [SDK平台工具](https://developer.android.com/tools/releases/platform-tools?hl=ko)
2. [SP Flash Tool](https://drive.google.com/file/d/15WVHWMMdBRHaOnwzgycJxiaXAnOJ5XCy/view)
3. [MTK驱动](https://mtkdriver.com/)
4. 全球版固件(ZUI 16): [TB373FU固件](https://mirrors.lolinet.com/firmware/lenowow/2024/Idea_Tab_Pro_2024/TB373FU/)
5. Scatter文件(根据设备存储容量选择):
   - 128GB型号: [全球版_128GB](https://drive.google.com/drive/folders/1KTX6kK6DxcjoVzZnwMo21wIbEWccd1dm)
   - 256GB型号: [全球版_256GB](https://drive.google.com/drive/folders/1KTX6kK6DxcjoVzZnwMo21wIbEWccd1dm)

### 重要提示
- 下载文件的路径中不能包含韩文(如桌面、文档等)，请将文件夹移动到不含韩文的路径下操作

## 驱动及初始设置
1. 安装MTK驱动
2. 按照[此指南](https://usefultoknow.tistory.com/entry/Android-%EC%9C%88%EB%8F%84%EC%9A%B0-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C-ADB-%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C-%EB%B0%8F-%ED%99%98%EA%B2%BD-%EB%B3%80%EC%88%98-%EC%84%A4%EC%A0%95)设置SDK平台工具环境

## 设备完整备份
1. 进入PreLoader模式: 
   - 关机 → 按住音量上键同时连接电脑 → 在设备管理器中确认PreLoader设备识别
   ![PreLoader模式](https://svrforum.com/files/attach/images/2025/02/19/0b14962058c5e12d55aaa658cbfd7407.png)

2. 运行SP Flash Tool:
   - 在Download-XML中选择与设备存储容量匹配的文件夹中的flash.xml文件
   - 切换到ReadBack选项卡，选择Auto菜单，点击ReadPT按钮
   ![读取分区](https://svrforum.com/files/attach/images/2025/02/19/ee4f631fc0127eca78bdc67ee7be6196.png)

3. 选择除userdata外的所有分区，点击Read Back按钮
   ![选择分区](https://svrforum.com/files/attach/images/2025/02/19/00b6a340241c36df04f6e6e371dda1a7.png)

4. 等待约10分钟完成备份
   ![备份完成](https://svrforum.com/files/attach/images/2025/02/19/c47947665dcdf6cd75ffd43073299715.png)

5. 长按电源键+音量上键15秒重启设备
6. 备份文件保存在ReadBack文件夹中，请妥善保管
   ![备份文件](https://svrforum.com/files/attach/images/2025/02/19/fda5e3caf7643a05edafeaba6bc685dc.png)

## 全球版ROM安装
### 警告
此过程将清除所有数据，请先备份重要数据

1. 再次进入PreLoader模式(同上)
2. 将下载的scatter文件重命名为`MT6897_Android_scatter.xml`并移动到全球版固件文件夹的image子文件夹中
3. 运行全球版固件文件夹中的`SPFlashToolV6`
   ![SP Flash Tool](https://svrforum.com/files/attach/images/2025/02/19/f07225809c098d51a2ab9268aedd4063.png)

4. 设置:
   - Download-XML: `\image\download_agent\flash.xml`
   - Authentication File: `\image\da.auth`
   ![工具设置](https://svrforum.com/files/attach/images/2025/02/19/bb863961e74d5a63d39ac1f6b3904e29.png)

5. **重要**: 取消选中`lk_a`、`lk_b`、`dtbo_a`和`dtbo_b`分区(否则会导致设备变砖)
   ![取消选中分区](https://svrforum.com/files/attach/images/2025/02/19/e8dc6f6319540fb92c54953489f3adfe.png)

6. 勾选Auto Reboot，选择Download Only选项，点击Download按钮
7. 刷机过程约需10分钟

## 刷机后处理
如果刷机后能正常启动，可跳过此步骤；如果无法启动，请按以下步骤操作:

1. 进入fastboot模式:
   - 关机 → 开机 → 出现logo时立即按音量上键
   (如果无限重启，只需按住音量上键)
   ![Fastboot模式](https://svrforum.com/files/attach/images/2025/02/19/e1ee3b1cf64ca490bf849f386d03edb3.png)

2. 在电脑CMD窗口中输入以下命令检查当前bootslot:
   ```bash
   fastboot getvar all
