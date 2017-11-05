---

title: collectionView瀑布流
layout: post
category: 技术
comments: false

---



	1.自定义一个layout类继承自UICollectionViewLayout
	
	
	2.在.h文件声明一些属性和代理协议用于保存布局数据:
	
	@protocol MyLayoutDelegate <NSObject>
	
	- (float)myLayout:(MyLayout *)layout indexPath:(NSIndexPath *)indexPath;
	
	@end
	
	@property (nonatomic)NSInteger myColumns;
	@property (nonatomic)UIEdgeInsets mySectionInsets;
	@property (nonatomic)float minLine;
	@property (nonatomic)float minItem;
	
	@property (nonatomic)CGSize customSize;
	@property (nonatomic, weak)id<MyLayoutDelegate>delegate;


	3.在.m文件中的扩展中声明一个保存列高度的数组和一个保存所有布局对象的数组, 再声明两个计算最长列和最短列的方法
	@property (nonatomic)NSMutableArray *allItems;
	@property (nonatomic)NSMutableArray *allColumnsHeight;

 	- (NSInteger)shortColumnsIndex;
	- (NSInteger)longColumnsIndex;

	4.将声明的属性懒加载和方法实现
	
	- (NSMutableArray *)allItems
	{
	    if (_allItems == nil)
	    {
	        
	        _allItems = [NSMutableArray array];
	        
	    }
	    
	    return _allItems;
	}
	
	
	- (NSMutableArray *)allColumnsHeight
	{
	    if (_allColumnsHeight == nil)
	    {
	        _allColumnsHeight = [NSMutableArray array];
	    }
	    
	    return _allColumnsHeight;
	}


	
	
	- (NSInteger)shortColumnsIndex
	{
    
    float minValue = MAXFLOAT;
    NSInteger minIndex = 0;
    float selValue = 0;
    
    for (int i = 0; i<self.allColumnsHeight.count; i++)
    {
        selValue = [self.allColumnsHeight[i] floatValue];
        if (selValue < minValue)
        {
            minValue = selValue;
            minIndex = i;
        }
        
    }
    
    return minIndex;
    
	}



	- (NSInteger)longColumnsIndex
	{
	    float maxValue = 0;
	    NSInteger maxIndex = 0;
	    float selValue = 0;
	    
	    for (int i = 0; i<self.allColumnsHeight.count; i++)
	    {
	        
	        selValue = [self.allColumnsHeight[i] floatValue];
	        if (selValue > maxValue)
	        {
	            maxValue = selValue;
	            maxIndex = i;
	        }
	        
	    }
	    
	    return maxIndex;
	    
	}
	
	
	5.计算item的frame
	- (void)layoutItemLP
	{
    
    
    for (int i = 0; i<self.myColumns; i++)
    {
        [self.allColumnsHeight addObject:@(self.mySectionInsets.top)];
    }
    
    NSInteger items = [self.collectionView numberOfItemsInSection:0];
    for (int j = 0; j<items; j++)
    {
        
        NSInteger shortIndex = [self shortColumnsIndex];
        float itemX = self.mySectionInsets.left + (self.customSize.width + self.minItem) * shortIndex;
        float itemY = [self.allColumnsHeight[shortIndex] floatValue] + self.minLine;
        NSIndexPath *indexPathLP = [NSIndexPath indexPathForItem:j inSection:0];
        float itemH = 0;
        if ([self.delegate respondsToSelector:@selector(myLayout:indexPath:)])
        {
            itemH = [self.delegate myLayout:self indexPath:indexPathLP];
        }
        
        UICollectionViewLayoutAttributes *attri = [UICollectionViewLayoutAttributes layoutAttributesForCellWithIndexPath:indexPathLP];
        
        attri.frame = CGRectMake(itemX, itemY, self.customSize.width, itemH);
        [self.allItems addObject:attri];
        
        self.allColumnsHeight[shortIndex] = @([self.allColumnsHeight[shortIndex] floatValue] + self.minLine + itemH);
        
    }
    
    
	}


	6.重写父类的方法
	
	//布局前准备
	- (void)prepareLayout
	{
	    [super prepareLayout];
	    
	    //调用计算item方法
	    [self layoutItemLP];
	}
	
	//返回布局数组
	- (NSArray<UICollectionViewLayoutAttributes *> *)layoutAttributesForElementsInRect:(CGRect)rect
	{
	    return self.allItems;
	}
	
	//显示的范围
	- (CGSize)collectionViewContentSize
	{
	    
	    CGSize contentsize = self.collectionView.contentSize;
	    
	    NSInteger longIndex = [self longColumnsIndex];
	    
	    contentsize.height = [self.allColumnsHeight[longIndex] floatValue] + self.mySectionInsets.bottom;
	    
	    return contentsize;
	    
	}
