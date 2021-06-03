# ScreenRecord

Android屏幕录制解决方案

通过MediaProjection进行屏幕录制,支持录音，保存为MP4文件在本地。

需要设备在Android 5.0以上,详细内容见代码。

MediaProjection是Android5.0后才开放的屏幕采集接口，通过系统级服务MediaProjectionManager进行管理。

这里先整体说一下屏幕录制的流程，不然看起来费劲。

1、通过startActivityForResult(Intent intent)判断是否录屏授权的Activity

其中intent对象就需要MediaProjectionManager.createScreenCaptureIntent();获取

2、在onActivityResult回调方法中做具体录屏工作

比如：创建MediaRecorder，设置MP4文件路径
创建VirtualDisplay，设置屏幕相关参数
如果不在onActivityResult回调中执行会有问题。

3、开始录屏

MediaRecorder.start()

4、停止录屏

MediaRecorder.reset();
MediaRecorder.release();

录屏过程用到录音权限和数据读写权限。
https://www.zego.im/article/androidlupingcaiji

第一步，申请权限。在 AndroidManifest 加上申请权限的代码，因为我们需要用到音频录制。
第二步，获取系统服务。通过 MediaProjectionManager 获取一个系统服务，这个系统服务需要获取用户授权：
第三步，创建 Intent 跳转服务。MediaProjectManager 已经封装了获取 Intent 的方法 createScreenCaptureIntent， 拿到 Intent 之后，当调用 startActivityForResult 方法时，会触发一个请求授权的弹窗，当用户同意授权或者拒绝授权，都会通过 onActivityResult 返回。
第四步，监听 onActivityResult 根据用户授权返回的结果获取 MediaProjection
第五步，创建虚拟屏幕。我们已经获取到了 MediaProjection，接下来就是要创建一个虚拟屏幕——VirtualDisplay，这一步是屏幕录制的关键所在
第六步，屏幕录制保存（MediaRecorder）
![](http://ww1.sinaimg.cn/large/cfeeee4dgy1fpb7o57lvxj20b90mt74d.jpg)

