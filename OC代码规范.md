# OC编码规范

## 建议
- 为了项目的“整洁性”不建议同时使用多语言开发（如OC和Swift混编）
- 在导入第三方框架之前，充分考虑导入此框架的必要性和风险，遵循能不用第三方就不用的原则


## 一、头文件的导入（#import）

- 写法模板
>
> \#import "当前类头发文件"
>
> \#import <系统库>
>
> \#import <第三方库>
>
> \#import "其他类"
>
> 尽量按照先系统类 第三方类 自己写的类顺序导入 中间不能有空格

- 建议的写法

```objc
#import "YJHomeViewController.h"		// 当前类头文件

// controllers 
#import "YJLoginViewController.h" 

// views
#import "YJCycleScrollView.h" 

// models 
#import "YJMessageModel.h"

// others
#import "UINavigationBar+Awesome.h" 
```
- 不建议的写法

```objc
#import "YJHomeViewController.h"		// 当前类头文件
 
#import "YJLoginViewController.h" 
#import "YJCycleScrollView.h" 
#import "YJMessageModel.h"
#import "UINavigationBar+Awesome.h" 
```

## 二、@Class的写法
> 在.h文件中尽量使用@class，引用头文件
> 
> 写法模板
> @class class1, class2;
>

- 建议的写法

```objc
@class UIView, UIImage;
```
- 不建议的写法

```objc
@class UIPress;
@class UIPressesEvent;
```

## 三、@Interface的写法

> 写法模板
>
> @interface 类名 : 父类  <协议1, 协议2>
>
> @interface和类名中间一个空格
>
> 类名后紧跟“ : ”之后空格加上父类协议之间用,空格分割

- 建议的写法

```objc
@interface AppDelegate : UIResponder <UIApplicationDelegate, UITableViewDataSource>
```

- 不建议的写法

```objc
@interface AppDelegate:UIResponder<UIApplicationDelegate,UITableViewDataSource>
```

## 四、@property关键词的使用

> 对象 strong
>
> 基本变量 assign
>
> Xib控件、代理用 weak
>
> 字符串、block使用 copy
>
> 对于一些弱引用对象使用 weak
>
> 对于需要赋值内存对象 copy

## 五、@property的写法

> @property(关键词, 关键词) 类 *变量名称;  // 注释
>
> 关键词用,空格分割 类前后空格

- 建议的写法

```objc
@property (nonatomic, copy) NSString  *productID;         // 产品标识
@property (nonatomic, copy) NSString  *status;            // 状态
```

- 不建议的写法

```objc
@property (nonatomic, copy) NSString  * productID;         // 产品标识
@property (nonatomic, copy) NSString  * status;            // 状态
```


## 六、控件尽量放到匿名分类中且分组
> 1. 控件组
>
> 2. 数据源组<建议带上模型类型>
> 
> 3. 交互变量组

- 建议的写法

```objc
@interface YJHomeViewController ()  
@property (nonatomic, strong) UIView *navigationBarView;        // 导航栏  
@property (nonatomic, strong) YJCycleScrollView *advertiseView; // 广告栏 
@property (nonatomic, strong) YJPopularRecommendView *popularView; // 热门推荐视图

@property (nonatomic, strong) NSMutableArray <YJBannerModel *> *adsArray;          // 广告 
@property (nonatomic, strong) NSMutableArray <YJProductModel *> *popularInfoArray; // 热门推荐 

@property (nonatomic, assign) NSInteger nowPage;            // 当前页码 
@property (nonatomic, assign) NSInteger selectedIndex;

@end 
```

## 七、类的功能模块建议按以下方式分组
> 1. view的生命周期方法
> 
> 2. 初始化方法
> 
> 3. 代理
> 
> 4. 事件处理
> 
> 5. 网络请求
> 
> 6. 懒加载方法

- 建议的写法

```objc
#pragma mark - view的生命周期方法

#pragma mark - setup

#pragma mark - <DataSource>

#pragma mark - <Delegate>

#pragma mark - actions

#pragma mark - request dataSource

#pragma mark - setter and getter
```

## 八、const的使用

> 1. 开头用k标识
>
> 2.  k + 项目名前缀 + 作用名称 + 模块名（可不写）

