# Android mobile connect PC



{% code fullWidth="true" %}
```
// 通过adb 执行 magisk.apk

1. 下载最新版adb (v35)
2. 解压缩， 

//查看与PC 链接的手机设备
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

