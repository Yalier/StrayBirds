---
layout: post
title: 隐藏navigationBar和tabbar
category: 技术
comments: fasle
---

"

self.navigationController.navigationBarHidden = YES;

self.hidesBottomBarWhenPushed = YES;


在基类中隐藏所有tabbar

-(void)pushViewController:(UIViewController *)viewController animated:(BOOL)animated
{
    if(self.viewControllers.count)
    {
        viewController.hidesBottomBarWhenPushed = YES;
    }

    [super pushViewController:viewController animated:animated];
}

"
