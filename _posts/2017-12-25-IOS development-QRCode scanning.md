---
layout: post
title:  iOS开发-二维码的扫描
date:   2017-12-25 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---


### 一、前言

最近在做一个关于扫描二维码签到的小东西，所以还是上来写一篇关于二维码的文章，网上也有一些扫描二维码的框架，例如ZXing或者ZBar。但是感觉还不如用原生的好，所以果断采用原生的了。本文介绍的二维码的扫描，就是显示二维码扫描的结果，至于链接的跳转和应用的打开，就不多说明，只要在plist文件和扫描的代理方法里面做处理就好了。
ps：

- 二维码的扫描要调用相机，模拟器是不支持相机的，所以用模拟器测试的话，是会崩溃。
- 原生的二维码扫描不支持图像识别，只支持摄像头扫描识别。

### 二、相关类的介绍

- `AVCaptureDevice`：代表抽象的硬件设备。
- `AVCaptureDeviceInput`：输入设备
- `AVCaptureMetadataOutput`：输出类，扫描的码的类型均由这个类管理。
- `AVCaptureSession`：会话对象，连接输入设备和输出设备。
- `AVCaptureVideoPreviewLayer`：图层类，将相机扫描到的图像实时显示在屏幕上。

### 三、扫描的界面的搭建

界面效果预览

