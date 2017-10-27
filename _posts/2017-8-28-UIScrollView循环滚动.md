---
title: UIScrollView循环滚动
layout: post
category: 技术
comments: false

---



	 UIScrollView循环滚动一般和UIPageControl一起使用
	 1.首先创建一个UIScrollView加入到父容器中,在创建一个PageControl也加入到父容器中
	 2.一般for循环往ScrollView里面添加要展示的图片ImageView
	 for (int i = 0; i<3; i++)
    {
        UIImageView *iv = [[UIImageView alloc] initWithImage:[UIImage imageNamed:[NSString stringWithFormat:@"%d", i]]];
        iv.frame = CGRectMake(self.myScrollView.frame.size.width * i, 0, self.myScrollView.frame.size.width, self.myScrollView.frame.size.height);
        [self.myScrollView addSubview:iv];
        
    }
    
    3.在这个代理中让pageControl和scrollView联动
	    - (void)scrollViewDidScroll:(UIScrollView *)scrollView
	{
	    
	    self.myPage.currentPage = (scrollView.contentOffset.x + scrollView.frame.size.width * 0.5) / scrollView.frame.size.width;
	    
	}
    
    4.添加一个定时器NSTimer来实现循环滚动
    self.myTimer = [NSTimer scheduledTimerWithTimeInterval:2 target:self selector:@selector(autoScroll) userInfo:nil repeats:YES];
    NSRunLoop *runLoop = [NSRunLoop currentRunLoop];
    [runLoop addTimer:self.myTimer forMode:NSRunLoopCommonModes];
    
	    - (void)autoScroll
	{
	    NSInteger index = self.myPage.currentPage;
	    index ++;
	    if (index > self.myPage.numberOfPages - 1)
	    {
	        index = 0;
	    }
	    
	    [self.myScrollView setContentOffset:CGPointMake(index * self.myScrollView.frame.size.width, 0) animated:YES];
	    
	}
	
	5.手动拖动scrollView时移除定时器，松手时再加入定时器
		- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
	{
	    [self.myTimer invalidate];
	}
	
	
	- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate
	{
	    self.myTimer = [NSTimer scheduledTimerWithTimeInterval:2 target:self selector:@selector(autoScroll) userInfo:nil repeats:YES];
	    NSRunLoop *runLoop = [NSRunLoop currentRunLoop];
	    [runLoop addTimer:self.myTimer forMode:NSRunLoopCommonModes];
	}

 