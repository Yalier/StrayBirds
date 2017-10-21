---
title: 加载Xib和StoryBoard
layout: post
category: 技术
comments: false
---

        //在storyBoard中寻找viewController
        UIStoryboard *sb = [UIStoryboard storyboardWithName:@"Main" bundle:[NSBundle mainBundle]];
        UIViewController *vc = [sb instantiateViewControllerWithIdentifier:@"ViewController"];
        
        //返回xib
        [[NSBundle mainBundle] loadNibNamed:@"xibView" owner:nil options:nil].firstObject;
