---
title: 获取infoPlist文件信息
layout: post
category: 技术
comments: false
---


    //获取infoPlist文件
        NSDictionary *info = [[NSBundle mainBundle] infoDictionary];

        NSLog(@"%@", info);
        
        