- 建议的写法

```objc
static NSString *const kYJOpenAccountAgreement = @"yjlc://openAccount";     // 开户 
static NSString *const kYJAddRechargeAgreement = @"yjlc://addRecharge";     // 充值 
static NSString *const kYJAddWithdrawAgreement = @"yjlc://addWithdraw";	    // 提现
```

### 定义外部可使用的const常量
- 声明

```objc
UIKIT_EXTERN NSString *const kNoticationUpdateCartList;
``` 
- 实现 

```objc
NSString *const kNoticationUpdateCartList = @"kNoticationUpdateCartList";
```  
### 对于只在.m内部声明的const,建议添加static
- 建议写法

```objc
static NSString *kInvestmentCellID = @"InvestmentCellID";
```
 
## 九、enum的定义
> 
> 1. 对应的enum写到对应的类中，方便寻找和使用
>
> 2. 使用NS _ ENUM和NS _ OPTIONS进行定义

- 建议的写法

```objc
typedef NS_ENUM(NSInteger, UIViewAnimationCurve) {
    UIViewAnimationCurveEaseInOut,         // slow at beginning and end
    UIViewAnimationCurveEaseIn,            // slow at beginning
    UIViewAnimationCurveEaseOut,           // slow at end
    UIViewAnimationCurveLinear,
};

typedef NS_OPTIONS(NSUInteger, UIViewAutoresizing) {
    UIViewAutoresizingNone                 = 0,
    UIViewAutoresizingFlexibleLeftMargin   = 1 << 0,
    UIViewAutoresizingFlexibleWidth        = 1 << 1,
    UIViewAutoresizingFlexibleRightMargin  = 1 << 2,
    UIViewAutoresizingFlexibleTopMargin    = 1 << 3,
    UIViewAutoresizingFlexibleHeight       = 1 << 4,
    UIViewAutoresizingFlexibleBottomMargin = 1 << 5
};
```

- 不建议的写法

```objc
typedef enum {
    GBAppRunDeveloperModeDebug,
    GBAppRunDeveloperModePreRelease,
    GBAppRunDeveloperModeRelease
}GBAppRunDeveloperMode;
```
## 十、三元操作符
> 简单的if else判断，尽量使用三目运算符，提高代码的简洁性和可读性

- 建议写法

```objc
NSInter result = a > b ? a : b;
```

- 不建议写法

```objc
NSInter result;
if (a > b) {
	result = a;
} else {
	result = b;
}
```

## 十一、单例
> 建议使用 dispatch _ once 来创建单例

- 建议写法

```objc
+ (instancetype)sharedInstance {
	static id sharedInstance = nil;
	static dispatch_once_t onceToken;
 	dispatch_once(&onceToken, ^{
 		sharedInstance = [[self alloc] init];
   }); 
	return sharedInstance;
}
``` 

## 十二、init方法
> 返回类型应该使用instancetype而不是id

- 建议写法

```objc
- (instancetype)init { 
	if (self = [super init]) {
		// ...
	}
	return self;
}
```

## 十三、写模块化的代码 
> 好的模块化代码需要满足以下几点：
> 
> 1. 避免写太长的函数（函数长度最好都不超过40行，因为我的眼球不转的话，最大的视角只看得到40行代码）；
> 
> 2. 制造小的工具函数，让逻辑看起来更清晰，提高代码复用率；
> 
> 3. 每个函数只做一件逻辑相关的事情，比如读文件函数就只做读取文件操作，写入文件函数就只做写入功能，绝对不掺合其他逻辑；
> 
> 4. 避免使用全局变量和类成员（class member）来传递信息，尽量使用局部变量和参数（减少对全局变量和类成员的维护成本和出错率）；

## 十四、写可读的代码
> 有些人以为写很多注释就可以让代码更加可读，然而却发现事与愿违。注释不但没能让代码变得可读，反而由于大量的注释充斥在代码中间，让程序变得障眼难读。而且代码的逻辑一旦修改，就会有很多的注释变得过时，需要更新。修改注释是相当大的负担，所以大量的注释，反而成为了妨碍改进代码的绊脚石。
> 
> 所以如何写可读的代码，我们首先要明白什么时候写注释，什么时候不用写注释。

