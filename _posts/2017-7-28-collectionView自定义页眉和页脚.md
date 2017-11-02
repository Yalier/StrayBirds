---
title: collectionView自定义页眉和页脚
layout: post
category: 技术
comments: false

---


	//注册页眉和页脚
	 [self.myCollectionView registerClass:[HeaderCollectionReusableView class] forSupplementaryViewOfKind:UICollectionElementKindSectionHeader withReuseIdentifier:@"header"];
     [self.myCollectionView registerClass:[FooterCollectionReusableView class] forSupplementaryViewOfKind:UICollectionElementKindSectionFooter withReuseIdentifier:@"footer"];
    
    //一定要设置页眉和页脚的宽高
	    self.myCollectionFlowLayout.headerReferenceSize = CGSizeMake(0, 50);
	    self.myCollectionFlowLayout.footerReferenceSize = CGSizeMake(0, 50);


	//调用这个代理方法返回页眉和页脚
		- (UICollectionReusableView *)collectionView:(UICollectionView *)collectionView viewForSupplementaryElementOfKind:(NSString *)kind atIndexPath:(NSIndexPath *)indexPath
	{
	    
	    Model *m = self.myImageList[indexPath.section];
	    
	    if ([kind isEqualToString:UICollectionElementKindSectionHeader])
	    {
	        
	        HeaderCollectionReusableView *header = [collectionView dequeueReusableSupplementaryViewOfKind:kind withReuseIdentifier:@"header" forIndexPath:indexPath];
	        header.myModel = m;
	        
	        return header;
	        
	    }
	    else
	    {
	        
	        FooterCollectionReusableView *foot = [collectionView dequeueReusableSupplementaryViewOfKind:kind withReuseIdentifier:@"footer" forIndexPath:indexPath];
	        foot.myModel = m;
	        
	        return foot;
	        
	    }
	    
	    
	}
