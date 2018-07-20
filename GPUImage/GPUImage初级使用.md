#### 初级使用（一）

- 给image添加滤镜，然后显示到UIImageView上

```objc

self.mGPUImageView = [[GPUImageView alloc]init];
self.mGPUImageView.fillMode = kGPUImageFillModeStretch;

GPUImageFilter *filter = [[GPUImageWeakPixelInclusionFilter alloc] init];

UIImage *image = [UIImage imageNamed:@"face"];

self.mGPUImageView.image = [filter imageByFilteringImage:image];

```

#### 初级使用（二）

- 从摄像头获取数据源，并且添加滤镜，展示在UIImageView上；无录像

```objc
//1. 创建
self.mGPUVideoCamera = [[GPUImageVideoCamera alloc] initWithSessionPreset:AVCaptureSessionPreset1920x1080 cameraPosition:AVCaptureDevicePositionBack];

self.mGPUImageView = [[GPUImageView alloc]init];
self.mGPUImageView.fillMode = kGPUImageFillModeStretch;

//2.mGPUVideoCamera -> filter -> mGPUImageView
//添加响应链
GPUImageSepiaFilter* filter = [[GPUImageSepiaFilter alloc] init];
[self.mGPUVideoCamera addTarget:filter];
[filter addTarget:self.mGPUImageView];

//3.启动相机
[self.mGPUVideoCamera startCameraCapture];

//可有可无
//监听屏幕方向，修改摄像头的输出方向
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(deviceOrientationDidChange:) name:UIDeviceOrientationDidChangeNotification object:nil];
- (void)deviceOrientationDidChange:(NSNotification *)notification {
    UIInterfaceOrientation orientation = (UIInterfaceOrientation)[UIDevice currentDevice].orientation;
    self.mGPUVideoCamera.outputImageOrientation = orientation;
}

```

#### 初级使用（三）

- 录制视频并保存

```objc
//1.创建
self.videoCamera = [[GPUImageVideoCamera alloc] initWithSessionPreset:AVCaptureSessionPreset640x480 cameraPosition:AVCaptureDevicePositionFront];
self.videoCamera.outputImageOrientation = UIInterfaceOrientationPortrait;
self.videoCamera.horizontallyMirrorFrontFacingCamera = YES;
self.filterView = [[GPUImageView alloc] initWithFrame:self.view.frame];
self.filterView.center = self.view.center;
[self.view addSubview:self.filterView];

//2.设置保存路径
NSString *pathToMovie = [NSHomeDirectory() stringByAppendingPathComponent:@"Documents/Movie.m4v"];
unlink([pathToMovie UTF8String]);//删除路径下的文件
NSURL *movieURL = [NSURL fileURLWithPath:pathToMovie];

//3. 添加响应链
//videoCamera -> beautifyFilter -> self.filterView
                                -> _movieWriter
_movieWriter = [[GPUImageMovieWriter alloc] initWithMovieURL:movieURL size:CGSizeMake(640.0, 480.0)];
    
self.videoCamera.audioEncodingTarget = _movieWriter;

_movieWriter.encodingLiveVideo = YES;

[self.videoCamera startCameraCapture];

GPUImageBeautifyFilter *beautifyFilter = [[GPUImageBeautifyFilter alloc] init];

[self.videoCamera addTarget:beautifyFilter];

[beautifyFilter addTarget:self.filterView];

[beautifyFilter addTarget:_movieWriter];

[_movieWriter startRecording];

```
#### 初级使用（四）
- 给GPUImage 添加滤镜组<转>

```objc

// 图片输入源
_inputImage = [UIImage imageNamed:@"icon"];
 
// 初始化 picture
_picture    = [[GPUImagePicture alloc] initWithImage:_inputImage smoothlyScaleOutput:YES];
 
// 初始化 imageView
_imageView  = [[GPUImageView alloc] initWithFrame:self.iconImageView.bounds];
[self.iconImageView addSubview:_imageView];
 
// 初始化 filterGroup
_filterGroup = [[GPUImageFilterGroup alloc] init];
[_picture addTarget:_filterGroup];
 
 
// 添加 filter
/**
原理：
1. filterGroup(addFilter) 滤镜组添加每个滤镜
2. 按添加顺序（可自行调整）前一个filter(addTarget) 添加后一个filter
3. filterGroup.initialFilters = @[第一个filter]];
4. filterGroup.terminalFilter = 最后一个filter;
*/
GPUImageRGBFilter *filter1         = [[GPUImageRGBFilter alloc] init];
GPUImageToonFilter *filter2        = [[GPUImageToonFilter alloc] init];
GPUImageColorInvertFilter *filter3 = [[GPUImageColorInvertFilter alloc] init];
GPUImageSepiaFilter       *filter4 = [[GPUImageSepiaFilter alloc] init];
[self addGPUImageFilter:filter1];
[self addGPUImageFilter:filter2];
[self addGPUImageFilter:filter3];
[self addGPUImageFilter:filter4];
 
// 处理图片
[_picture processImage];
[_filterGroup useNextFrameForImageCapture];
 
```

```

- (void)addGPUImageFilter:(GPUImageOutput<GPUImageInput> *)filter
{
    [_filterGroup addFilter:filter];
 
    GPUImageOutput<GPUImageInput> *newTerminalFilter = filter;
 
    NSInteger count = _filterGroup.filterCount;
 
    if (count == 1)
    {
        _filterGroup.initialFilters = @[newTerminalFilter];
        _filterGroup.terminalFilter = newTerminalFilter;
 
    } else
    {
        GPUImageOutput<GPUImageInput> *terminalFilter    = _filterGroup.terminalFilter;
        NSArray *initialFilters                          = _filterGroup.initialFilters;
 
        [terminalFilter addTarget:newTerminalFilter];
 
        _filterGroup.initialFilters = @[initialFilters[0]];
        _filterGroup.terminalFilter = newTerminalFilter;
    }

```

#####上面这几个例子其实表示的很清楚了，GPUImage的常规使用方式就是`source(视频、图片源) -> filter（滤镜） -> final target (处理后视频、图片)`按照这一套流程来的

