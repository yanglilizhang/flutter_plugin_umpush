[TOC]

# flutter_plugin_umpush

友盟推送插件 支持android 和ios 

对友盟推送不了解的可以先看一下官方的文档  这样对项目会有比较全面的认知

[友盟推送官网文档](https://www.umeng.com/push)

## 集成方式 

在pubspec.yaml 文件上引入如下配置

    引入方式1(引入最新的版本)
    flutter_plugin_record:
        git:
          url: https://github.com/yxwandroid/flutter_plugin_umpush.git
    
    引入方式2 (引入指定某次commit)
    flutter_plugin_record:
        git:
          url: https://github.com/yxwandroid/flutter_plugin_umpush.git
          ref: 29c02b15835907879451ad9f8f88c357149c6085
          


   

  
## 提供的方法

- 设置别名Alias



        ///flutter -> native  setAlias   设置别名
          Future<void> setAlias(String token) async {
            _channel.invokeMethod("setAlias", {"alias": token});
          }

      
- 获取deviceToken


     
      ///flutter -> native get devicveToken
      Future<void> getDeviceToken() async {
         _channel.invokeMethod("deviceToken");
      }




## 使用步骤 
### android 配置步骤 
#### 1,在清单文件内填写友盟账号appkey 

        <meta-data
                android:name="UMENG_APPKEY"
                android:value="5d1ab3d53fc195d690000a13"></meta-data>
         <meta-data
                android:name="UMENG_MESSAGE_SECRET"
                android:value="5f555515f05f1db2830d5cc23bb89469"></meta-data>
                
                
             
             
             
             
       
    
#### 2,在项目中进行初始化插件 

    
     
         FlutterPluginUmpush _flutterUmpush = FlutterPluginUmpush.instance;
         
         
       
   
#### 3,针对使用的场景 调用相应的方法 



### IOS 配置步骤 

####  添加appKey  

在 FlutterPluginUmpushPlugin类的 如下方法中替换appkey 

       
            
        - (BOOL) application:(UIApplication *)application
        didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            NSLog(@"umeng_push_plugin application didFinishLaunchingWithOptions %@", _launchNotification);
            // Override point for customization after application launch.
            [UMCommonLogManager setUpUMCommonLogManager];
            [UMConfigure setLogEnabled:YES];
            
            
            [UMConfigure initWithAppkey:@"5dd658923fc1951fe000006a" channel:@"flutter"];
           // [MobClick event:@"flutter_ok"];
            NSLog(@"umeng_push_plugin application init umeng ok");
          
            
            // Push组件基本功能配置
            UMessageRegisterEntity * entity = [[UMessageRegisterEntity alloc] init];
            //type是对推送的几个参数的选择，可以选择一个或者多个。默认是三个全部打开，即：声音，弹窗，角标
            entity.types = UMessageAuthorizationOptionBadge|UMessageAuthorizationOptionSound|UMessageAuthorizationOptionAlert;
            [UNUserNotificationCenter currentNotificationCenter].delegate=self;
            [UMessage registerForRemoteNotificationsWithLaunchOptions:launchOptions Entity:entity     completionHandler:^(BOOL granted, NSError * _Nullable error) {
                if (granted) {
                }else{
                }
            }];
            
       
            _launchNotification = [launchOptions objectForKey:UIApplicationLaunchOptionsRemoteNotificationKey];
            return YES;
        }







###   注意!!!
 
 默认配置app在前台不接受通知的,具体配置位置分别参考友盟推送前后台推送文档
 