### 什么时候写注释：
> 1. 有时，你也许会为了绕过其他一些代码的设计问题，采用一些违反直觉的作法。这时候你可以使用很短注释，说明为什么要写成那奇怪的样子。（尽量没有）
> 
> 2. 后台返回的参数，有时确实需要写简短的注释来标明这个字段的意思。
> 
> 3. 网络请求的方法，我们有事需要标注方法的作用、参数和返回值。
> 
> 4. 当一块功能模块，需要严格按照某个业务逻辑执行才行时，可以使用适当的注释。
> 
> 5. 合理的使用TODO:，当你未完成此功能又急需去完成其它功能的时候，请放一个TODO:在这并解析清楚未完成的工作，保证自己不会忘掉，并在完成后删掉（这个很重要）。

- 建议写法

```objc
// 属性

@property (nonatomic, copy) NSString *userName;     // 用户昵称
@property (nonatomic, copy) NSString *address;      // 用户地址
```

```objc
// 网络请求

/**
 *  获取订单详情
 *  
 *  @param orderId  订单编号
 *  
 *  @return 网络请求
 */
+ (NSURLSessionDataTask *)getOrderInfoById:(NSString *)orderId completionHandler:(void(^)(OrderModel *order, NSError *error))completionHandler;
```

```objc
// 有较强的业务逻辑及TODO:注释

- (void)checkUserAuthentication {
    // 判断是否实名认证
    if (![YJRuntime sharedInstance].currentUser.tpStatus) {
        // TODO: 去实名认证
        return;
    }
    
    // 判断是否实设置支付密码
    if (![YJRuntime sharedInstance].currentUser.payPwdStatus) {
        // TODO: 去设置支付密码
        return;
    }
    
    // 判断是否绑定银行卡
    if (![YJRuntime sharedInstance].currentUser.bankcardStatus) {
        // TODO: 去绑定银行卡
        return;
    }
    
    // TODO: ...
}
``` 

	
### 不写注释，也能写出可读性很强的代码：
> 使用有意义的函数和变量名字 
> 
 
```objc
// 如系统的present方法，不管方法名还是变量名都很好的表达了方法的功能和参数的意义
- (void)presentViewController:(UIViewController *)viewControllerToPresent animated: (BOOL)flag completion:(void (^ __nullable)(void))completion

// 有时候我们为了表达清楚某个方法和参数的意思，避免不了会写的很长
- (void)showWebViewControllerWithProductModel:(YJProductModel *)productModel {
    // ... 
}

``` 

> 局部变量应该尽量接近使用它的地方
> 

- 建议的写法
	
```objc
- (void)loadDataSource {
	...
	...
	int index = ...;
	[self refreshViewByIndex:index];
    ...
}
```
- 不建议的写法 
	
```objc
- (void)loadDataSource {
	... 
	int index = ...;
	...
	...
	[self refreshViewByIndex:index];
    ...
}
```
> 局部变量名字应该简洁明了

- 建议的写法
	
```objc
boolean success = [self deleteFileWithfilePath:...];
if (success) {
	...
} else {
	...
}
```
- 不建议的写法 
	
```objc
boolean successInDeleteFile = [self deleteFileWithfilePath:...];
if (successInDeleteFile) {
	...
} else {
	...
} 
```

>  把复杂的逻辑提取出去，做成“帮助函数”

- 建议的写法
	
```objc
// 把查询、更新和保存的代码分开了实现
- (BOOL)updateFileContentWithContent:(NSData *)content filePath:(NSString *)filePath {
	// 1. 调用查找文件方法
	
	// 2. 更新文件内容代码
	
	// 3. 调用保存文件方法
}

- (NSData *)findContentWithFilePath:(NSString *)filePath {
	// ...
}

- (BOOL)saveContentWithFilePath:(NSString *)filePath {
	// ...
}

```

- 不建议的写法 
	
```objc
// 把查询、更新和保存文件的所有代码写在一块了
- (BOOL)updateFileContentWithContent:(NSData *)content filePath:(NSString *)filePath {
	// 1. 查找文件代码
	
	// 2. 更新文件内容代码
	
	// 3. 保存文件代码
	
	// ...
}
```

