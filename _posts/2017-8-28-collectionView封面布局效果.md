---
title: collectionView封面布局效果
layout: post
category: 技术
comments: false


---




	1.创建一个类继承自UICollectionViewFlowLayout
	2.重写父类中的下列方法

	- (void)prepareLayout
	{
	    [super prepareLayout];
	}
	
	
	//是否重新布局
	- (BOOL)shouldInvalidateLayoutForBoundsChange:(CGRect)newBounds
	{
	    return YES;
	}
	
	//在这个方法中计算每个item的缩放
	- (NSArray<UICollectionViewLayoutAttributes *> *)layoutAttributesForElementsInRect:(CGRect)rect
	{
	    //总中心点
	    CGFloat centent_X = self.collectionView.contentOffset.x + self.collectionView.frame.size.width * 0.5;
	    
	    NSArray *attributesLP = [super layoutAttributesForElementsInRect:rect];
	    for (UICollectionViewLayoutAttributes *attribu in attributesLP)
	    {
	        
	        CGFloat cell_X =  ABS(attribu.center.x - centent_X);
	        
	        CGFloat factor = 0.003;
	        
	        CGFloat scaleLP = 1 / (1 + cell_X * factor);
	        
	        attribu.transform = CGAffineTransformMakeScale(scaleLP, scaleLP);
	        
	        
	    }
	    
	    
	    return attributesLP;
	    
	    
	}
	
	
	//在这个方法中计算中点，根据中点调整每个item的位置
	- (CGPoint)targetContentOffsetForProposedContentOffset:(CGPoint)proposedContentOffset withScrollingVelocity:(CGPoint)velocity
	{
	    
	    CGFloat cen_X = proposedContentOffset.x + self.collectionView.frame.size.width * 0.5;
	    
	    NSArray *myArr = [super layoutAttributesForElementsInRect:CGRectMake(proposedContentOffset.x, proposedContentOffset.y, self.collectionView.frame.size.width, self.collectionView.frame.size.height)];
	    
	    
	    NSInteger minIndex = 0;
	    UICollectionViewLayoutAttributes *min_att = myArr[minIndex];
	    for (int i = 1; i<myArr.count; i++)
	    {
	        
	        UICollectionViewLayoutAttributes *currentAtt = myArr[i];
	        if (ABS(currentAtt.center.x - cen_X) < ABS(min_att.center.x - cen_X))
	        {
	            min_att = currentAtt;
	            minIndex = i;
	        }
	        
	        
	    }
	    
	    
	    CGFloat minDistance = min_att.center.x - cen_X;
	    return CGPointMake(proposedContentOffset.x + minDistance, proposedContentOffset.y);
	    
	    
	}

		