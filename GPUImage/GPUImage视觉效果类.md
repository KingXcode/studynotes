# 视觉效果 Visual Effect

 ```objc
 #import "GPUImageSketchFilter.h"                    //素描
 #import "GPUImageThresholdSketchFilter.h"           //阀值素描，形成有噪点的素描
 #import "GPUImageToonFilter.h"                      //卡通效果（黑色粗线描边）
 #import "GPUImageSmoothToonFilter.h"                //相比上面的效果更细腻，上面是粗旷的画风
 #import "GPUImageKuwaharaFilter.h"                  //桑原(Kuwahara)滤波,水粉画的模糊效果；处理时间比较长，慎用

 #import "GPUImageMosaicFilter.h"                    //黑白马赛克
 #import "GPUImagePixellateFilter.h"                 //像素化
 #import "GPUImagePolarPixellateFilter.h"            //同心圆像素化
 #import "GPUImageCrosshatchFilter.h"                //交叉线阴影，形成黑白网状画面
 #import "GPUImageColorPackingFilter.h"              //色彩丢失，模糊（类似监控摄像效果）

 #import "GPUImageVignetteFilter.h"                  //晕影，形成黑色圆形边缘，突出中间图像的效果
 #import "GPUImageSwirlFilter.h"                     //漩涡，中间形成卷曲的画面
 #import "GPUImageBulgeDistortionFilter.h"           //凸起失真，鱼眼效果
 #import "GPUImagePinchDistortionFilter.h"           //收缩失真，凹面镜
 #import "GPUImageStretchDistortionFilter.h"         //伸展失真，哈哈镜
 #import "GPUImageGlassSphereFilter.h"               //水晶球效果
 #import "GPUImageSphereRefractionFilter.h"          //球形折射，图形倒立

 #import "GPUImagePosterizeFilter.h"                 //色调分离，形成噪点效果
 #import "GPUImageCGAColorspaceFilter.h"             //CGA色彩滤镜，形成黑、浅蓝、紫色块的画面
 #import "GPUImagePerlinNoiseFilter.h"               //柏林噪点，花边噪点
 #import "GPUImage3x3ConvolutionFilter.h"            //3x3卷积，高亮大色块变黑，加亮边缘、线条等
 #import "GPUImageEmbossFilter.h"                    //浮雕效果，带有点3d的感觉
 #import "GPUImagePolkaDotFilter.h"                  //像素圆点花样
 #import "GPUImageHalftoneFilter.h"                  //点染,图像黑白化，由黑点构成原图的大致图形

 ```


