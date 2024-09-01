---
icon: mobile-screen-button
---

# Android mobile connect PC



{% code fullWidth="true" %}
```
// 通过adb 执行 magisk.apk

1. 下载最新版adb (v35)
2. 解压缩， 

//查看与PC 链接的手机设备 (需要在PC上安装手机驱动）
adb devices

// 查看包名
adb shell pm list package

// 所有包名及路径
adb shell pm list package -f
// 删除文件
adb shell rm data/system/gesture.key

//查看包的 AndroidManifest.xml， 可以看到包信息
aapt dump xmltree magisk.apk AndroidManifest.xml > manifest.txt

（在manifest.txt中 查找：manifest, uses-sdk, Activity)


//运行一个包
adb shell am start -n com.topjohnwu.magisk/com.topjohnwu.magisk.ui.MainActivity

adb shell am start -n com.topjohnwu.magisk/com.topjohnwu.magisk.ui.surequest.SuRequestActivity
```
{% endcode %}

{% code fullWidth="true" %}
```
//need rooted

一般密码锁、图案锁、隐私锁的密码文件名都是以.key为结尾，例如： 
隐私锁文件：access_control.key、file.protected.password.key、file.protected.pattern.key
密码锁文件：password.key、access.control.password.key、access.control.pattern.key。
手势锁文件：gesture.key、gatekeeper.pattern.key、gatekeeper.password.key

```
{% endcode %}



{% code fullWidth="true" %}
```
// 打开设置主界面
adb shell am start com.android.settings/com.android.settings.Settings


更多页面

com.android.settings.AccessibilitySettings 辅助功能设置
com.android.settings.ActivityPicker 选择活动
com.android.settings.ApnSettings APN设置
com.android.settings.ApplicationSettings 应用程序设置
com.android.settings.BandMode 设置GSM/UMTS波段
com.android.settings.BatteryInfo 电池信息
com.android.settings.DateTimeSettings 日期和坝上旅游网时间设置
com.android.settings.DateTimeSettingsSetupWizard 日期和时间设置
com.android.settings.DevelopmentSettings 开发者设置
com.android.settings.DeviceAdminSettings 设备管理器
com.android.settings.DeviceInfoSettings 关于手机
com.android.settings.Display 显示——设置显示字体大小及预览
com.android.settings.DisplaySettings 显示设置
com.android.settings.DockSettings 底座设置
com.android.settings.IccLockSettings SIM卡锁定设置
com.android.settings.InstalledAppDetails 语言和键盘设置
com.android.settings.LanguageSettings 语言和键盘设置
com.android.settings.LocalePicker 选择手机语言
com.android.settings.LocalePickerInSetupWizard 选择手机语言
com.android.settings.ManageApplications 已下载（安装）软件列表
com.android.settings.MasterClear 恢复出厂设置
com.android.settings.MediaFormat 格式化手机闪存
com.android.settings.PhysicalKeyboardSettings 设置键盘
com.android.settings.PrivacySettings 隐私设置
com.android.settings.ProxySelector 代理设置
com.android.settings.RadioInfo 手机信息
com.android.settings.RunningServices 正在运行的程序（服务）
com.android.settings.SecuritySettings 位置和安全设置
com.android.settings.Settings 系统设置
com.android.settings.SettingsSafetyLegalActivity 安全信息
com.android.settings.SoundSettings 声音设置
com.android.settings.TestingSettings 测试——显示手机信息、电池信息、使用情况统计、Wifi information、服务信息
com.android.settings.TetherSettings 绑定与便携式热点
com.android.settings.TextToSpeechSettings 文字转语音设置
com.android.settings.UsageStats 使用情况统计
com.android.settings.UserDictionarySettings 用户词典
com.android.settings.VoiceInputOutputSettings 语音输入与输出设置
com.android.settings.WirelessSettings 无线和网络设置

作者：GeekBug
链接：https://juejin.cn/post/6844903840274186247
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
{% endcode %}
