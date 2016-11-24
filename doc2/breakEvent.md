## 直播中断事件的处理
在直播过程中，经常会遇到突发事件导致直播被中断。这里列举一些常见事件的处理方式供开发者参考：
>* 来电话
>* 音频中断
>* 切后台
>* 锁屏
>* 断网
>* crash
>* 被踢

### 一、来电话
无需做任何处理

来电话时，来电界面会覆盖当前直播界面，参考切后台处理

### 二、音频中断
无需做任何处理

发产生音频中断(如闹钟事件)时，分两种情况:

>* 无新界面，不影响当前播放(录制)视频及语音
>* 有新界面，参考切后处理

### 三、切后台
#### Android
Android应用在切后台时，ILiveSDK中为用户提供了三种模式:

模式名称|模式说明
:--|:--:
VIDEOMODE_BSUPPORT|支持后台模式,应用切到后台时依然默默地播放(录制)视频和语音
VIDEOMODE_NORMAL|普通模式(*默认*)，应用切到后台时，暂停播放(录制)视频和语音，回到前台时恢复
VIDEOMODE_BMUTE|后台静默模式，应用切到后台，关闭所有音视频上下行数据，回到前台时恢复

#### iOS
iOS分两种情况:
>* 界面被覆盖，此时会暂停播放(录制)视频和语音，回到前面时恢复
>* 切到后台，短时间只会暂停(同上)，长时间会被回收(系统机制)

*PS: 在直播房间时，如果90秒无上行数据，音视频房间会被后台收回*

### 四、锁屏
#### Android
同切后台，用户可以在应用中自行设置保活模式，避免锁屏
#### iOS
ILiveSDK内部设置保活模式，不会锁屏(进入房间保活，退出房间不保活)

### 五、断网
在网络中断时，SDK内部会尝试重连，用户可自行监控系统网络状态

如需了解SDK内部网络状态，可参考[ILiveQualityData](https://github.com/zhaoyang21cn/ILiveSDK_Android_Demos/blob/master/doc/ILiveSDK/quality.md)

如房间超过90秒没有上行数据，音视频房间会被回收，会上抛onRoomDisconnect事件

### 六、app crash
ILiveSDK内置了[bugly上报](https://bugly.qq.com/v2/)

### 七、被踢
多终端登录时，会收到被踢事件，此时建议用户重新登陆(如果恢复需自己记录状态)
#### Android
可以在ILiveLoginManager类调用setUserStatusListener监听强制下线事件通知

```java
public interface TILVBStatusListener {
    void onForceOffline(int error, String message);
}
```