>  把复杂的表达式提取出去，做成中间变量
	
- 建议的写法
	
```objc
	NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
	[defaults setObject:@"123" forKey:@"OrderID"];
	[defaults synchronize];
```
-  不建议的写法 
	
```objc
[[[NSUserDefaults standardUserDefaults] setObject:@"123" forKey:@"OrderID"] synchronize];

```

> 在合理的地方换行
	
- 建议的写法
	
```objc
	if (userName.length > 0 && 
	    [phone checkedPhoneNumber] &&
	    [password checkedPassword]) {
        // ...
    }
```
- 不建议的写法
	
```objc
	if (userName.length > 0 && [phone checkedPhoneNumber] && [password checkedPassword]) {
        // ...
    }
```

## 十五、写直观的代码
>
>写代码有一条重要的原则：如果有更加直接，更加清晰的写法，就选择它（即使它有时候看起来更长，更笨，也一样选择它）
>
	
- 建议的写法
	
```objc  
// 案例： 如果需要多个嵌套进行判断，可以写成下面那样，不满足直接返回的方式
- (void)checkUserAuthentication { 
    if (![YJRuntime sharedInstance].currentUser.tpStatus) {
        // TODO: 去实名认证
        return;
    }
     
    if (![YJRuntime sharedInstance].currentUser.payPwdStatus) {
        // TODO: 去设置支付密码
        return;
    }
     
    if (![YJRuntime sharedInstance].currentUser.bankcardStatus) {
        // TODO: 去绑定银行卡
        return;
    }
    
    // TODO: ...
}
```
- 不建议的写法
	
```objc
	if ([YJRuntime sharedInstance].currentUser.tpStatus) {
   		if ([YJRuntime sharedInstance].currentUser.payPwdStatus) {
         	if ([YJRuntime sharedInstance].currentUser.bankcardStatus) {
	       	// TODO: ...
    		} else {
    			// TODO: 去绑定银行卡
    		} 
    	} else {
    		// TODO: 去设置支付密码
    	} 
    } else {
    	// TODO: 去实名认证
    } 
```

## 十六、通知及监听的移除

> 如果类中监听了通知，应该在dealloc方法中移除对象监听

- 建议的写法

```objc
- (void)dealloc {
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}
```

- 不建议的写法

```objc
- (void)dealloc {
    [[NSNotificationCenter defaultCenter] removeObserver:self name:name1 object:nil];
    [[NSNotificationCenter defaultCenter] removeObserver:self name:name2 object:nil];
    [[NSNotificationCenter defaultCenter] removeObserver:self name:name3 object:nil];
}
``` 
## 十七、其它
### 1. 使用readonly修饰，不被外部修改的属性；
 
### 3. 方法命名的规范 
- 如果不是写初始化方法不要用init进行开头
- 如果不是属性的set方法不要用set作为方法的前缀
 
### 4. 控件命名的规范
- 一定不要单单用首字母简写命名控件（特殊意义的除外，如WTO、RMB等），并且名字后一定要加上控件类型，例如： UILabel结尾加上Label，UIImageView结尾记上ImageView。
- 建议的写法

```objc
@property(nonatomic, strong) UILabel *userNameLabel;
```
- 不建议的写法

```objc
@property(nonatomic, strong) UILabel *userName;
``` 

### 5. 大括号{}不换行
- 建议的写法

```objc
if(YES) {
  // ...
}
```
- 不建议的写法

```objc
if(YES)
{
  // ...
}
``` 
  
### 6. 对于#define宏命名

> 单词全部的大写，且单词之间用“_”分割，最好加上项目前缀

建议的写法

```objc
#define NS_ENUM_AVAILABLE_MAC(_mac) CF_ENUM_AVAILABLE_MAC(_mac)
#define NS_ENUM_AVAILABLE_IOS(_ios) CF_ENUM_AVAILABLE_IOS(_ios)
```

不建议的写法

```objc
#define ZDScreenHeight [UIScreen mainScreen].bounds.size.height
#define ZDScreenWidth  [UIScreen mainScreen].bounds.size.width
```

