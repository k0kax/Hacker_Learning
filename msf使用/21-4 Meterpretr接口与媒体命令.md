# Meterpreter-接口与媒体命令

接口命令主要针对鼠标键盘进行监控。

`    enumdesktops   List all accessible desktops and window stations//列出存在的存在的桌面和窗口
    getdesktop     Get the current meterpreter desktop
    idletime       Returns the number of seconds the remote user has been idle
    keyboard_send  Send keystrokes//发送键盘活动 不太好用啊
    keyevent       Send key events//键盘活动，不太好用啊`    

## 键盘嗅探
   ` keyscan_dump   Dump the keystroke buffer//加载键盘缓存记录
    keyscan_start  Start capturing keystrokes//开启键盘记录，会将记录存入缓存
    keyscan_stop   Stop capturing keystrokes//停止键盘记录` 

如果我们要对管理员的键盘进行记录的话，就需要把进程1096迁移到Administrator的进程516

在system权限下，是无法捕获Administrator的键盘记录.

![](C:\Users\Aurora\OneDrive\桌面\Metasploit\21.4.0.png)

键盘记录需要谨慎添加，避免杀软。

## 鼠标

`    mouse          Send mouse events//mouse操作事件。主要用于对鼠标进行操作，同上需要切换pid。`    

`Usage: mouse action (move移动, click点击, up点击后弹起, down按下, rightclick右键单击, rightup右键点击后抬起, rightdown右键按下, doubleclick双击)`

`以左上角为坐标原点，建立直角坐标系，x0-1024 y0-867,以屏幕像素值为主。
       mouse [x] [y] (click)
       mouse [action] [x] [y]
  e.g: mouse click
       mouse rightclick 1 1
       mouse move 640 480`

## 屏幕

​    `screenshare    Watch the remote user desktop in real time//屏幕共享，会生成一个web网页。
​    screenshot     Grab a screenshot of the interactive desktop//屏幕拍照
​    setdesktop     Change the meterpreters current desktop
​    uictl          Control some of the user interface component`

## 摄像头与麦克风

 `record_mic     Record audio from the default microphone for X seconds`

`  -d   Number of seconds to record (Default: 1)//记录时间，默认1s
    -f   The wav file path (Default: '/root/桌面/[randomname].wav')//记录的保存位置
    -h   Help Banner
    -p   Automatically play the captured audio (Default: 'true')  //播放已经抓取的音频`

`
    webcam_chat    Start a video chat//视频通话。
    webcam_list    List webcams//摄像头查看
    webcam_snap    Take a snapshot from the specified webcam//拍照
    webcam_stream  Play a video stream from the specified webcam//直播 相关参数显示得有摄像头`

## 音频播放

直接在目标上播放本地音频

play 本地音频路径

## 提权

getsystem









