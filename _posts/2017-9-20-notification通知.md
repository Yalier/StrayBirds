---
layout: post
title: notification通知
category: 技术
comments: false
---

* 通知机制，相当于一个发布者将消息发送给通知中心，然后在通知中心注册过的观察者会监听到这条消息。

[NSNotificationCenter  defaultCenter];   //需要注意的是，通知中心也是一个单例

1.a作为发布者，发送消息下面三种方法：

    -(void)postNotification:(NSNotification *)notification;

    -(void)postNotificationName:(NSString *)aName object:(id)anObject;

    -(void)postNotificationName:(NSString *)aName object:(id)anObject userInfo:(NSDictionary *)aUserInfo;
    
* postNotificationName:指定消息名称；

* object：指定发消息者；

* userInfo：通知中用于传递参数的载体，传递的方法是把参数放在NSDictionary类型的userInfo中。例如：NSDictionary *dict = [notification userInfo];

一般使用第三个方法，如果参数不需要的，可以设置为nil.


2.b注册通知，加入观察者：

  -(void)addObserver:(id)observer selector:(SEL)aSelector name:(nullable NSString *)aName object:(nullable id)anObject;
 //@selector中为回调方法，在本类中对通知进行相应的处理，name为通知名称、object为对象；

* object == nil，那么客户对象（self）将收到任何对象发出NSWindowDidBecomeMainNotification的通知消息；

* name == nil,那么观察者将接收到object对象的所有消息，但是确定不了接收这些消息的顺序。

* object == nil，name == nil，那么该观察者将收到所有对象的所有消息。

* 对于一个任意的观察者observer，如果不能保证其对应的selector有本类自定义的方法（例如：MyMethod），可采用[observer respondsToSelector:@selector(MyMethod:)]] 进行检查。

“

*所以完整的添加观察者过程为：*

      if([observer respondsToSelector:@selector(MyMethod:)])
      {
        [[NSNotificationCenter defaultCenter] addObserver:observer selector:@selector(MyMethod:) name:NSWindowDidBecomeMainNotification object:nil];
      } 
      
 ”     
   
3.移除通知

在ARC下，系统会自动回收无用的通知对象内存，但是由于系统回收机制ARC有一定的延迟性，所以即使不会出错，也建议养成习惯，对通知进行手动释放无用的通知。

移除有2种方法：

    //释放所有通知
    
    - (void)removeObserver:(id)observer;
    
    //释放名称为aName的通知
    
    - (void)removeObserver:(id)observer name:(nullable NSString *)aName object:(nullable id)anObject;
    
    
一般在视图控制器中，可以在“didReceiveMemoryWarning：”中发送解除消息：【这只是参考，建议还是在 ：-(void)dealloc ){} 中进行移除。 】

"

    -(void)didReceiveMemoryWarning
    {
        [super didReceiveMemoryWarning];
        //移除观察者
        [[NSNotificationCenter defaultCenter] removeObserver:self];
    }
    
"


--- 那些年我们用过的系统通知名称~

"

    UIApplicationDidFinishLaunchingNotification // 应用程序启动后
    UIApplicationDidBecomActiveNotification     // 进入前台 
    UIApplicationWillResignActiveNotification   // 应用将要进入后台
    UIApplicationDidEnterBackgroundNotification // 进入后台
    UIKeyboardWillShowNotification        // 键盘即将显示 
    UIKeyboardDidShowNotification         // 键盘显示完毕 
    UIKeyboardWillHideNotification        // 键盘即将隐藏 
    UIKeyboardDidHideNotification         // 键盘隐藏完毕 
    
"


