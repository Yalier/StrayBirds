---
title: UITabbarController的简单使用
layout: post
category: 技术
comments: false

---

	//创建window
	self.self.window=[[UIWindow alloc]initWithFrame:[UIScreen mainScreen].bounds];

	//创建tabbarController
	UITabBarController *tabbarVC = [[UITabBarController alloc] init];
    
    UIViewController *vc1 = [[UIViewController alloc] init];
    vc1.tabBarItem.title = @"vc1";
    vc1.tabBarItem.image = [UIImage imageNamed:@"1"];
    vc1.tabBarItem.selectedImage = [UIImage imageNamed:@"2"];
    
    
    UIViewController *vc2 = [[UIViewController alloc] init];
    vc2.tabBarItem.title = @"vc2";
    vc2.tabBarItem.image = [UIImage imageNamed:@"3"];
    vc2.tabBarItem.selectedImage = [UIImage imageNamed:@"4"];
    
    //添加子控制器
    tabbarVC.viewControllers = @[vc1, vc2];
    
    //将tabbarController设为window的根控制器
    self.window.rootViewController = tabbarVC;
    
    
    //显示window
    [self.window makeKeyAndVisible];
    
    
    
    
    //自定义Tabbar
    也可以自定义一个view，里面添加一些button和label，imageView等控件，然后将这个view设置给UITabbarController的tabbar，通过delegate或者block将按钮的tag值传给UITabbarController的selectedIndex属性实现子控制器间的切换
    
    
    
    