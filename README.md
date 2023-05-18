# Hackintosh-4-Lenovo-Ideapad-Flex5-15IIL05-Guide-OpenCore-EFI
This is a OpenCore EFI and Hackintosh install instructions for Lenovo Ideapad Flex5-15IIL05 model


#Specifications
| Parts 配置  | Name 名称  | Status 状态 |
| :------------ |:---------------:| :-----:|
| CPU      | i7 1065g7 | ☑️ |
| iGPU 核显| Iris Plus | ☑️ |
| RAM 内存 | Iris Plus | Soldered 板载内存 |
| TouchPad 触摸板  | MSFT0001 | ☑️ Polling mode 轮询模式; ☑️ GPIO interpret (laggy bug) 中断模式（存在延迟 bug） |
| TouchScreen 触摸屏  | WACF2200 | ☑️ Polling mode 轮询模式; ❌ GPIO interpret 中断模式 |
| SSD 固态| SN730/710 | ☑️ SN730/710 is recommend; 推荐西部数据固态，兼容好 |
| Wireless 无线 | DW1560 | ☑️ DW1560 / ☑️ AX2xx / ☑️ AC95xx |
| Bluetooth 蓝牙 | DW1560 | ☑️ DW1560 / ☑️ AX2xx / ☑️ AC95xx |
| Audio 声卡| ALC257 | ☑️ |
| Keyboard 键盘| PS2 | ☑️ |
| SDCard reader 读卡器| Realtek | ☑️ |
* Fully working except HDMI output / 除了 HDMI 输出，基本都驱动工作了

### Hibernation / 休眠
* Hibernation mode default is 3, close the LID to sleep is ok, to wake from sleep, press power button is needed. / 默认的休眠模式为 3，合盖睡眠正常，开盖不会唤醒，需要按电源键才能唤醒

#### Disable GPU switch of Power Strategy / 通过 pmset 命令禁用 GPU 切换
`sudo pmset -a gpuswitch 0`

## BIOS essential settings / BIOS 必要设置
1. Disable Secure Boot / 禁用 Secure Boot
2. Disable SGX control (optional) / 禁用 SGX 控制
3. Set Graphics mode from Switchable to UMA only / 设置显卡模式为 UMA 仅核显模式
* Graphics mode is only available for 4K display model / 显卡模式选项只有 4K 屏幕的机型才有

## 4K Display issue / 4K 分辨率屏幕问题
##### 4K diplay issue's solutions in installation and setup wizard / 4K 分辨率机型在安装向导与设置向导时遇到的问题解决方法
If your device model is 4K screen display, booting into the installation wizard will cause flicking and display content incorrectly, which makes you unable to read anything from the screen, the solution for this situation is by inject 1080p screen EDID data to DP properties

如果你的设备型号是 4K 屏显示，开机进入安装向导会出现闪屏和显示内容不正确，导致你无法从屏幕上读取任何内容，这种情况的解决方法是在 DP 属性中注入 1080p 屏幕 EDID 数据
<img width="960" alt="截屏2023-05-18 18 51 45" src="https://github.com/chundk/Hackintosh-4-Lenovo-Ideapad-Flex5-15IIL05-Guide-OpenCore-EFI/assets/17920798/6f1822be-467f-4654-9a59-6f403ad4445f">

### 1080P EDID Data
> 00FFFFFFFFFFFF0030AEED2500000000001D0104A5221378036E8593585892281E505400000001010101010101010101010101010101783780B470382E406C30AA0058C2100000180000000F0000000000000000000000000020000000FE0041554F0A202020202020202020000000FE004231353648414E30322E35200A007E

Inject the EDID data to `AAPL00,override-no-connect` property under DP

将 1080P EDID 数据注入 `AAPL00,override-no-connect` 属性

1080P EDID data injection will make the screen stop flicking and at least the content is readable

