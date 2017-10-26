---
title: UIScrollView基本使用
layout: post
category: 技术
comments: false

---

	//遵守协议
	1.<UIScrollViewDelegate>


	_myScrollView.frame = self.view.bounds;
        //内容视图尺寸
        _myScrollView.contentSize = CGSizeMake(self.view.frame.size.width*3, self.view.frame.size.height);
        //偏移量
        _myScrollView.contentOffset = CGPointMake(100, 100);
        //内边距
        _myScrollView.contentInset = UIEdgeInsetsMake(50, 50, 50, 50);
        //是否显示横竖滚动条
        _myScrollView.showsHorizontalScrollIndicator = YES;
        _myScrollView.showsVerticalScrollIndicator = YES;
        //是否有弹性
        _myScrollView.bounces = YES;
        //是否可以滚动
        _myScrollView.scrollEnabled = YES;
         //设置代理人
        2._myScrollView.delegate = self;
        
        [self.view addSubview:_myScrollView];
        
        
        
     	3.实现协议方法(这里只是其中一部分方法)
	- (void)scrollViewDidScroll:(UIScrollView *)scrollView
	{
	    
	}
	- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
	{
	    
	}
	- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate
	{
	    
	}
        