### 7. 局部变量 

> 局部的变量在要初始化时，尽量设置默认值（对于一些对象判断是否赋值可以不进行初始化）
> 

- 建议的写法

```objc
int index = 0;
```

- 不建议的写法

```objc
int index;
```

>  使用驼峰命名法，命名变量

- 建议的写法

```objc
UIViewController *viewController = [[UIViewController alloc] init];
// 如果实在是 不想写太长的参数名，可以写成首字母大写的形式，但语义一定要清楚
YJWKWebViewController *webVC = [[YJWKWebViewController alloc] initWithUrl:nil];
```
- 不建议的写法

```objc
UIViewController *viewcontroller = [[UIViewController alloc] init];
YJWKWebViewController *webvc = [[YJWKWebViewController alloc] initWithUrl:nil];
```

### 8. 对于NS_OPTIONS类型多个值用“|”连接不能用“+”

- 建议的写法

```objc
UNAuthorizationOptionAlert | UNAuthorizationOptionBadge | UNAuthorizationOptionSound
```

- 不建议的写法

```objc
UNAuthorizationOptionAlert + UNAuthorizationOptionBadge + UNAuthorizationOptionSound
```

### 9. block的命名 
> 尽量和苹果的命名一致使用completion或CompletionHandle结尾，也可用Block命名作为参数名结尾。
>  

- 建议的写法

```objc
typedef void(DidUpdateViewWithCompletionHandle)()
```

- 不建议的写法

```objc
typedef void(DidUpdateViewWithCallBack)()
```

### 10. 尽量少在initialize或load方法做一些初始化的事情

- 建议的做法

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    
    [[UITabBarItem appearance] setTitleTextAttributes:@{NSForegroundColorAttributeName : GBCOLOR(153, 153, 153, 1.0)} forState:UIControlStateNormal];
    [[UITabBarItem appearance] setTitleTextAttributes:                                                         @{NSForegroundColorAttributeName : GBCOLOR(255, 129, 55, 1.0)} forState:UIControlStateSelected];
 }
