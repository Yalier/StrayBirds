--- 
title: collectionView的flowLayout
layout: post
category: 技术
comments: false

---

"

		 //通过属性设置
		@property (weak, nonatomic) IBOutlet UICollectionViewFlowLayout *myFlowLayout;
	
	 	 //每个item的大小
	    self.myFlowLayout.itemSize = CGSizeMake(50, 50);
	    //最小列间距
	    self.myFlowLayout.minimumInteritemSpacing = 10;
	    //最小行间距
	    self.myFlowLayout.minimumLineSpacing = 10;
	    //每组的内间距
	    self.myFlowLayout.sectionInset = UIEdgeInsetsMake(20, 20, 20, 20);
	    
	    
	    
		  //通过代理设置
		  <UICollectionViewDelegateFlowLayout>
		  
	    - (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath;
		- (UIEdgeInsets)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout insetForSectionAtIndex:(NSInteger)section;
		- (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout minimumLineSpacingForSectionAtIndex:(NSInteger)section;
		- (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout minimumInteritemSpacingForSectionAtIndex:(NSInteger)section;

"