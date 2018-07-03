# GPUImageDemo示例功能

1.BenchmarkSuite：GPUImage与基于CPU实现的图片处理效果以及CoreImage做的处理效率比较。

2.ColorObjectTracking：

3.CubeExample：说明GPUImage与OpenGLES渲染的相互关系。给摄像头获取到的每一帧内容加上sepia（乌贼色效果？）的滤镜并且显示在一个立方体的表面，可以用手机旋转立方体。立方体被渲染成一个texture-backed（没懂）的frambuffer对象，再反馈给GPUImage进行马赛克处理后显示。

4.FeatureExtractionTest

5.FilterShowcase：GPUImage内建滤镜的全部展示，这个demo中的判断语句优点复杂。

6.MultiViewFilterExample：摄像头实时滤镜效果，多个滤镜叠加，并且其中两个是自定义滤镜。

7.RawDataTest：

8.SimpleImageFilter：对一张静态图片进行滤镜操作，保存到disk。

9.SimplePhotoFilter：

10.SimpleVideoFileFilter：对disk中的视频文件进行虚化处理，并保存为另一个视频文件。

11.SimpleVideoFilter：马赛克录像，滑杆可调整马赛克颗粒大小。