```

- 不建议的做法

```objc
+ (void)initialize {
    [[UITabBarItem appearance] setTitleTextAttributes:@{NSForegroundColorAttributeName : GBCOLOR(153, 153, 153, 1.0)} forState:UIControlStateNormal];
    [[UITabBarItem appearance] setTitleTextAttributes:                                                         @{NSForegroundColorAttributeName : GBCOLOR(255, 129, 55, 1.0)} forState:UIControlStateSelected];
}
```     

### 11. 属性使用懒加载 
- 建议的写法

```objc
- (UILabel *)titleLabel {
    if (!_titleLabel) {
        _titleLabel 		      = [[UILabel alloc] init];
        _titleLabel.font 	      = [UIFont systemFontOfSize:16];
        _titleLabel.textColor     = [UIColor darkGrayColor];
        _titleLabel.textAlignment = NSTextAlignmentCenter;
    }
    return _titleLabel;
}
```

### 12. 给controller“瘦身”
> 1. 对于页面的搭建尽量封装到独立的view中；
> 
> 2. model（或viewModel）负责数据的请求和解析；
> 
> 3. controller最好只做页面的跳转和交互操作；
> 
> 4. 合理利用分类，封装和扩展功能。 

### 13. 多用类型常量，少用#define

- 建议的写法

```objc
static const NSTimeInterval kAnimationDuration = 0.3;
```

- 不建议的写法

```objc
#define ANIMATION_DURATION 0.3
```

### 14. 对于一些状态的判断，使用枚举表示不同的状态
> 尽量少用根据数字来直接判断不同的状态 

-  建议的写法

```objc
typedef NS_ENUM(NSUInteger, HomeViewState) {
  	HomeViewStateNoData,
  	HomeViewStateFailure,
  	HomeViewStateItemList,
  	HomeViewStateBannerList
};
switch(state) {
	case HomeViewStateNoData : {
		// 显示没数据
		break;
	} 
	case HomeViewStateFailure : {
		// 显示请求错误
		break;
	} 
	case HomeViewStateItemList : {
		// 显示商品的列表
		break;
	} 
	case HomeViewStateBannerList : {
		// 显示banner列表
		break;
	}
	default :
	break;
} 
```

- 不建议的写法

```objc
if(state == 0) {
  // 显示没数据
} else if(state == 1) {
  // 显示请求错误
} else if(state == 2) {
  // 显示商品的列表
} else if(state == 3) {
  // 显示banner列表
} else {}
```  

### 15. 对于一些自己不确定的可以使用try catch
> 对于不知道后台返回什么类型的，可以使用try catch (因为OC是运行时语法，可能array不一定是NSArray类型的)

- 建议的写法

```objc
int index = 0;
@try {
  NSArray *array = obj[@"list"];
  index = [array.firstObject intValue];
}
@catch {}
``` 

- 不建议的写法

```objc
// 如果后台返回list为字段 这段代码就崩溃了 可以使用try catch也可以用Model库 或者自己添加判断
int index = 0;
NSArray *array = obj[@"list"];
if(array.count > 0) {
  index = [array.firstObject intValue];
}
```

### 16. 遍历方法的选择
> 1. 如果只需要便利数组和字典尽量使用增强for循环方法

- 建议的写法

```objc
for(NSString *name in names) {
  // ...
}
```

- 不建议的写法

```objc
for(int i = 0; i < names.lenght ; i ++) {
  NSString *name = names[i];
  // ...
}
```

> 2. 需要便利字典和数组的内容 并且需要索引用block的遍历方法

- 建议的写法

```objc
[names enumerateObjectsUsingBlock:^(NSString * _Nonnull name, NSUInteger idx, BOOL * _Nonnull stop) {
	// ...
}];
```
- 不建议的写法

```objc
for(int i = 0; i < names.lenght ; i ++) {
  NSString *name = names[i];
  // ...
}
```

### 17. 如果想进行缓存优先使用NSCache，不要使用NSDictionary进行缓存

- 建议的写法

```objc
NSCache *cache = [[NSCache alloc] init];
[cache setObject:object forKey:key];
```
- 不建议的写法

```objc
NSMutableDictionary *dictionary = [NSMutableDictionary dictionary];
[dictionary setObject:object forKey:key];
```
 
### 18. nil 和 BOOL 检查
- 建议的写法

```objc
if(name) {
	// ...
}
if (isMyFriend) {
	// ... 
}
```

- 不建议的写法

```objc
if(name != nil) {
 	// ...
}
if(isMyFriend == YES) {
	// ... 
}
```     
 

### 19. 数组和字典最好指定元素的类型
- 建议的写法

```objc
NSArray<NSString *> *names = [NSArray array];
```

- 不建议的写法

```objc
NSArray *names = [NSArray array];
```
### 20. 数组和字典的元素垂直写
- 建议的写法

```objc
NSArray *array = @[
  					@"a",
  					@"b",
  					@"b"
					];
NSDictionary *dictionary = @{
							@"a" : @"",
							@"b" : @"",
							@"c" : @""
                            };
```

- 不建议写法

```objc
NSArray *array = @[@"a", @"b", @"b"];
NSDictionary *dictionary = @{@"a" : @"", @"b" : @"", @"c" : @""};
```

### 21. 使用CGRect的函数获取值

- 建议的写法

```objc 
CGFloat x 	   = CGRectGetMinX(frame);
CGFloat y 	   = CGRectGetMinY(frame);
CGFloat width  = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

- 不建议写法

```objc
CGFloat x      = frame.origin.x;
CGFloat y      = frame.origin.y;
CGFloat width  = frame.size.width;
CGFloat height = frame.size.height;
```

### 22. git分支功能介绍

- master: 主分支。只对应线上版本的代码。
- develop: 开发分支，永远是最新的代码，团队成员需要开始新需求的时候，都是在这个分支上拉取代码。
- fixbug: 修复线上bug的临时分支。代码从master上check下来，上线后merge到master和develop上去，并且会被删除掉。
- release: 发布版本时的临时分支，从develop分支上check下来，用于固定发布那一刻的代码版本，如果在发布期间有问题可以在此分支上进行修改。上线后会合并到master和develop上去，并且会被删除掉。


