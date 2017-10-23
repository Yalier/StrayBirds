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
    
    * 注
    	//这个方法会有图片缓存
		[UIImage imageNamed:imageStr];
		
		
		//获取图片路径
		NSString *path = [[NSBundle mainBundle] pathForResource:imageStr ofType:@"png"];

		//这个方法没有图片缓存
		[UIImage imageWithContentsOfFile:path];
		
		
		//执行完动画后，释放掉数组内存
        //[self.myIV performSelector:@selector(setAnimationImages:) withObject:nil afterDelay:self.myIV.animationDuration];
        [self.myIV performSelector:@selector(setAnimationImages:) withObject:nil];
   	