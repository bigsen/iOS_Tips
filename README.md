``***
![](http://upload-images.jianshu.io/upload_images/790890-e9aee1a12c8b272a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**1，打印View所有子视图**
```
po [[self view]recursiveDescription]
```
**2，layoutSubviews调用的调用时机**
```
* 当视图第一次显示的时候会被调用。
* 添加子视图也会调用这个方法。
* 当本视图的大小发生改变的时候是会调用的。
* 当子视图的frame发生改变的时候是会调用的。
* 当删除子视图的时候是会调用的.
```
**3，NSString过滤特殊字符**
```
// 定义一个特殊字符的集合
NSCharacterSet *set = [NSCharacterSet characterSetWithCharactersInString:
@"@／：；（）¥「」＂、[]{}#%-*+=_\\|~＜＞$€^•'@#$%^&*()_+'\""];
// 过滤字符串的特殊字符
NSString *newString = [trimString stringByTrimmingCharactersInSet:set];
```
**4，TransForm属性**
```
//平移按钮
CGAffineTransform transForm = self.buttonView.transform;
self.buttonView.transform = CGAffineTransformTranslate(transForm, 10, 0);

//旋转按钮
CGAffineTransform transForm = self.buttonView.transform;
self.buttonView.transform = CGAffineTransformRotate(transForm, M_PI_4);

//缩放按钮
self.buttonView.transform = CGAffineTransformScale(transForm, 1.2, 1.2);

//初始化复位
self.buttonView.transform = CGAffineTransformIdentity;
```
**5，去掉分割线多余15像素**
```
首先在viewDidLoad方法加入以下代码：
 if ([self.tableView respondsToSelector:@selector(setSeparatorInset:)]) {
        [self.tableView setSeparatorInset:UIEdgeInsetsZero];    
}   
if ([self.tableView respondsToSelector:@selector(setLayoutMargins:)]) {        
[self.tableView setLayoutMargins:UIEdgeInsetsZero];
}
然后在重写willDisplayCell方法
- (void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell 
forRowAtIndexPath:(NSIndexPath *)indexPath{   
if ([cell respondsToSelector:@selector(setSeparatorInset:)]) {       
[cell setSeparatorInset:UIEdgeInsetsZero];    
}    
if ([cell respondsToSelector:@selector(setLayoutMargins:)]) {        
[cell setLayoutMargins:UIEdgeInsetsZero];    
}
}
```
**6，计算方法耗时时间间隔**
```
// 获取时间间隔
#define TICK   CFAbsoluteTime start = CFAbsoluteTimeGetCurrent();
#define TOCK   NSLog(@"Time: %f", CFAbsoluteTimeGetCurrent() - start)
```
**7，Color颜色宏定义**
```
// 随机颜色
#define RANDOM_COLOR [UIColor colorWithRed:arc4random_uniform(256) / 255.0 green:arc4random_uniform(256) / 255.0 blue:arc4random_uniform(256) / 255.0 alpha:1]
// 颜色(RGB)
#define RGBCOLOR(r, g, b) [UIColor colorWithRed:(r)/255.0f green:(g)/255.0f blue:(b)/255.0f alpha:1]
// 利用这种方法设置颜色和透明值，可不影响子视图背景色
#define RGBACOLOR(r, g, b, a) [UIColor colorWithRed:(r)/255.0f green:(g)/255.0f blue:(b)/255.0f alpha:(a)]
```
**8，Alert提示宏定义**
```
#define Alert(_S_, ...) [[[UIAlertView alloc] initWithTitle:@"提示" message:[NSString stringWithFormat:(_S_), ##__VA_ARGS__] delegate:nil cancelButtonTitle:@"确定" otherButtonTitles:nil] show]
```
**8，让 iOS 应用直接退出**
```
- (void)exitApplication {
    AppDelegate *app = [UIApplication sharedApplication].delegate;
    UIWindow *window = app.window;

    [UIView animateWithDuration:1.0f animations:^{
        window.alpha = 0;
    } completion:^(BOOL finished) {
        exit(0);
    }];
}
```
**8，NSArray 快速求总和 最大值 最小值 和 平均值  **
```
NSArray *array = [NSArray arrayWithObjects:@"2.0", @"2.3", @"3.0", @"4.0", @"10", nil];
CGFloat sum = [[array valueForKeyPath:@"@sum.floatValue"] floatValue];
CGFloat avg = [[array valueForKeyPath:@"@avg.floatValue"] floatValue];
CGFloat max =[[array valueForKeyPath:@"@max.floatValue"] floatValue];
CGFloat min =[[array valueForKeyPath:@"@min.floatValue"] floatValue];
NSLog(@"%f\n%f\n%f\n%f",sum,avg,max,min);
```
**9，修改Label中不同文字颜色**
```
- (void)touchesEnded:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
{
    [self editStringColor:self.label.text editStr:@"好" color:[UIColor blueColor]];
}

- (void)editStringColor:(NSString *)string editStr:(NSString *)editStr color:(UIColor *)color {
    // string为整体字符串, editStr为需要修改的字符串
    NSRange range = [string rangeOfString:editStr];

    NSMutableAttributedString *attribute = [[NSMutableAttributedString alloc] initWithString:string];

    // 设置属性修改字体颜色UIColor与大小UIFont
    [attribute addAttributes:@{NSForegroundColorAttributeName:color} range:range];

    self.label.attributedText = attribute;
}
```
**10，播放声音**
```
#import<AVFoundation/AVFoundation.h>
//  1.获取音效资源的路径
   NSString *path = [[NSBundle mainBundle]pathForResource:@"pour_milk" ofType:@"wav"];
//  2.将路劲转化为url
   NSURL *tempUrl = [NSURL fileURLWithPath:path];
//  3.用转化成的url创建一个播放器
   NSError *error = nil;
   AVAudioPlayer *play = [[AVAudioPlayer alloc]initWithContentsOfURL:tempUrl error:&error];
   self.player = play;
//  4.播放
[play play];
```
**11，检测是否IPad Pro和其它设备型号**
```
- (BOOL)isIpadPro
{   
UIScreen *Screen = [UIScreen mainScreen];   
CGFloat width = Screen.nativeBounds.size.width/Screen.nativeScale;  
 CGFloat height = Screen.nativeBounds.size.height/Screen.nativeScale;         
BOOL isIpad =[[UIDevice currentDevice] userInterfaceIdiom] == UIUserInterfaceIdiomPad;   
BOOL hasIPadProWidth = fabs(width - 1024.f) < DBL_EPSILON;    
BOOL hasIPadProHeight = fabs(height - 1366.f) < DBL_EPSILON;  
return isIpad && hasIPadProHeight && hasIPadProWidth;
}

#define UI_IS_LANDSCAPE ([UIDevice currentDevice].orientation == UIDeviceOrientationLandscapeLeft || [UIDevice currentDevice].orientation == UIDeviceOrientationLandscapeRight)#define UI_IS_IPAD ([[UIDevice currentDevice] userInterfaceIdiom] == UIUserInterfaceIdiomPad)#define UI_IS_IPHONE ([[UIDevice currentDevice] userInterfaceIdiom] == UIUserInterfaceIdiomPhone)#define UI_IS_IPHONE4 (UI_IS_IPHONE && [[UIScreen mainScreen] bounds].size.height < 568.0)#define UI_IS_IPHONE5 (UI_IS_IPHONE && [[UIScreen mainScreen] bounds].size.height == 568.0)#define UI_IS_IPHONE6 (UI_IS_IPHONE && [[UIScreen mainScreen] bounds].size.height == 667.0)#define UI_IS_IPHONE6PLUS (UI_IS_IPHONE && [[UIScreen mainScreen] bounds].size.height == 736.0 || [[UIScreen mainScreen] bounds].size.width == 736.0) // Both orientations#define UI_IS_IOS8_AND_HIGHER ([[UIDevice currentDevice].systemVersion floatValue] >= 8.0)

文／Originalee（简书作者）原文链接：http://www.jianshu.com/p/9d36aa12429f著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。
```
**11，修改Tabbar Item的属性**
```
    // 修改标题位置
    self.tabBarItem.titlePositionAdjustment = UIOffsetMake(0, -10);
    // 修改图片位置
    self.tabBarItem.imageInsets = UIEdgeInsetsMake(-3, 0, 3, 0);

// 批量修改属性
for (UIBarItem *item in self.tabBarController.tabBar.items) {
        [item setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:
                 [UIFont fontWithName:@"Helvetica" size:19.0], NSFontAttributeName, nil]
                            forState:UIControlStateNormal];
    }

// 设置选中和未选中字体颜色
    [[UITabBar appearance] setShadowImage:[[UIImage alloc] init]];

    //未选中字体颜色
    [[UITabBarItem appearance] setTitleTextAttributes:@{NSForegroundColorAttributeName:[UIColor greenColor]} forState:UIControlStateNormal];

    //选中字体颜色
    [[UITabBarItem appearance] setTitleTextAttributes:@{NSForegroundColorAttributeName:[UIColor cyanColor]} forState:UIControlStateSelected];
```

**12，NULL - nil - Nil - NSNULL的区别**
```
* nil是OC的，空对象，地址指向空（0）的对象。对象的字面零值

* Nil是Objective-C类的字面零值

* NULL是C的，空地址，地址的数值是0，是个长整数

* NSNull用于解决向NSArray和NSDictionary等集合中添加空值的问题
```
**11，去掉BackBarButtonItem的文字**
```
[[UIBarButtonItem appearance] setBackButtonTitlePositionAdjustment:UIOffsetMake(0, -60)
                                                         forBarMetrics:UIBarMetricsDefault];
```
**12，控件不能交互的一些原因**
```
1，控件的userInteractionEnabled = NO
2，透明度小于等于0.01，aplpha
3，控件被隐藏的时候，hidden = YES
4，子视图的位置超出了父视图的有效范围，子视图无法交互，设置了。
5，需要交互的视图，被其他视图盖住（其他视图开启了用户交互）。
```
**12，修改UITextField中Placeholder的文字颜色**
```
[text setValue:[UIColor redColor] forKeyPath:@"_placeholderLabel.textColor"];
}
```
**13，视图的生命周期**
```
1、 alloc 创建对象，分配空间
2、 init (initWithNibName) 初始化对象，初始化数据
3、 loadView 从nib载入视图 ，除非你没有使用xib文件创建视图
4、 viewDidLoad 载入完成，可以进行自定义数据以及动态创建其他控件
5、 viewWillAppear视图将出现在屏幕之前，马上这个视图就会被展现在屏幕上了
6、 viewDidAppear 视图已在屏幕上渲染完成

1、viewWillDisappear 视图将被从屏幕上移除之前执行
2、viewDidDisappear 视图已经被从屏幕上移除，用户看不到这个视图了
3、dealloc 视图被销毁，此处需要对你在init和viewDidLoad中创建的对象进行释放.

viewVillUnload－ 当内存过低，即将释放时调用；
viewDidUnload－当内存过低，释放一些不需要的视图时调用。

```
**14，应用程序的生命周期**
```
1，启动但还没进入状态保存 ：
- (BOOL)application:(UIApplication *)application willFinishLaunchingWithOptions:(NSDictionary *)launchOptions 

2，基本完成程序准备开始运行：
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
 
3，当应用程序将要入非活动状态执行，应用程序不接收消息或事件，比如来电话了：
- (void)applicationWillResignActive:(UIApplication *)application 

4，当应用程序入活动状态执行，这个刚好跟上面那个方法相反：
- (void)applicationDidBecomeActive:(UIApplication *)application   

5，当程序被推送到后台的时候调用。所以要设置后台继续运行，则在这个函数里面设置即可：
- (void)applicationDidEnterBackground:(UIApplication *)application  

6，当程序从后台将要重新回到前台时候调用，这个刚好跟上面的那个方法相反：
- (void)applicationWillEnterForeground:(UIApplication *)application  

7，当程序将要退出是被调用，通常是用来保存数据和一些退出前的清理工作：
- (void)applicationWillTerminate:(UIApplication *)application  
```
**15，判断view是不是指定视图的子视图**
```
BOOL isView =  [textView isDescendantOfView:self.view];
```
**16，判断对象是否遵循了某协议**
```
if ([self.selectedController conformsToProtocol:@protocol(RefreshPtotocol)]) {
[self.selectedController performSelector:@selector(onTriggerRefresh)];
}
```

**17，页面强制横屏**
```
#pragma mark - 强制横屏代码
- (BOOL)shouldAutorotate{    
//是否支持转屏       
return NO;
}
- (UIInterfaceOrientationMask)supportedInterfaceOrientations{    
//支持哪些转屏方向    
return UIInterfaceOrientationMaskLandscape;
}
- (UIInterfaceOrientation)preferredInterfaceOrientationForPresentation{               
return UIInterfaceOrientationLandscapeRight;
}
- (BOOL)prefersStatusBarHidden{   
return NO;
}
```
**18，系统键盘通知消息**
```
1、UIKeyboardWillShowNotification-将要弹出键盘
2、UIKeyboardDidShowNotification-显示键盘
3、UIKeyboardWillHideNotification-将要隐藏键盘
4、UIKeyboardDidHideNotification-键盘已经隐藏
5、UIKeyboardWillChangeFrameNotification-键盘将要改变frame
6、UIKeyboardDidChangeFrameNotification-键盘已经改变frame
```

**19，关闭navigationController的滑动返回手势**
```
self.navigationController.interactivePopGestureRecognizer.enabled = NO;
```
**20，设置状态栏背景为任意的颜色**
```
- (void)setStatusColor
{
    UIView *statusBarView = [[UIView alloc] initWithFrame:CGRectMake(0, 0,[UIScreen mainScreen].bounds.size.width, 20)];
    statusBarView.backgroundColor = [UIColor orangeColor];
    [self.view addSubview:statusBarView];
}
```
**21，让Xcode的控制台支持LLDB类型的打印**
```
打开终端输入三条命令:
touch ~/.lldbinit
echo display @import UIKit >> ~/.lldbinit
echo target stop-hook add -o \"target stop-hook disable\" >> ~/.lldbinit
```
下次重新运行项目，然后就不报错了。
![](http://upload-images.jianshu.io/upload_images/790890-f3e134d18e165916.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**22，Label行间距**
```Objc
-（void）test{
    NSMutableAttributedString *attributedString =    
[[NSMutableAttributedString alloc] initWithString:self.contentLabel.text];
    NSMutableParagraphStyle *paragraphStyle =  [[NSMutableParagraphStyle alloc] init];  
  [paragraphStyle setLineSpacing:3];

//调整行间距       
[attributedString addAttribute:NSParagraphStyleAttributeName 
value:paragraphStyle 
range:NSMakeRange(0, [self.contentLabel.text length])];
     self.contentLabel.attributedText = attributedString;
}
```
**23，UIImageView填充模式**
```
@"UIViewContentModeScaleToFill",      // 拉伸自适应填满整个视图  
@"UIViewContentModeScaleAspectFit",   // 自适应比例大小显示  
@"UIViewContentModeScaleAspectFill",  // 原始大小显示  
@"UIViewContentModeRedraw",           // 尺寸改变时重绘  
@"UIViewContentModeCenter",           // 中间  
@"UIViewContentModeTop",              // 顶部  
@"UIViewContentModeBottom",           // 底部  
@"UIViewContentModeLeft",             // 中间贴左  
@"UIViewContentModeRight",            // 中间贴右  
@"UIViewContentModeTopLeft",          // 贴左上  
@"UIViewContentModeTopRight",         // 贴右上  
@"UIViewContentModeBottomLeft",       // 贴左下  
@"UIViewContentModeBottomRight",      // 贴右下  
```
**24，宏定义检测block是否可用**
```objc
#define BLOCK_EXEC(block, ...) if (block) { block(__VA_ARGS__); };   
// 宏定义之前的用法
 if (completionBlock)   {   
    completionBlock(arg1, arg2); 
 }    
// 宏定义之后的用法
 BLOCK_EXEC(completionBlock, arg1, arg2);
```
**25，Debug栏打印时自动把Unicode编码转化成汉字**
```
// 有时候我们在xcode中打印中文,会打印出Unicode编码,还需要自己去一些在线网站转换,有了插件就方便多了。
DXXcodeConsoleUnicodePlugin 插件
```
**26，设置状态栏文字样式颜色**
```
[[UIApplication sharedApplication] setStatusBarHidden:NO];
[[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent];
```

**26，自动生成模型代码的插件**
```
// 可自动生成模型的代码，省去写模型代码的时间
ESJsonFormat-for-Xcode
```

**27，iOS中的一些手势**
```
轻击手势（TapGestureRecognizer）
轻扫手势（SwipeGestureRecognizer）
长按手势（LongPressGestureRecognizer）
拖动手势（PanGestureRecognizer）
捏合手势（PinchGestureRecognizer）
旋转手势（RotationGestureRecognizer）
```
**27，iOS 开发中一些相关的路径**
```
模拟器的位置:
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs 

文档安装位置:
/Applications/Xcode.app/Contents/Developer/Documentation/DocSets

插件保存路径:
~/Library/ApplicationSupport/Developer/Shared/Xcode/Plug-ins

自定义代码段的保存路径:
~/Library/Developer/Xcode/UserData/CodeSnippets/ 
如果找不到CodeSnippets文件夹，可以自己新建一个CodeSnippets文件夹。

证书路径
~/Library/MobileDevice/Provisioning Profiles
```
**28，获取 iOS 路径的方法**
``` 
获取家目录路径的函数
NSString *homeDir = NSHomeDirectory();

获取Documents目录路径的方法
NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
NSString *docDir = [paths objectAtIndex:0];

获取Documents目录路径的方法
NSArray *paths = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES);
NSString *cachesDir = [paths objectAtIndex:0];

获取tmp目录路径的方法：
NSString *tmpDir = NSTemporaryDirectory();
```

**29，字符串相关操作 **
```
去除所有的空格
[str stringByReplacingOccurrencesOfString:@" " withString:@""]

去除首尾的空格
[str stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceCharacterSet]];

- (NSString *)uppercaseString; 全部字符转为大写字母
- (NSString *)lowercaseString 全部字符转为小写字母
```

**30， CocoaPods pod install/pod update更新慢的问题**
```
pod install --verbose --no-repo-update 
pod update --verbose --no-repo-update
如果不加后面的参数，默认会升级CocoaPods的spec仓库，加一个参数可以省略这一步，然后速度就会提升不少。
```

**31，MRC和ARC混编设置方式**
```

在XCode中targets的build phases选项下Compile Sources下选择 不需要arc编译的文件
双击输入 -fno-objc-arc 即可

MRC工程中也可以使用ARC的类，方法如下：
在XCode中targets的build phases选项下Compile Sources下选择要使用arc编译的文件
双击输入 -fobjc-arc 即可
```

**32，把tableview里cell的小对勾的颜色改成别的颜色**
```
_mTableView.tintColor = [UIColor redColor];
```

**33，调整tableview的separaLine线的位置**
```
tableView.separatorInset = UIEdgeInsetsMake(0, 100, 0, 0);
```

**34，设置滑动的时候隐藏navigationbar**
```
navigationController.hidesBarsOnSwipe = Yes
```

**35，自动处理键盘事件，实现输入框防遮挡的插件**
```
IQKeyboardManager
https://github.com/hackiftekhar/IQKeyboardManager
```

**36，Quartz2D相关**
```
图形上下是一个CGContextRef类型的数据。
图形上下文包含：
1，绘图路径（各种各样图形）
2，绘图状态（颜色，线宽，样式，旋转，缩放，平移）
3，输出目标（绘制到什么地方去？UIView、图片）

1，获取当前图形上下文
CGContextRef ctx = UIGraphicsGetCurrentContext();
2，添加线条
CGContextMoveToPoint(ctx, 20, 20);
3，渲染
CGContextStrokePath(ctx);
CGContextFillPath(ctx);
4，关闭路径
CGContextClosePath(ctx);
5，画矩形
CGContextAddRect(ctx, CGRectMake(20, 20, 100, 120));
6，设置线条颜色
[[UIColor redColor] setStroke];
7， 设置线条宽度
CGContextSetLineWidth(ctx, 20);
8，设置头尾样式
CGContextSetLineCap(ctx, kCGLineCapSquare);
9，设置转折点样式
CGContextSetLineJoin(ctx, kCGLineJoinBevel);
10，画圆
CGContextAddEllipseInRect(ctx, CGRectMake(30, 50, 100, 100));
11，指定圆心
CGContextAddArc(ctx, 100, 100, 50, 0, M_PI * 2, 1);
12，获取图片上下文
UIGraphicsGetImageFromCurrentImageContext();
13，保存图形上下文
CGContextSaveGState(ctx)
14，恢复图形上下文
CGContextRestoreGState(ctx)
```

**37，屏幕截图**
```
    // 1. 开启一个与图片相关的图形上下文
    UIGraphicsBeginImageContextWithOptions(self.view.bounds.size,NO,0.0);

    // 2. 获取当前图形上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 3. 获取需要截取的view的layer
    [self.view.layer renderInContext:ctx];

    // 4. 从当前上下文中获取图片
    UIImage *image = UIGraphicsGetImageFromCurrentImageContext();

    // 5. 关闭图形上下文
    UIGraphicsEndImageContext();

    // 6. 把图片保存到相册
    UIImageWriteToSavedPhotosAlbum(image, nil, nil, nil);
```

**37，左右抖动动画**
```
//1, 创建核心动画
CAKeyframeAnimation *keyAnima = [CAKeyframeAnimation animation]; 

//2, 告诉系统执行什么动画。
keyAnima.keyPath = @"transform.rotation"; 
keyAnima.values = @[@(-M_PI_4 /90.0 * 5),@(M_PI_4 /90.0 * 5),@(-M_PI_4 /90.0 * 5)]; 

//  3, 执行完之后不删除动画 
keyAnima.removedOnCompletion = NO; 

// 4, 执行完之后保存最新的状态 
keyAnima.fillMode = kCAFillModeForwards; 

// 5, 动画执行时间 
keyAnima.duration = 0.2; 

// 6, 设置重复次数。 
keyAnima.repeatCount = MAXFLOAT; 

// 7, 添加核心动画 
[self.iconView.layer addAnimation:keyAnima forKey:nil];
```

**38，CALayer 的知识**
```
CALayer  负责视图中显示内容和动画
UIView     负责监听和响应事件

创建UIView对象时，UIView内部会自动创建一个图层（既CALayer）
UIView本身不具备显示的功能，是它内部的层才有显示功能.

CALayer属性：
position  中点（由anchorPoint决定）
anchorPoint 锚点
borderColor 边框颜色
borderWidth 边框宽度
cornerRadius 圆角半径
shadowColor 阴影颜色
contents 内容
opacity 透明度
shadowOpacity 偏移
shadowRadius 阴影半径
shadowColor 阴影颜色
masksToBounds 裁剪
```
**39，性能相关**
```
1. 视图复用,比如UITableViewCell,UICollectionViewCell.
2. 数据缓存,比如用SDWebImage实现图片缓存。
3. 任何情况下都不能堵塞主线程，把耗时操作尽量放到子线程。
4. 如果有多个下载同时并发，可以控制并发数。
5. 在合适的地方尽量使用懒加载。
6. 重用重大开销对象，比如：NSDateFormatter、NSCalendar。
7. 选择合适的数据存储。
8. 避免循环引用。避免delegate用retain、strong修饰，block可能导致循环引用，NSTimer也可能导致内存泄露等。
9. 当涉及到定位的时候，不用的时候最好把定位服务关闭。因为定位耗电、流量。
10. 加锁对性能有重大开销。
11. 界面最好不要添加过多的subViews.
12. TableView 如果不同行高，那么返回行高，最好做缓存。
13. Viewdidload 里尽量不要做耗时操作。
```

**40，验证身份证号码**
```
//验证身份证号码
- (BOOL)checkIdentityCardNo:(NSString*)cardNo
{
    if (cardNo.length != 18) {
        return  NO;
    }
    NSArray* codeArray = [NSArray arrayWithObjects:@"7",@"9",@"10",@"5",@"8",@"4",@"2",@"1",@"6",@"3",@"7",@"9",@"10",@"5",@"8",@"4",@"2", nil];
    NSDictionary* checkCodeDic = [NSDictionary dictionaryWithObjects:[NSArray arrayWithObjects:@"1",@"0",@"X",@"9",@"8",@"7",@"6",@"5",@"4",@"3",@"2", nil]  forKeys:[NSArray arrayWithObjects:@"0",@"1",@"2",@"3",@"4",@"5",@"6",@"7",@"8",@"9",@"10", nil]];

    NSScanner* scan = [NSScanner scannerWithString:[cardNo substringToIndex:17]];

    int val;
    BOOL isNum = [scan scanInt:&val] && [scan isAtEnd];
    if (!isNum) {
        return NO;
    }
    int sumValue = 0;

    for (int i =0; i<17; i++) {
        sumValue+=[[cardNo substringWithRange:NSMakeRange(i , 1) ] intValue]* [[codeArray objectAtIndex:i] intValue];
    }

    NSString* strlast = [checkCodeDic objectForKey:[NSString stringWithFormat:@"%d",sumValue%11]];

    if ([strlast isEqualToString: [[cardNo substringWithRange:NSMakeRange(17, 1)]uppercaseString]]) {
        return YES;
    }
    return  NO;
}
```
**41，响应者链条顺序**
```
1> 当应用程序启动以后创建 UIApplication 对象

2> 然后启动“消息循环”监听所有的事件

3> 当用户触摸屏幕的时候, "消息循环"监听到这个触摸事件

4> "消息循环" 首先把监听到的触摸事件传递了 UIApplication 对象

5> UIApplication 对象再传递给 UIWindow 对象

6> UIWindow 对象再传递给 UIWindow 的根控制器(rootViewController)

7> 控制器再传递给控制器所管理的 view

8> 控制器所管理的 View 在其内部搜索看本次触摸的点在哪个控件的范围内（调用Hit test检测是否在这个范围内）

9> 找到某个控件以后(调用这个控件的 touchesXxx 方法), 再一次向上返回, 最终返回给"消息循环"

10> "消息循环"知道哪个按钮被点击后, 在搜索这个按钮是否注册了对应的事件, 如果注册了, 那么就调用这个"事件处理"程序。（一般就是执行控制器中的"事件处理"方法）
```

####42，使用函数式指针执行方法和忽略performSelector方法的时候警告
```
不带参数的：
SEL selector = NSSelectorFromString(@"someMethod");
IMP imp = [_controller methodForSelector:selector];
void (*func)(id, SEL) = (void *)imp;
func(_controller, selector);

带参数的：
SEL selector = NSSelectorFromString(@"processRegion:ofView:");
IMP imp = [_controller methodForSelector:selector];
CGRect (*func)(id, SEL, CGRect, UIView *) = (void *)imp;
CGRect result = func(_controller, selector, someRect, someView);

忽略警告：
#pragma clang diagnostic push 
#pragma clang diagnostic ignored "-Warc-performSelector-leaks"
[someController performSelector: NSSelectorFromString(@"someMethod")]
#pragma clang diagnostic pop

如果需要忽视的警告有多处，可以定义一个宏：
#define SuppressPerformSelectorLeakWarning(Stuff) \ 
do {\ 
_Pragma("clang diagnostic push") \ 
_Pragma("clang diagnostic ignored \"-Warc-performSelector-leaks\"") \
Stuff; \
_Pragma("clang diagnostic pop") \ 
} while (0)
使用方法：
SuppressPerformSelectorLeakWarning( 
[_target performSelector:_action withObject:self]
);
```

**43，UIApplication的简单使用**
```
--------设置角标数字--------
//获取UIApplication对象
    UIApplication *ap = [UIApplication sharedApplication];
//在设置之前, 要注册一个通知,从ios8之后,都要先注册一个通知对象.才能够接收到提醒.
    UIUserNotificationSettings *notice =
    [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeBadge categories:nil];
//注册通知对象
    [ap registerUserNotificationSettings:notice];
//设置提醒数字
    ap.applicationIconBadgeNumber = 20;

--------设置联网状态--------
UIApplication *ap = [UIApplication sharedApplication];
ap.networkActivityIndicatorVisible = YES;
--------------------------
```

44, UITableView隐藏空白部分线条
```
self.tableView.tableFooterView =  [[UIView alloc]init];
```

45，显示git增量的Xcode插件：GitDiff
```
下载地址：https://github.com/johnno1962/GitDiff
这款插件的名字是GitDiff，作用就是可以显示表示出git增量提交的代码行，比如下图
会在Xcode左边标识出来：
```
![](http://upload-images.jianshu.io/upload_images/790890-c600e11f4f73014d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


46，各种收藏的网址
```
unicode编码转换
http://tool.chinaz.com/tools/unicode.aspx    

JSON 字符串格式化
http://www.runoob.com/jsontool

RGB 颜色值转换
http://www.sioe.cn/yingyong/yanse-rgb-16/

短网址生成
http://dwz.wailian.work/

MAC 软件下载
http://www.waitsun.com/

objc 中国
http://objccn.io/
```

47，NSObject 继承图
![](http://upload-images.jianshu.io/upload_images/790890-87f15751aca47601.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

48，浅拷贝、深拷贝、copy和strong
```
浅拷贝：（任何一方的变动都会影响到另一方）
只是对对象的简单拷贝，让几个对象共用一片内存，当内存销毁的时候，指向这片内存的几个指针
需要重新定义才可以使用。

深拷贝：（任何一方的变动都不会影响到另一方）
拷贝对象的具体内容，内存地址是自主分配的，拷贝结束后，两个对象虽然存的值是相同的，但是
内存地址不一样，两个对象也互不影响，互不干涉。

copy和Strong的区别：copy是创建一个新对象，Strong是创建一个指针。
```

49，SEL 和 IMP
```
SEL: 其实是对方法的一种包装,将方法包装成一个SEL类型的数据,去寻找对应的方法地址,找到方法地址后
就可以调用方法。这些都是运行时特性,发消息就是发送SEL,然后根据SEL找到地址,调用方法。


IMP:  是”implementation”的缩写,它是objetive-C 方法 (method)实现代码块的地址,类似函数
指针,通过它可以 直接访问任意一个方法。免去发送消息的代价。
```

50, self 和 super
```
在动态方法中，self代表着"对象"
在静态方法中，self代表着"类"

万变不离其宗，记住一句话就行了：
self代表着当前方法的调用者self 和 super 是oc提供的 两个保留字, 但有根本区别，self是类的隐藏的
参数变量,指向当前调用方法的对象（类也是对象，类对象）
另一个隐藏参数是_cmd，代表当前类方法的selector。

super并不是隐藏的参数,它只是一个"编译器指示符"
super 就是个障眼法 发，编译器符号， 它可以替换成 [self class],只不过 方法是从 self 的
超类开始寻找。
```

51, 长连接 和 短连接
```
长连接：（长连接在没有数据通信时，定时发送数据包(心跳)，以维持连接状态）
连接→数据传输→保持连接(心跳)→数据传输→保持连接(心跳)→……→关闭连接；  
长连接：连接服务器就不断开

短连接：（短连接在没有数据传输时直接关闭就行了）
连接→数据传输→关闭连接；
短连接：连接上服务器，获取完数据，就立即断开。
```

52, HTTP 基本状态码
```
200 OK
    请求已成功，请求所希望的响应头或数据体将随此响应返回。

300 Multiple Choices
    被请求的资源有一系列可供选择的回馈信息，每个都有自己特定的地址和浏览器驱动的商议信息。用户或浏览器能够自行选择一个首选的地址进行重定向。

400 Bad Request
    由于包含语法错误，当前请求无法被服务器理解。除非进行修改，否则客户端不应该重复提交这个请求。

404 Not Found
    请求失败，请求所希望得到的资源未被在服务器上发现。没有信息能够告诉用户这个状况到底是暂时的还是永久的。假如服务器知道情况的话，应当使用410状态码来告知旧资源因为某些内部的配置机制问题，已经永久的不可用，而且没有任何可以跳转的地址。404这个状态码被广泛应用于当服务器不想揭示到底为何请求被拒绝或者没有其他适合的响应可用的情况下。

408 Request Timeout
    请求超时。客户端没有在服务器预备等待的时间内完成一个请求的发送。客户端可以随时再次提交这一请求而无需进行任何更改。

500 Internal Server Error
    服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。一般来说，这个问题都会在服务器的程序码出错时出现。

```

53, TCP 和 UDP 
```
TCP：

- 建立连接，形成传输数据的通道
- 在连接中进行大数据传输（数据大小受限制）
- 通过三次握手完成连接，是可靠协议
- 必须建立连接，效率比UDP低

UDP:

- 只管发送，不管接受
- 将数据以及源和目的封装成数据包中，不需要建立连接、
- 每个数据报的大小限制在64K之内
- 不可靠协议
- 速度快
```

54, 三次握手和四次断开
```
三次握手：
你在吗-我在的-我问你个事情
四次断开握手
我这个问题问完了--你问完了吗---可以下线了吗---我真的问完了拜拜
```

55, 设置按钮按下时候会发光
```
button.showsTouchWhenHighlighted=YES;
```
56，怎么把tableview里Cell的小对勾颜色改成别的颜色？
```
_mTableView.tintColor = [UIColor redColor];  
```

57, 怎么调整Cell 的 separaLine的位置？**
```
_myTableView.separatorInset = UIEdgeInsetsMake(0, 100, 0, 0);  
```

58, ScrollView莫名其妙不能在viewController划到顶怎么办？
```
self.automaticallyAdjustsScrollViewInsets = NO;  
```
59, 设置TableView不显示没内容的Cell。
```
self.tableView.tableFooterView = [[UIView alloc]init]
```

60，复制字符串到iOS剪贴板
```
UIPasteboard *pasteboard = [UIPasteboard generalPasteboard];
pasteboard.string = self.label.text;
```

61，宏定义多行使用方法
```
例子：（ 只需要每行加个 \ 就行了）

#define YKCodingScanData \
-(void)setValue:(id)value forUndefinedKey:(NSString *)key{} \
- (instancetype)initWithScanJson:(NSDictionary *)dict{ \
if (self = [super init]) { \
[self setValuesForKeysWithDictionary:dict]; \
} \
    return self; \
} \
```
62，去掉cell点击后背景变色
```
[tableView deselectRowAtIndexPath:indexPath animated:NO];
如果发现在tableView的didSelect中present控制器弹出有些慢也可以试试这个方法
```
63，线程租调度事例
```
//  群组－统一监控一组任务
dispatch_group_t group = dispatch_group_create();

dispatch_queue_t q = dispatch_get_global_queue(0, 0);
// 添加任务
// group 负责监控任务，queue 负责调度任务
dispatch_group_async(group, q, ^{
[NSThread sleepForTimeInterval:1.0];
NSLog(@"任务1 %@", [NSThread currentThread]);
});
dispatch_group_async(group, q, ^{
NSLog(@"任务2 %@", [NSThread currentThread]);
});
dispatch_group_async(group, q, ^{
NSLog(@"任务3 %@", [NSThread currentThread]);
});

// 监听所有任务完成 － 等到 group 中的所有任务执行完毕后，"由队列调度 block 中的任务异步执行！"
dispatch_group_notify(group, dispatch_get_main_queue(), ^{
// 修改为主队列，后台批量下载，结束后，主线程统一更新UI
NSLog(@"OK %@", [NSThread currentThread]);
});

NSLog(@"come here");
```
64、视图坐标转换
```
// 将像素point由point所在视图转换到目标视图view中，返回在目标视图view中的像素值
- (CGPoint)convertPoint:(CGPoint)point toView:(UIView *)view;
// 将像素point从view中转换到当前视图中，返回在当前视图中的像素值
- (CGPoint)convertPoint:(CGPoint)point fromView:(UIView *)view;

// 将rect由rect所在视图转换到目标视图view中，返回在目标视图view中的rect
- (CGRect)convertRect:(CGRect)rect toView:(UIView *)view;
// 将rect从view中转换到当前视图中，返回在当前视图中的rect
- (CGRect)convertRect:(CGRect)rect fromView:(UIView *)view;

*例把UITableViewCell中的subview(btn)的frame转换到
controllerA中
// controllerA 中有一个UITableView, UITableView里有多行UITableVieCell，cell上放有一个button
// 在controllerA中实现:
CGRect rc = [cell convertRect:cell.btn.frame toView:self.view];
或
CGRect rc = [self.view convertRect:cell.btn.frame fromView:cell];
// 此rc为btn在controllerA中的rect

或当已知btn时：
CGRect rc = [btn.superview convertRect:btn.frame toView:self.view];
或
CGRect rc = [self.view convertRect:btn.frame fromView:btn.superview];
```
65、设置animation动画终了，不返回初始状态
```
animation.removedOnCompletion = NO;
animation.fillMode = kCAFillModeForwards;
```
66、UIViewAnimationOptions类型
```
常规动画属性设置（可以同时选择多个进行设置）
UIViewAnimationOptionLayoutSubviews：动画过程中保证子视图跟随运动。
UIViewAnimationOptionAllowUserInteraction：动画过程中允许用户交互。
UIViewAnimationOptionBeginFromCurrentState：所有视图从当前状态开始运行。
UIViewAnimationOptionRepeat：重复运行动画。
UIViewAnimationOptionAutoreverse ：动画运行到结束点后仍然以动画方式回到初始点。
UIViewAnimationOptionOverrideInheritedDuration：忽略嵌套动画时间设置。
UIViewAnimationOptionOverrideInheritedCurve：忽略嵌套动画速度设置。
UIViewAnimationOptionAllowAnimatedContent：动画过程中重绘视图（注意仅仅适用于转场动画）。  
UIViewAnimationOptionShowHideTransitionViews：视图切换时直接隐藏旧视图、显示新视图，而不是将旧视图从父视图移除（仅仅适用于转场动画）UIViewAnimationOptionOverrideInheritedOptions ：不继承父动画设置或动画类型。
2.动画速度控制（可从其中选择一个设置）
UIViewAnimationOptionCurveEaseInOut：动画先缓慢，然后逐渐加速。
UIViewAnimationOptionCurveEaseIn ：动画逐渐变慢。
UIViewAnimationOptionCurveEaseOut：动画逐渐加速。
UIViewAnimationOptionCurveLinear ：动画匀速执行，默认值。
3.转场类型（仅适用于转场动画设置，可以从中选择一个进行设置，基本动画、关键帧动画不需要设置）
UIViewAnimationOptionTransitionNone：没有转场动画效果。
UIViewAnimationOptionTransitionFlipFromLeft ：从左侧翻转效果。
UIViewAnimationOptionTransitionFlipFromRight：从右侧翻转效果。
UIViewAnimationOptionTransitionCurlUp：向后翻页的动画过渡效果。    
UIViewAnimationOptionTransitionCurlDown ：向前翻页的动画过渡效果。    
UIViewAnimationOptionTransitionCrossDissolve：旧视图溶解消失显示下一个新视图的效果。    
UIViewAnimationOptionTransitionFlipFromTop ：从上方翻转效果。    
UIViewAnimationOptionTransitionFlipFromBottom：从底部翻转效果。
```
67、获取当前View所在的控制器
```
#import "UIView+CurrentController.h"

@implementation UIView (CurrentController)

/** 获取当前View所在的控制器*/
-(UIViewController *)getCurrentViewController{
    UIResponder *next = [self nextResponder];
    do {
        if ([next isKindOfClass:[UIViewController class]]) {
            return (UIViewController *)next;
        }
        next = [next nextResponder];
    } while (next != nil); 
    return nil;
}
```
68、iOS横向滚动的scrollView和系统pop手势返回冲突的解决办法
```
- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer
{
// 首先判断otherGestureRecognizer是不是系统pop手势
if ([otherGestureRecognizer.view isKindOfClass:NSClassFromString(@"UILayoutContainerView")]) {
// 再判断系统手势的state是began还是fail，同时判断scrollView的位置是不是正好在最左边
if (otherGestureRecognizer.state == UIGestureRecognizerStateBegan && self.contentOffset.x == 0) {
return YES;
}
}
return NO;
}
```
69、设置状态栏方向位置
```
修改状态栏方向，
[UIApplication sharedApplication].statusBarOrientation = UIInterfaceOrientationLandscapeLeft;

枚举值说明：
UIDeviceOrientationPortraitUpsideDown,  //设备直立，home按钮在上
UIDeviceOrientationLandscapeLeft,       //设备横置，home按钮在右
UIDeviceOrientationLandscapeRight,      //设备横置, home按钮在左
UIDeviceOrientationFaceUp,              //设备平放，屏幕朝上
UIDeviceOrientationFaceDown             //设备平放，屏幕朝下

再实现这个代理方法就行了
- (BOOL)shouldAutorotate
{
return NO; //必须返回no, 才能强制手动旋转
}
```
69、修改 UICollectionViewCell 之间的间距
```
- (UIEdgeInsets)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout insetForSectionAtIndex:(NSInteger)section
{
    return UIEdgeInsetsMake(10, 10, 10, 10);
}
```
70，时间戳转换成标准时间
```
-(NSString *)TimeStamp:(NSString *)strTime
{
    //因为时差问题要加8小时 == 28800 sec
    NSTimeInterval time=[strTime doubleValue]+28800;
    
    NSDate *detaildate=[NSDate dateWithTimeIntervalSince1970:time];
    
    //实例化一个NSDateFormatter对象
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    
    //设定时间格式,这里可以设置成自己需要的格式
    [dateFormatter setDateFormat:@"yyyy-MM-dd HH:mm:ss"];
    
    NSString *currentDateStr = [dateFormatter stringFromDate: detaildate];
    
    return currentDateStr;
}
```
71，CocoaPods  安装不上怎么办
```
终端进入 repos目录
cd ~/.cocoapods/repos
新建一个文件夹master
然后下载 https://coding.net/u/CocoaPods/p/Specs/git
到master下
```
72，快速创建Block
```
输入 ：xcode中输入 ： inlineblock
```
73，Git 关联仓库 ,和基本配置
```
-------Git global setup-------

git config --global user.name "张大森"
git config --global user.email "zhangdasen@126.com"

-------Create a new repository-------
git clone git@gitlab.testAddress.com:test/QRZxing.git
cd QRZxing
touch README.mdgit
add README.mdgit commit -m "add README"
git push -u origin master

-------Existing folder or Git repository-------
cd existing_folder
git init
git remote add origin git@gitlab.testAddress.com:test/QRZxing.git
git add .
git commit
git push -u origin master

```
74，Git 命令大全

![1234.png](http://upload-images.jianshu.io/upload_images/790890-2233c59924c77b95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>以上整理只为自己和大家方便查看，iOS中小技巧和黑科技数不尽
**如果大家有不错的代码和技巧，也可留言或私信我**，然后加上。
待续。。。。。。
>会继续更新的!  😇