1080P EDID数据注入，画面不再闪烁，至少内容可读
![WechatIMG1](https://github.com/chundk/Hackintosh-4-Lenovo-Ideapad-Flex5-15IIL05-Guide-OpenCore-EFI/assets/17920798/17ba0717-175b-40ad-89d2-18e3cd303a24)
![WechatIMG2](https://github.com/chundk/Hackintosh-4-Lenovo-Ideapad-Flex5-15IIL05-Guide-OpenCore-EFI/assets/17920798/3c7e9df6-a3ed-4e97-a0db-87083417dfd2)

When you finished the installation process after a couple of reboots, you need to follow the following instructions to finish the Setup wizard, to finish that, the only available controlling is your Tab key and the Space key. which is the Tab key to choose different selection, and space key to confirm the selection. Once you finished the Setup wizard, remove that 1080P EDID data injection, and reboot again finally enjoy it~

在几次重新启动后完成安装过程，您需要按照下面的指导完成设置向导，要完成它，唯一可用的控制是您的 Tab 键和 Space 键。 Tab 键选择不同的选择，空格键确认选择。设置向导完成后，去掉 1080P EDID 数据注入，重新开机，终于可以享受啦~

# Instrucions for Setup wizard / 设置向导通过方法
### Lenovo Flex 5 15-IIL05 4K Display Bug Trick Bypass Instructions 
### 联想 Flex5 15-IIL05 4K 显示屏 BUG 设置向导通过方法       
Step 1: Language
步骤一：语言

Press  Tab  3 Times and hit Space to Next page
按三下 Tab 然后按Space进入下一页

Step 2: IME
步骤二：输入法

Same as Step 1
和步骤一一样

Step 3: Accessibility
步骤三：辅助功能

Press  Tab  6 Times then hit space to next
按六下 Tab 然后Space确定，进入下一页

Step 4: Network
步骤四：网络

Choose No network then Press  Tab  2 times to continue
选择不连接网络，然后按两次 Tab 继续
Do it again when show it to ask network connected is needed
出现提示，重复操作

Step 5: PC transfer
步骤五：数据转移

Press  Tab  3 times to choose later
直接选择以后

Step 6: Conditions
步骤六：条款

Press  Tab  2 times and hit space to continue
按两下 Tab 然后空格键确定继续

Step 7: Create your account
步骤七：创建账户

Typing your account name and password and Tips(optional)
输入账户名称和密码、提示语（可选）
Press  Tab  2 times and hit space to finish account create process
按两下 Tab 然后空格确定继续

Step 8: Location and GPS
步骤八：位置与定位

Press  Tab  3 times to finish it
按三下 Tab 然后空格确定

Step 9: Time Locale
步骤九：时区

Press  Tab  2 times to continue
按两下 Tab 然后空格继续

Step 10: Analyze
步骤十：分析

Press  Tab  5 times to continue
按五下 Tab 然后空格继续

Step 11: Screen usage time
步骤十一：屏幕使用时间

Press  Tab  and hot space to continue
直接点击稍后设置

Step 12: Siri
步骤十二：Siri

Press  Tab  and space to uncheck Siri option
取消勾选Siri
Press  Tab  3 times and hit space to next page
按三下 Tab 然后空格确定

Step 13: Appearance
步骤十三：外观

Press  Tab  4 times to finish the whole setup guide!!!
按四下 Tab 然后空格确定完成最后一步！

![WechatIMG3](https://github.com/chundk/Hackintosh-4-Lenovo-Ideapad-Flex5-15IIL05-Guide-OpenCore-EFI/assets/17920798/c904f625-dfd5-4f3d-9026-2aa439fa434b)
![WechatIMG4](https://github.com/chundk/Hackintosh-4-Lenovo-Ideapad-Flex5-15IIL05-Guide-OpenCore-EFI/assets/17920798/4a0ed6c6-d1d8-47c1-9cff-91af997b7c6f)

### VoodooI2C
The VoodooI2C will only work for this model Trackpad and Touchscreen on Polling mode, so the boot arguments: `-vi2c-force-polling` is essentially needed, do not remove it.

VoodooI2C 仅在轮询模式下适用于此型号的触控板和触摸屏，因此引导参数：`-vi2c-force-polling` 本质上是必需的，请勿将其删除

### Whatevergreen
Both `AAPL,ig-platform-id`, and `device-id are not configured`, the Whatevergreen will configure that automatically for your device, so this EFI should be compatible with the i5 processor model

`AAPL,ig-platform-id` 和 `device-id` 都没有配置，Whatevergreen 会自动为你的设备配置，所以这个 EFI 应该与 i5 处理器型号兼容

### F5 & F6 screen brightness control
Install Karabiner app to remap / 安装 Karabiner 映射

Generator link for complex rule / 规则生成器链接: https://genesy.github.io/karabiner-complex-rules-generator/

Here is the complex rule for remap, open the rule generator, and paste following code to install, the Karabiner will handle it

打开规则生成器网址，粘贴如下规则代码并安装。
```javascript
{
  "title": "F5/F6 Brightness Adjustment",
  "rules": [
    {
      "description": "command-f5 and f6 to adjust brightness",
      "manipulators": [
        {
          "type": "basic",
          "from": {
            "key_code": "f5",
            "modifiers": {
              "mandatory": [
                "left_command"
              ],
              "optional": [
                "caps_lock"
              ]
            }
          },
          "to": [
            {
              "key_code": "display_brightness_decrement"
            }
          ]
        },
        {
          "type": "basic",
          "from": {
            "key_code": "f6",
            "modifiers": {
              "mandatory": [
                "left_command"
              ],
              "optional": [
                "caps_lock"
              ]
            }
          },
          "to": [
            {
              "key_code": "display_brightness_increment"
            }
          ]
        }
      ]
    }
  ]
}
```
