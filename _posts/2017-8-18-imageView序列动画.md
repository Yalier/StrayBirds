---
title: imageView序列帧动画
layout: post
category: 技术
comments: false

---



	//执行动画的数组 animationImages
	self.myIV.animationImages = self.imageAnimationArr;
	
	//动画持续时间 animationDuration
    self.myIV.animationDuration = 5;
    
    //动画重复次数 animationRepeatCount
    self.myIV.animationRepeatCount = NSIntegerMax;
    
    //是否正在执行动画 isAnimating
    self.myIV.isAnimating
    
    //开始动画 startAnimating
    [self.myIV startAnimating];
    
    //结束动画 stopAnimating
    [self.myIV stopAnimating];