![](https://upload-images.jianshu.io/upload_images/1873639-46e2e71cad352d84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/377)

- 在屏幕中央，拖了一个view，作为扫描的区域框，并设置好它的约束。

- 如果你手上的图片是下面这种的话，就可以跟我一样，添加4个imageView到扫描的区域框内，然后分别设置好它们的约束，让它们分别在扫描区域框的四个角上。

![](https://upload-images.jianshu.io/upload_images/1873639-1189a154870caf9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/390)

- 如果你手上是下面的这种图片，则需要处理一下，具体步骤如下图所示，就是对其进行一定的拉伸处理。这样，在屏幕中央，不是拖出一个view，而是拖出一个imageView，设置它的image为你的图片。

![](https://upload-images.jianshu.io/upload_images/1873639-beb102869e06d042.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

![](https://upload-images.jianshu.io/upload_images/1873639-35034bff5162d578.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/482)

![](https://upload-images.jianshu.io/upload_images/1873639-d0a78b31a9e1f741.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/265)

- 最后就是扫描区域那根线的添加了，这里我是在代码里面进行添加，并设置了相关的动画，然后在`- (void)viewWillAppear:(BOOL)animated`方法里面进行调用。具体代码如下：

```objc
/**
 *  添加扫描线以及开启扫描线的动画
 */
-(void)startAnimate {
    CGFloat scanImageViewX = self.scanView.frame.origin.x;
    CGFloat scanImageViewY = self.scanView.frame.origin.y;
    CGFloat scanImageViewW = self.scanViewWidth.constant;
    CGFloat scanImageViewH = 7;
    
    _scanImageView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"scanLine"]];
    _scanImageView.frame = CGRectMake(scanImageViewX, scanImageViewY, scanImageViewW, scanImageViewH);
    [self.scanView addSubview:_scanImageView];
    
    [UIView animateWithDuration:2.0 delay:0 options:UIViewAnimationOptionRepeat animations:^{
        _scanImageView.frame = CGRectMake(scanImageViewX, scanImageViewY + self.scanViewHeight.constant, scanImageViewW, scanImageViewH);
    } completion:nil];
}
```

### 四、具体代码

- 设备，输入源和输出源的懒加载

```objc
/**
 *  懒加载设备
 */
-(AVCaptureDevice *)device {
    if (!_device) {
        _device = [AVCaptureDevice defaultDeviceWithMediaType:AVMediaTypeVideo];
    }
    return _device;
}
/**
 *  懒加输入源
 */
-(AVCaptureDeviceInput *)input {
    if (!_input) {
        _input = [AVCaptureDeviceInput deviceInputWithDevice:self.device error:nil];
    }
    return _input;
}
/**
 *  懒加载输出源
 */
-(AVCaptureMetadataOutput *)output {
    if (!_output) {
        _output = [[AVCaptureMetadataOutput alloc] init];
        [_output setMetadataObjectsDelegate:self queue:dispatch_get_main_queue()];
        
    }
    return _output;
}
```


- 设置扫描二维码的方法，在`- (void)viewDidLoad`方法里进行调用。
    - 1、该方法里面，创建会话设备，并设置为高质量的采集，然后分别判断添加输入源和输入源到会话中。
    - 2、条码的类型，我这里直接把全部码所在的数组都放进去了，比较方便吧，当然只设置其中几种条码也可以。
    - 3、设置扫描的范围，我们下面再说。
    - 4、创建一个预览的图层，将会话作为创建的参数传入，并图层为铺满整个屏幕。
    - 5、创建一个非扫描区域的黑色蒙板图层，设置它的代理为当前的控制器，并实现它的代理方法，它的代理方法其实就是创建一个蒙板，代理方法具体的实现，待会在下面会贴出代码。


```objc
/**
 *  设置扫描二维码
 */
-(void)setupScanQRCode {
    // 1、创建设备会话对象，用来设置设备数据输入
    _session = [[AVCaptureSession alloc] init];
    [_session setSessionPreset: AVCaptureSessionPresetHigh];    //高质量采集
    
    if ([_session canAddInput:self.input]) {
        [_session addInput:self.input];
    }
    if ([_session canAddOutput:self.output]) {
        [_session addOutput:self.output];
    }
    // 2.设置条码类型为二维码
    [self.output setMetadataObjectTypes:self.output.availableMetadataObjectTypes];
    
    // 3.设置扫描范围
    [self setOutputInterest];
    
    // 4、实时获取摄像头原始数据显示在屏幕上
    _preview = [AVCaptureVideoPreviewLayer layerWithSession:_session];
    _preview.videoGravity = AVLayerVideoGravityResizeAspectFill;
    _preview.frame = self.view.layer.bounds;
    self.view.layer.backgroundColor = [[UIColor blackColor] CGColor];
    [self.view.layer insertSublayer:_preview atIndex:0];
    
    // 5.设置非扫描区域的黑色蒙版图层
    self.maskLayer = [[CALayer alloc]init];
    self.maskLayer.frame = self.view.layer.bounds;
    self.maskLayer.delegate = self;
    [self.view.layer insertSublayer:self.maskLayer above:_preview];
    [self.maskLayer setNeedsDisplay];
}
```


- 设置扫描范围

关于扫描范围，这是一个坑，稍稍不注意，就会踩进去了。扫描的范围是通过这个参数`rectOfInterest`来设置的，但这个参数不是普通的`CGRect`，而是0~1的一个范围比例。正确的创建为`CGRectMake(y/Height,x/Width,height/Height,width/Width)，`这里左边是扫描区域的`x，y，width，height，`右边的是当前控制器view的Width和Height。具体的代码实现如下：

```objc
/**
 *  设置二维码的扫描范围
 */
-(void)setOutputInterest {
    CGSize size = self.view.bounds.size;
    CGFloat scanViewWidth = 240;
    CGFloat scanViewHeight = 240;
    CGFloat scanViewX = (size.width - scanViewWidth) / 2;
    CGFloat scanViewY = (size.height - scanViewHeight) / 2;
    CGFloat p1 = size.height/size.width;
    CGFloat p2 = 1920./1080.;
    if (p1 < p2) {
        CGFloat fixHeight = self.view.bounds.size.width * 1920. / 1080.;
        CGFloat fixPadding = (fixHeight - size.height)/2;
        _output.rectOfInterest = CGRectMake((scanViewY + fixPadding) / fixHeight,
                                            scanViewX / size.width,
                                            scanViewHeight / fixHeight,
                                            scanViewWidth / size.width);
    } else {
        CGFloat fixWidth = self.view.bounds.size.height * 1080. / 1920.;
        CGFloat fixPadding = (fixWidth - size.width)/2;
        _output.rectOfInterest = CGRectMake(scanViewY / size.height,
                                            (scanViewX + fixPadding) / fixWidth,
                                            scanViewHeight / size.height,
                                            scanViewWidth / fixWidth);
    }
}
```

- 蒙板的代理方法如下：

```objc
/**
 *   蒙板生成,需设置代理，并在退出页面时取消代理
 */
-(void)drawLayer:(CALayer *)layer inContext:(CGContextRef)ctx{
    if (layer == self.maskLayer) {
        UIGraphicsBeginImageContextWithOptions(self.maskLayer.frame.size, NO, 1.0);
        CGContextSetFillColorWithColor(ctx, [UIColor colorWithRed:0 green:0 blue:0 alpha:0.6].CGColor);
        CGContextFillRect(ctx, self.maskLayer.frame);
        CGRect scanFrame = [self.view convertRect:self.scanView.frame fromView:self.scanView.superview];
        CGContextClearRect(ctx, scanFrame);
    }
}
```

- 二维码扫描的回调方法的具体实现如下：这里我用了一个遮盖的框架`SVProgressHUD`，模拟一下扫描的等待过程，瞎装一下。

```objc
-void)captureOutput:(AVCaptureOutput *)captureOutput didOutputMetadataObjects:(NSArray *)metadataObjects fromConnection:(AVCaptureConnection *)connection {
    NSString *stringValue;
    
    // 显示遮盖
    [SVProgressHUD show];
    
    if ([metadataObjects count ] > 0 ) {
        // 当扫描到数据时，停止扫描
        [ _session stopRunning ];
        
        // 将扫描的线从父控件中移除
        [_scanImageView removeFromSuperview];
        
        AVMetadataMachineReadableCodeObject * metadataObject = [metadataObjects objectAtIndex : 0 ];
        
        stringValue = metadataObject. stringValue ;
    }
    // 当前延迟1.0秒
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        // 隐藏遮盖
        [SVProgressHUD dismiss];
        
        // 将扫描后的结果显示在label上
        self.scanResult.text = stringValue;
    });
}
```

- 这里，附带一下方法的代用和开始扫描方法的调用

```objc
-(void)viewDidLoad {
    [super viewDidLoad];
    
    // 设置扫描二维码
    [self setupScanQRCode];
}
-(void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    
    // 添加扫描线以及开启扫描线的动画
    [self startAnimate];
    
    // 开启二维码扫描
    [_session startRunning];
}
-(void)dealloc{
    // 删除预览图层
    if (_preview) {
        [_preview removeFromSuperlayer];
    }
    if (self.maskLayer) {
        self.maskLayer.delegate = nil;
    }
}
```


### 五、以上就是所有的代码了，那么看一下运行的结果吧，这里我只扫描了2二维码和条形码。

ps：截gif的时候，出了点差错，勿怪啊。


![](https://upload-images.jianshu.io/upload_images/1873639-aea5ef4f1d683b30.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/448)

![](https://upload-images.jianshu.io/upload_images/1873639-2f23daec13115f7a.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/448)

### 六、总结

- 苹果原生的二维码，iOS 7开始有了，到现在网上已经有很多大牛写了技术博客和文章，要学习起来还是非常简单的。在我看来，就设置扫描范围那里有点坑，需要注意。最后，附上demo吧，因为是Xcode 8创建的项目，Xcode 7的小伙伴就慎重下载吧，因为Xcode 7是没办法打开Xcode 8创建的xib文件的。

- 特别说明：
iOS 10 的权限问题，因为iOS 10 对权限的要求更高了，如果我们不设置相应权限问题的话，会直接导致程序崩溃。这时，我们需要在plist文件中加上相机权限的描述

```objc
<key>NSCameraUsageDescription</key>
 <string>cameraDesciption</string>
```


