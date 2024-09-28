Android 安卓开发
===
[TOC]

## Activity 活动  / 界面
类似 html + js，这里是 XML + Java

在 Java 中，也是使用 findViewById 来命令式寻找 view，然后实现接口的回调，设置监听器。

发生 ConfigurationChange 时，Activity 可能会重新创建，导致临时数据丢失。

## Manifest
所有 activity 需要在 manifest 里注册


## Intent 意图
跨 Avtivity 互相调用需要 Intent

例如，可以通过 action Intent 启动其他 activity

## Application
每个应用有一个 Application, 多个 Activity

Application 也有生命周期函数


## ContentProvider
```
content://<authority>/<data_path>/id
```

![](https://notes.sjtu.edu.cn/uploads/upload_3acfb18188e5984aabf71de0eba2a733.png)



组件
====

## Layout
#### LinearLayout
#### GridLayout
#### RelativeLayout

## Button
按 500ms 分为 Click 和 LongClick。同样可以设置点击事件，disable 等等

TO BE CONTINUED