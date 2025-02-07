**[English Document](README_en.md)**

---

基于 iOS 14 的 `AVPictureInPictureController`，完全自定义画中画，已覆盖市面上所有画中画的核心和难点。

**iOS Android Flutter 全部平台，全部代码，全部开源！！！**


**运行环境：**

真机 & iOS14及以上

**demo展示：**


1. 悬浮提词器：

![pip](pip.gif)


2. 悬浮秒表：

![FloatTimer](悬浮秒表.PNG)

---


> **若有用，给个 star，后续会更新更多功能！**


---


## 难点和思路：

*因为 Android 的自定义画中画无任何难度，所以此处只讨论 iOS 的自定义画中画。*

### 1. 如何添加自定义 view 到画中画窗口？

你如果在画中画弹出前后将所有 window 打印出来，你就会发现，当画中画弹出后，多了一个 window，而这个 window 就是你添加自定义 view 的 window。

### 2. 如何隐藏系统的快进快退按钮、播放按钮、进度条？

`AVPictureInPictureController` 提供的接口就那几个，你尝试了所有接口都实现不了。这个时候就可以断定，这个功能是通过私有 api 来实现的了。

### 3. 如何用代码动态修改窗口的形状？

窗口的形状由视频的形状决定。

### 4. 如何用代码旋转窗口？

详见demo源码。


### 5. 如何在录视频时不暗屏？

详见demo源码。


### 6. 如何让画中画在后台一直运行？

播放无声音频。


### 7. 如何进入后台时自动开启画中画？

`AVPictuerInPictureController` 提供了一个属性：


```swift
if #available(iOS 14.2, *) {
    pipController.canStartPictureInPictureAutomaticallyFromInline = true
} else {
    // Fallback on earlier versions
}
```

注：播放器必须处于播放状态。


### 8. 如何监听画中画窗口变大变小？

KVO，监听画中画里view大小的变化；或者直接在 `layoutSubviews` 方法里处理。


### 9. 如何在画中画开启时，让 app 自动进入后台？

调用下面方法：

```swift
UIControl().sendAction(#selector(URLSessionTask.suspend), to: UIApplication.shared, for: nil)
```

### 10. 如何提升悬浮秒表的精度？

使用 GCD Timer。


### 最后，如何应对苹果的审核？

苹果可能会因为你使用了后台权限而拒绝你。

可能是因为它没看到你使用了画中画功能，录屏告诉它你使用了画中画，画中画必须依赖 Background Mode.

如果这样还不行，在你的 App 里添加视频播放功能，顺带开启视频播放器的画中画功能，有了能开启画中画的视频播放器，你就可以理所当然的使用 Background Mode 了。

如何快速添加视频播放功能？用 iOS 自带的视频播放器 class 啊。

也可以用一个 web，在 web 里放视频播放器。

视频播放器放什么？实在不知道放什么就放你们产品的画中画使用教程吧。


### 引申出的骚操作：如何给让 App 可以一直在后台运行？

- 问：如何让你开发的 App 可以一直在后台运行？
- 答：后台放无声音频就阔以咯。


- 问：后台放无声音频需要 Background Mode，审核阔能不通过。
- 答：给你的 App 一个可以正当使用 Background Mode 的理由，比如说视频播放，比如说画中画。


