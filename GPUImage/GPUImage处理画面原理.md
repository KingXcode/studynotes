# GPUImage处理画面原理

- GPUImage采用链式方式来处理画面,通过addTarget:方法为链条添加每个环节的对象，处理完一个target,就会把上一个环节处理好的图像数据传递下一个target去处理，称为GPUImage处理链。
    - 比如：墨镜原理，从外界传来光线，会经过墨镜过滤，在传给我们的眼睛，就能感受到大白天也是乌黑一片。
    - 一般的target可分为两类
        - 中间环节的target, 一般是各种filter, 是GPUImageFilter或者是子类。
        - 最终环节的target, GPUImageView：用于显示到屏幕上, 或者GPUImageMovieWriter：写成视频文件。
- GPUImage处理主要分为3个环节。
    > source(视频、图片源) -> filter（滤镜） -> final target (处理后视频、图片)
    - GPUImaged的Source:都继承GPUImageOutput的子类，作为GPUImage的数据源,就好比外界的光线，作为眼睛的输出源。
        - `GPUImageVideoCamera：`用于实时拍摄视频
        - `GPUImageStillCamera：`用于实时拍摄照片
        - `GPUImagePicture：`用于处理已经拍摄好的图片，比如png,jpg图片
        - `GPUImageMovie：`用于处理已经拍摄好的视频,比如mp4文件
        - GPUImageTextureInput：
        - GPUImageRawDataInput：
        - GPUImageUIElement：
        
    - GPUImage的filter，GPUimageFilter类或者子类，这个类继承自GPUImageOutput,并且遵守GPUImageInput协议，这样既能流进，又能流出，就好比我们的墨镜，光线通过墨镜的处理，最终进入我们眼睛
    - GPUImage的`final target:`，GPUImageView,GPUImageMovieWriter,GPUImageTextureOutput,GPUImageRawDataOutput就好比我们眼睛，最终输入目标。

