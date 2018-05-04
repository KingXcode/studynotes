>用AVFoudation进行视频录制相对UIImagePickerController来说复杂，涉及到的相关类也更多，打个比喻，就是我想在要用积木搭房子，但是我现在只有一堆非常零碎的零件来进行组装。如下，要进行视频录制并且保存我总结了一个相关步骤。


- 第一步：导入相关的头文件`#import <AVFoundation/AVFoundation.h>`

- 第二步：创建`session`会话对象，并且设置捕获质量

- 第三步：创建对应媒体类型的`device`对象和`deviceInput`对象

- 第四步：将对应的`input`对象添加进入`session`中

- 第五步：创建文件输出对象`AVCaptureMovieFileOutput`，并且将该对象添加进`session`中的`output`中。

至此，设备初始化的工作完成。

- 第六步：根据`session`创建`AVCaptureVideoPreviewLayer`层，并且设置好`frame`

- 第七步：将创建好的`layer`添加到相应的`view`上去。此时`session`调用`startRunning`，就能将摄像头捕捉到的影像在`layer`中显示。

- 第八步：定义好`fileurl`用作存储录制视频的路径。`fileURLWithPath：`记住一定是用这个方法创建的`url`。否者会异常。

- 第九步：`AVCaptureMovieFileOutput`的实例调用`startRecordingToOutputFileURL:recordingDelegate:`方法开始讲摄像头捕捉的视频保存进之前设定好的`fielurl`



### 代码：
- 首先定义session和output
```
@property (nonatomic,strong) AVCaptureSession *session;
@property (nonatomic,strong) AVCaptureMovieFileOutput *output;
```

- 然后实现对应方法：

```
self.session = [[AVCaptureSession alloc]init];

//设置捕获视频的质量
self.session.sessionPreset = AVCaptureSessionPresetHigh;
NSError *error;

//指定媒体类型为 视频 。获得捕获设备AVCaptureDevice对象。
AVCaptureDevice *cameraDevice = [AVCaptureDevice defaultDeviceWithMediaType:AVMediaTypeVideo];

//实例化捕获输入AVCaptureDeviceInput（视频）对象，通过对应的device对象实例化。
AVCaptureDeviceInput *camera = [AVCaptureDeviceInput deviceInputWithDevice:cameraDevice error:&error];

//指定媒体类型为 音频 。获得捕获设备AVCaptureDevice对象。
AVCaptureDevice *micDevice = [AVCaptureDevice defaultDeviceWithMediaType:AVMediaTypeAudio];

//实例化捕获输入AVCaptureDeviceInput（音频）对象，通过对应的device对象实例化。
AVCaptureDeviceInput *mic = [AVCaptureDeviceInput deviceInputWithDevice:micDevice error:&error];

/*
 当前获得了两个输入对象，一个是camera设备，一个是mic设备
 */
[self.session addInput:camera];
[self.session addInput:mic];

//创建文件输出对象
self.output = [[AVCaptureMovieFileOutput alloc]init];
if ([self.session canAddOutput:self.output]) {
    [self.session addOutput:self.output];
}

//创建layer层对象，参数是captureSession。并且设置layer的frame。然后天骄到相应的view上
AVCaptureVideoPreviewLayer *previewLayer = [AVCaptureVideoPreviewLayer layerWithSession:self.session];
previewLayer.frame = CGRectMake(0, 0, 375.0, 375.0*16/9);
[self.view.layer insertSublayer:previewLayer atIndex:0];
[self.session startRunning];

```

到此为止，当前界面的view上能输出摄像头捕捉到的数据。

- 接着定义好fileUrl：
```
-(NSURL *)fileUrl
{
    NSString *outputPath = [NSString stringWithFormat:@"%@%@",NSTemporaryDirectory(),@"myvideo.mov"];
    NSURL *urlPath = [NSURL fileURLWithPath:outputPath];//[NSURL URLWithString:outputPath];
    NSFileManager *manager = [NSFileManager defaultManager];
    if ([manager fileExistsAtPath:outputPath]) {
        NSError *error;
        [manager removeItemAtPath:outputPath error:&error];
    }
    return urlPath;
}
```

- 然后调用下述方法开始讲捕捉到的数据保存进上述的fileurl中：  
`[self.output startRecordingToOutputFileURL:fileUrl recordingDelegate:self];`

- 设置此方法后，系统会持续的将捕捉到的视频缓存起来，在通过调用：   
`[self.output stopRecording];`  
结束视频的录制，此时会调用下述代理方法     
`- (void)captureOutput:(AVCaptureFileOutput *)captureOutput didFinishRecordingToOutputFileAtURL:(NSURL *)outputFileURL fromConnections:(NSArray *)connections error:(NSError *)error;`

- 在这个代理方法内部实现保存的方法：
```
if (error == nil) {
    ALAssetsLibrary *library = [[ALAssetsLibrary alloc]init];
    [library writeVideoAtPathToSavedPhotosAlbum:outputFileURL completionBlock:^(NSURL *assetURL, NSError *error) {
        NSLog(@"%@",assetURL.absoluteString);
        if (error) {
            NSLog(@"%@",error.localizedDescription);
        }
    }];
}
```
