---
title: collectionView实现无限轮播
layout: post
category: 技术
comments: false

---



		//首先懒加载数据源数组
		- (NSMutableArray *)ImageList
	{
	    if (_ImageList == nil)
	    {
	        
	        _ImageList = [NSMutableArray array];
	       
	        for (int i = 0; i<4; i++)
	        {
	            NSString *str = [NSString stringWithFormat:@"%d", i];
	            UIImage *image = [UIImage imageNamed:str];
	            [_ImageList addObject:image];
	        }
	        
	
	    }
	    
	    return _ImageList;
	}
	
	
	//再将collectionView加到控制器view中
		- (UICollectionView *)myCollectionView
	{
	    if (_myCollectionView == nil)
	    {
	    
	        UICollectionViewFlowLayout *flowLayout = [[UICollectionViewFlowLayout alloc] init];
	        flowLayout.itemSize = self.view.bounds.size;
	        flowLayout.minimumLineSpacing = 0;
	        flowLayout.minimumInteritemSpacing = 0;
	        flowLayout.sectionInset = UIEdgeInsetsMake(0, 0, 0, 0);
	        flowLayout.scrollDirection = UICollectionViewScrollDirectionHorizontal;
	        _myCollectionView = [[UICollectionView alloc] initWithFrame:self.view.bounds collectionViewLayout:flowLayout];
	        _myCollectionView.delegate = self;
	        _myCollectionView.dataSource = self;
	        _myCollectionView.pagingEnabled = YES;
	        _myCollectionView.bounces = NO;
	        _myCollectionView.showsHorizontalScrollIndicator = NO;
	
	        [self.view addSubview:_myCollectionView];
	        
	    }
	    
	    return _myCollectionView;
	}
	
	
	//实现代理方法
	- (NSInteger)numberOfSectionsInCollectionView:(UICollectionView *)collectionView
	{
	    return 1;
	}
	
	- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section
	{
	    return self.ImageList.count;
	}
	
	- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath
	{
	
	
	    MyCollectionViewCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"cell" forIndexPath:indexPath];
	    
	    cell.myImage = self.ImageList[indexPath.item];
	    
	    return cell;
	    
	}
	
	
	
	//将数据源数组第一个元素前面和最后一个元素后面交叉插入一下
	[self.ImageList insertObject:self.ImageList.lastObject atIndex:0];
    [self.ImageList insertObject:self.ImageList[0+1] atIndex:self.ImageList.count];
    //collectionView滚动到第一张图片的位置
    [self.myCollectionView  scrollToItemAtIndexPath:[NSIndexPath indexPathForItem:1 inSection:0] atScrollPosition:UICollectionViewScrollPositionNone animated:NO];
    
    
    //使用GCD等待几秒钟后执行定时器
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(3 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        self.myTimer = [NSTimer timerWithTimeInterval:3 target:self selector:@selector(nextCell:) userInfo:nil repeats:YES];
        [self.myTimer fire];
        [[NSRunLoop currentRunLoop]addTimer:self.myTimer forMode:NSDefaultRunLoopMode];//mainRunLoop
    });

	- (void)nextCell:(NSTimer *)timer
	{
	    
	   int page = self.myCollectionView.contentOffset.x / self.myCollectionView.frame.size.width;
	    page ++;
	    
	    [self.myCollectionView scrollToItemAtIndexPath:[NSIndexPath indexPathForItem:page inSection:0] atScrollPosition:UICollectionViewScrollPositionNone animated:YES];
	    
	}



	//在scrollView的代理方法中进行判断是否是最后一张，或者第一张
	-(void)scrollViewDidScroll:(UIScrollView *)scrollView{
    
    NSInteger page = scrollView.contentOffset.x / scrollView.frame.size.width;
    
	//    NSLog(@"scrollViewDidScroll page:%ld",page);
    

        if(page == self.ImageList.count-1)
        {
            //最后一张
            page = 0+1;
            
            NSIndexPath *indexPath = [NSIndexPath indexPathForItem:page inSection:0];
            [self.myCollectionView scrollToItemAtIndexPath:indexPath atScrollPosition:UICollectionViewScrollPositionNone animated:NO];
            
        }
        else if (page == 0 && scrollView.contentOffset.x <= 30)
        {
            //            //第一张
            page = self.ImageList.count-2;
            
            NSIndexPath *indexPath = [NSIndexPath indexPathForItem:page inSection:0];
            [self.myCollectionView scrollToItemAtIndexPath:indexPath atScrollPosition:UICollectionViewScrollPositionNone animated:NO];
        }
    
        
    
	}

	
	
	//当用户开始拖拽的时候就调用
	- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
	{
	    
	        [self.myTimer invalidate];
	    
	}
	
	//当用户停止拖拽的时候调用
	- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate
	{
	    
	        self.myTimer = [NSTimer timerWithTimeInterval:3 target:self selector:@selector(nextCell:) userInfo:nil repeats:YES];
	        [[NSRunLoop currentRunLoop]addTimer:self.myTimer forMode:NSDefaultRunLoopMode];//mainRunLoop
	    
	    
	}
	
