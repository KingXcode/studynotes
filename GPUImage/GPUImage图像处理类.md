# 图像处理 Handle Image

```objc

 #import "GPUImageCrosshairGenerator.h"              //十字
 #import "GPUImageLineGenerator.h"                   //线条


 #import "GPUImageTransformFilter.h"                 //形状变化
 #import "GPUImageCropFilter.h"                      //剪裁
 #import "GPUImageSharpenFilter.h"                   //锐化
 #import "GPUImageUnsharpMaskFilter.h"               //反遮罩锐化


 #import "GPUImageFastBlurFilter.h"                  //模糊
 #import "GPUImageGaussianBlurFilter.h"              //高斯模糊
 #import "GPUImageGaussianSelectiveBlurFilter.h"     //高斯模糊，选择部分清晰
 #import "GPUImageBoxBlurFilter.h"                   //盒状模糊
 #import "GPUImageTiltShiftFilter.h"                 //条纹模糊，中间清晰，上下两端模糊
 #import "GPUImageMedianFilter.h"                    //中间值，有种稍微模糊边缘的效果
 #import "GPUImageBilateralFilter.h"                 //双边模糊
 #import "GPUImageErosionFilter.h"                   //侵蚀边缘模糊，变黑白
 #import "GPUImageRGBErosionFilter.h"                //RGB侵蚀边缘模糊，有色彩
 #import "GPUImageDilationFilter.h"                  //扩展边缘模糊，变黑白
 #import "GPUImageRGBDilationFilter.h"               //RGB扩展边缘模糊，有色彩
 #import "GPUImageOpeningFilter.h"                   //黑白色调模糊
 #import "GPUImageRGBOpeningFilter.h"                //彩色模糊
 #import "GPUImageClosingFilter.h"                   //黑白色调模糊，暗色会被提亮
 #import "GPUImageRGBClosingFilter.h"                //彩色模糊，暗色会被提亮
 #import "GPUImageLanczosResamplingFilter.h"         //Lanczos重取样，模糊效果
 #import "GPUImageNonMaximumSuppressionFilter.h"     //非最大抑制，只显示亮度最高的像素，其他为黑
 #import "GPUImageThresholdedNonMaximumSuppressionFilter.h" //与上相比，像素丢失更多


 #import "GPUImageSobelEdgeDetectionFilter.h"        //Sobel边缘检测算法(白边，黑内容，有点漫画的反色效果)
 #import "GPUImageCannyEdgeDetectionFilter.h"        //Canny边缘检测算法（比上更强烈的黑白对比度）
 #import "GPUImageThresholdEdgeDetectionFilter.h"    //阈值边缘检测（效果与上差别不大）
 #import "GPUImagePrewittEdgeDetectionFilter.h"      //普瑞维特(Prewitt)边缘检测(效果与Sobel差不多，貌似更平滑)
 #import "GPUImageXYDerivativeFilter.h"              //XYDerivative边缘检测，画面以蓝色为主，绿色为边缘，带彩色
 #import "GPUImageHarrisCornerDetectionFilter.h"     //Harris角点检测，会有绿色小十字显示在图片角点处
 #import "GPUImageNobleCornerDetectionFilter.h"      //Noble角点检测，检测点更多
 #import "GPUImageShiTomasiFeatureDetectionFilter.h" //ShiTomasi角点检测，与上差别不大
 #import "GPUImageMotionDetector.h"                  //动作检测
 #import "GPUImageHoughTransformLineDetector.h"      //线条检测
 #import "GPUImageParallelCoordinateLineTransformFilter.h" //平行线检测
 #import "GPUImageLocalBinaryPatternFilter.h"        //图像黑白化，并有大量噪点
 #import "GPUImageLowPassFilter.h"                   //用于图像加亮
 #import "GPUImageHighPassFilter.h"                  //图像低于某值时显示为黑

```


