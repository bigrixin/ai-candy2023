# PotPlayer 设置

**使用GPU设置**

1. 改名 potplayermini.exe→Daum.exe
2. 设置 NVIDIA控制面板中选择：\
   管理3D设置-程序设置-添加， \
   通过浏览找到刚才改好名的Daum.exe，\
   选择为高性能NVIDIA 处理器



**图音不同步问题解决**

* 选择内置解码器/DXVA设置
* 选项—滤镜—滤镜解码器管理—视频解码器—内置解码器/DXVA设置: \
  勾选: 图像滞后时允许漏帧保证同步 \
  勾选: 使用硬件加速



* Menu-》Filter-> select Built-in Audio Codec/DXVA
* Menu->Filter->Manage filter->Video Decoder->Built-in Audio Codec/DXVA Settings: Reduce video frame rate to adapt the receiver (select) \
  Use DXVA (select)
