---
layout: post
title: 【Android】adb 常用命令
date: 2020-11-23
description: adb 调试常用命令
tag: Android
---

# Android adb 常用命令

##  adb启动activity：

$ adb shell am start -n ｛包(package)名｝/ {活动(activity)名称}

如：启动浏览器

```
adb shell am start -n com.android.browser/com.android.browser.BrowserActivity
```
 
##  adb启动service：

$ adb shell am startservice -n ｛包(package)名｝/{服务(service)名称}

如：启动自己应用中一个service

```
adb shell am startservice -n com.android.traffic/com.android.traffic.maniservice
```
 
##  adb发送broadcast：
$ adb shell am broadcast -a｛广播动作｝

如：发送一个网络变化的广播

$ --ei --es 表示广播带的参数，-f 添加的flag

```
adb shell am broadcast -a com.example.Broadcast
```

$ 发送一个带 json 格式的广播

```
adb shell " am broadcast -a com.example.Broadcast -e 'com.example.Broadcast.extra.JSON' '{"destination":[{"info": {"lon":1.12345, "lat":5.4321}, "distance": 10, "name":"shanghai", "addr":"xuhui"}]}' --es "string" "This is a string" --ei "Int" 1 "
```

##  adb读写Settings值
Settings 包含 system, secure 和 global 三个表

其 Key-Value 存储在 data/system/users/0/

```
adb shell settings put system {keyname} {keyvalue}
adb shell settings get system {keyname}
```

##  adb安装卸载apk包
```
adb install -t xxx.apk
adb uninstall packagename
```

##  测试麦克风(tinycap 表示录音，ctrl+c退出，得到/data/rec.wav文件)
```
tinymix "MultiMedia5 Mixer QUIN_TDM_TX_0" 1
tinymix "QUIN_TDM_TX_0 Channels" "One"
tinycap /data/rec.wav -D 0 -d 9 -c 1 -r 16000
```

##  adb模拟按键
后退键 

```
adb shell input keyevent 4
```

更多详情见

https://www.cnblogs.com/mgzc-1508873480/p/7826967.html

##  adb截图
```
adb exec-out screencap -p > sc.png
adb shell screencap -p /sdcard/sc.png
```

##  adb录屏
```
adb shell screenrecord /sdcard/test01.mp4
```
