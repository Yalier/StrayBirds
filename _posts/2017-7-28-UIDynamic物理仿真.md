---
title: UIDynamic物理仿真
layout: post
category: 技术
comments: false

---


	1.重力行为
	UIView *redView = [[UIView alloc] init];
    redView.backgroundColor = [UIColor redColor];
    redView.frame = CGRectMake(50, 50, 50, 50);
    [self.view addSubview:redView];
    
    //创建一个UIDynamicAnimator 并设置仿真范围
    UIDynamicAnimator *dyAni = [[UIDynamicAnimator alloc] initWithReferenceView:self.view];
    self.myDynamic = dyAni;
    
    //创建一个物理行为
    //重力行为
    UIGravityBehavior *beha = [[UIGravityBehavior alloc] initWithItems:@[redView]];
	    //力量
	//  beha.magnitude = 0.5;
	    //向量
	//  beha.gravityDirection = CGVectorMake(1, 1);
	    //弧度
	    beha.angle = M_PI_2;
    
    
    //将物理行为添加到dynamicAnimator中
    [dyAni addBehavior:beha];
    
    
    
    
    2.碰撞行为
    //懒加载动画者
	- (UIDynamicAnimator *)myDynamic
	{
	    if (_myDynamic == nil)
	    {
	        
	        _myDynamic = [[UIDynamicAnimator alloc] initWithReferenceView:self.view];
	        
	    }
	    
	    return _myDynamic;
	}

    
    
    //红色view
    UIView *redView = [[UIView alloc] init];
    redView.frame = CGRectMake(100, 50, 60, 60);
    redView.backgroundColor = [UIColor redColor];
    [self.view addSubview:redView];
    
    //蓝色view
    UIView *blueView = [[UIView alloc] init];
    blueView.frame = CGRectMake(60, 100, 30, 30);
    blueView.backgroundColor = [UIColor blueColor];
    [self.view addSubview:blueView];
    
    
    //重力行为
    UIGravityBehavior *gravityB = [[UIGravityBehavior alloc] initWithItems:@[redView, blueView]];
    
    //碰撞行为
    UICollisionBehavior *collisionB = [[UICollisionBehavior alloc] initWithItems:@[redView, blueView]];
    //设置碰撞边界为仿真范围
    collisionB.translatesReferenceBoundsIntoBoundary = YES;
    //碰撞的模式为任意碰撞
    collisionB.collisionMode = UICollisionBehaviorModeEverything;
    
    //添加自定义碰撞边界
    [collisionB addBoundaryWithIdentifier:@"LP" fromPoint:CGPointMake(0, 220) toPoint:CGPointMake(230, 300)];
    
    //把物理行为添加到UIDymic中
    [self.myDynamic addBehavior:gravityB];
    [self.myDynamic addBehavior:collisionB];
    
    
    
    3.甩动行为
    - (UISnapBehavior *)mySnapBehavior
	{
	    if (_mySnapBehavior == nil)
	    {
	        
	        _mySnapBehavior = [[UISnapBehavior alloc] initWithItem:self.myRedView snapToPoint:CGPointZero];
	        //甩动幅度
	        _mySnapBehavior.damping = 0.2;
	        [self.myDynamic addBehavior:_mySnapBehavior];
	        
	    }
	    
	    return _mySnapBehavior;
	}
	
	- (UIDynamicAnimator *)myDynamic
	{
	    if (_myDynamic == nil)
	    {
	        _myDynamic = [[UIDynamicAnimator alloc] initWithReferenceView:self.view];
	        
	    }
	    
	    return _myDynamic;
	}



	- (void)viewDidLoad
	{
	    [super viewDidLoad];
	    
	    //做物理行为动画的view
	    UIView *redV = [[UIView alloc] init];
	    redV.frame = CGRectMake(200, 400, 60, 60);
	    redV.backgroundColor = [UIColor redColor];
	    [self.view addSubview:redV];
	    self.myRedView= redV;
	    
	    
	}



	- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
	{
    
    //代码的优化 只创建一个物理行为
    CGPoint locationP = [touches.anyObject locationInView:self.view];
    //要甩动到的点
    self.mySnapBehavior.snapPoint = locationP;
    
    NSLog(@"%@", @(self.myDynamic.behaviors.count));
	       
	}
    
    
    
    4.附着行为
    @interface AttachmentViewController ()

	@property (nonatomic)UIView *myRedView;
	
	//动画者对象
	@property (nonatomic)UIDynamicAnimator *myDynamic;
	
	//重力行为
	@property (nonatomic)UIGravityBehavior *myGravity;
	//附着行为
	@property (nonatomic)UIAttachmentBehavior *myAttachment;
	
	//手指的位置
	@property (nonatomic)CGPoint touchPoint;
	
	@end
	
	@implementation AttachmentViewController
    
    
    
    //附着行为
	- (UIAttachmentBehavior *)myAttachment
	{
	    if (_myAttachment == nil)
	    {
        
        //让某个控件附着到哪个点
        UIOffset offsetMy = UIOffsetMake(-(self.myRedView.bounds.size.width * 0.5), -(self.myRedView.bounds.size.height * 0.5));
        _myAttachment = [[UIAttachmentBehavior alloc] initWithItem:self.myRedView offsetFromCenter:offsetMy attachedToAnchor:CGPointZero];
        
        //指定点与控件之间的距离  刚性附着
        _myAttachment.length = 100;
        
        //弹性附着 下面两个属性
        _myAttachment.damping = 0.5;
        _myAttachment.frequency = 0.8;
        
        //动画执行期间不停的调用 的block
        __weak typeof(self) weakSelf = self;
        _myAttachment.action = ^{
            
            MyattachmentView *mattV = (MyattachmentView *)weakSelf.view;
            
            mattV.startP = weakSelf.touchPoint;
            
            CGPoint endPoint = CGPointMake(weakSelf.myRedView.bounds.size.width * 0.5 + offsetMy.horizontal,  weakSelf.myRedView.bounds.size.height * 0.5 + offsetMy.vertical);
            endPoint = [weakSelf.myRedView convertPoint:endPoint toView:weakSelf.view];
            
            mattV.endP = endPoint;
            
            [mattV setNeedsDisplay];
        };
        
	    }
	    
	    return _myAttachment;
	}

		//Dynamic
		- (UIDynamicAnimator *)myDynamic
		{
		    if (_myDynamic == nil)
		    {
		        _myDynamic = [[UIDynamicAnimator alloc] initWithReferenceView:self.view];
		    }
		    
		    return _myDynamic;
		    
		}
		
		//重力
		- (UIGravityBehavior *)myGravity
		{
		    if (_myGravity == nil)
		    {
		        _myGravity = [[UIGravityBehavior alloc] initWithItems:@[self.myRedView]];
		    }
		    
		    return _myGravity;
		}




	//触摸开始
	- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
	{
	    
	    CGPoint myPoint = [touches.anyObject locationInView:self.view];
	    self.myAttachment.anchorPoint = myPoint;
	    [self.myDynamic addBehavior:self.myAttachment];
	    self.touchPoint = myPoint;
	    
	    //让绘图view开始绘线条
	    MyattachmentView *myattView = (MyattachmentView *)self.view;
	    myattView.isEnded = NO;
	
	}
	
	//移动
	- (void)touchesMoved:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
	{
	    
	    CGPoint pointMy = [touches.anyObject locationInView:self.view];
	    self.myAttachment.anchorPoint = pointMy;
	    self.touchPoint = pointMy;
	
	    
	}
	
	//手指抬起
	- (void)touchesEnded:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
	{
	    //手指抬起时，移除附着行为
	    [self.myDynamic removeBehavior:self.myAttachment];
	    
	    //让绘图view删除线条
	    MyattachmentView *mV = (MyattachmentView *)self.view;
	    mV.isEnded = YES;
	    [mV setNeedsDisplay];
	    
	    
	}
	
	
	- (void)viewDidLoad
	{
	    [super viewDidLoad];
	    
	    //要做动画的view控件
	    UIView *redView = [[UIView alloc] init];
	    redView.backgroundColor = [UIColor redColor];
	    redView.frame = CGRectMake(100, 300, 60, 60);
	    [self.view addSubview:redView];
	    self.myRedView = redView;
	    
	    
	    [self.myDynamic addBehavior:self.myGravity];
	    
	    
	}
	
	
	//绘图view
	//MyattachmentView.h
		
	@interface MyattachmentView : UIView
	
	@property (nonatomic)CGPoint startP;
	@property (nonatomic)CGPoint endP;
	
	@property (nonatomic)BOOL isEnded;
	
	@end
	
	
	
	//MyattachmentView.m
	
	//绘图
	- (void)drawRect:(CGRect)rect
	{
	    
	    if (self.isEnded)
	    {
	        return;
	    }
	    
	    UIBezierPath *path = [UIBezierPath bezierPath];
	    [path moveToPoint:self.startP];
	    [path addLineToPoint:self.endP];
	    [path stroke];
	    
	    
	}
	
	
	
	
	5.碰撞的代理方法 和 item自身的物理行为
	- (UIDynamicAnimator *)myDynamic
	{
	    if (_myDynamic == nil)
	    {
	        _myDynamic = [[UIDynamicAnimator alloc] initWithReferenceView:self.view];
	    }
	    
	    return _myDynamic;
	}
	
	- (UICollisionBehavior *)myCollisionB
	{
	    if (_myCollisionB == nil)
	    {
	        
	        _myCollisionB = [[UICollisionBehavior alloc] init];
	        //设置碰撞边界的范围是否是仿真范围
	        _myCollisionB.translatesReferenceBoundsIntoBoundary = YES;
	        
	    }
	    
	    return _myCollisionB;
	    
	}
	
	- (UIGravityBehavior *)myGravity
	{
	    if (_myGravity == nil)
	    {
	        
	        _myGravity = [[UIGravityBehavior alloc] init];
	        
	    }
	    
	    return _myGravity;
	}
	
	
	
	- (void)viewDidLoad
	{
	    [super viewDidLoad];
	
	    UIView *blueView = [[UIView alloc] initWithFrame:CGRectMake(245, 60, 70, 70)];
	    blueView.backgroundColor = [UIColor blueColor];
	    [self.view addSubview:blueView];
	    
	    UIView *redView = [[UIView alloc] initWithFrame:CGRectMake(0, 550, 250, 40)];
	    redView.backgroundColor = [UIColor redColor];
	    [self.view addSubview:redView];
	    
	    
	    [self.myGravity addItem:blueView];
	    [self.myCollisionB addItem:blueView];
	    
	    //设置一个碰撞边界跟红色view一样
	    UIBezierPath *path = [UIBezierPath bezierPathWithRect:CGRectMake(0, 550, 250, 40)];
	    [self.myCollisionB addBoundaryWithIdentifier:@"redBoundary" forPath:path];
	    
	    [self.myDynamic addBehavior:self.myGravity];
	    [self.myDynamic addBehavior:self.myCollisionB];
	    
	    
	    //item自身的物理行为
	    UIDynamicItemBehavior *itemBehavior = [[UIDynamicItemBehavior alloc] initWithItems:@[blueView]];
	    //弹力
	    itemBehavior.elasticity = 1;
	    //摩擦力
	    itemBehavior.friction = 0;
	    
	    [self.myDynamic addBehavior:itemBehavior];
	    
	    
	    //碰撞代理
	    self.myCollisionB.collisionDelegate = self;
	    
	}
	
	
	//碰撞代理方法
	- (void)collisionBehavior:(UICollisionBehavior*)behavior beganContactForItem:(id <UIDynamicItem>)item withBoundaryIdentifier:(nullable id <NSCopying>)identifier atPoint:(CGPoint)p
	{
	    
	    NSString *str = (NSString *)identifier;
	    UIView *v = (UIView *)item;
	    if ([str isEqualToString:@"redBoundary"])
	    {
	        v.backgroundColor = [UIColor redColor];
	        
	        [UIView animateWithDuration:0.1 animations:^{
	            
	            v.backgroundColor = [UIColor blueColor];
	        }];
	        
	    }
	    
	    
	}
	
	
	
	6.推行为
	@interface PushViewController ()

	@property (nonatomic)UIDynamicAnimator *myDynamic;
	@property (nonatomic)UIPushBehavior *myPushBehavior;
	@property (nonatomic)UICollisionBehavior *myCollisionBehavior;
	
	@property (nonatomic)UIView *myRedView;
	@property (nonatomic)CGPoint firstPoint;
	@property (nonatomic)CGPoint currentPoint;
	
	@end
	
	@implementation PushViewController
	
	//碰撞行为
	- (UICollisionBehavior *)myCollisionBehavior
	{
	    if (_myCollisionBehavior == nil)
	    {
	        
	        _myCollisionBehavior = [[UICollisionBehavior alloc] init];
	        _myCollisionBehavior.translatesReferenceBoundsIntoBoundary = YES;
	        
	    }
	    
	    return _myCollisionBehavior;
	}
	
	
	//动画者
	- (UIDynamicAnimator *)myDynamic
	{
	    if (_myDynamic == nil)
	    {
	        _myDynamic = [[UIDynamicAnimator alloc] initWithReferenceView:self.view];
	    }
	    
	    return _myDynamic;
	}
	
	
	//推行为
	- (UIPushBehavior *)myPushBehavior
	{
	    if (_myPushBehavior == nil)
	    {
	        
	        _myPushBehavior = [[UIPushBehavior alloc] initWithItems:@[self.myRedView] mode:UIPushBehaviorModeInstantaneous];
	        //将推行为添加到Dynamic中
	        [self.myDynamic addBehavior:_myPushBehavior];
	        
	    }
	    
	    return _myPushBehavior;
	}
	
	
	
	- (void)viewDidLoad {
	    [super viewDidLoad];
	    
	    //红色view
	    UIView *redView = [[UIView alloc] init];
	    redView.backgroundColor = [UIColor redColor];
	    redView.frame = CGRectMake(200, 300, 60, 30);
	    [self.view addSubview:redView];
	    self.myRedView = redView;
	    
	    
	    //将view加进碰撞行为里面
	    [self.myCollisionBehavior addItem:redView];
	    //将碰撞行为加进动画者
	    [self.myDynamic addBehavior:self.myCollisionBehavior];
	    
	
	    
	}
	
	//触摸开始
	- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
	{
	    
	    self.firstPoint = [touches.anyObject locationInView:self.view];
	    
	    
	}
	
	//移动
	- (void)touchesMoved:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
	{
	    
	    self.currentPoint = [touches.anyObject locationInView:self.view];
	    
	    CGFloat distance = hypotf(self.currentPoint.x - self.firstPoint.x, self.currentPoint.y - self.firstPoint.y);
	    NSLog(@"distance---%@", @(distance));
	    //推的力度 与线的长度成正比
	    self.myPushBehavior.magnitude = distance * 0.01;
	   
	
	}
	
	//手指抬起
	- (void)touchesEnded:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
	{
	    
	    //推动方向
	//    self.myPushBehavior.pushDirection = CGVectorMake(self.currentPoint.x - self.firstPoint.x, self.currentPoint.y - self.firstPoint.y);
	    
	    CGPoint myPoint = CGPointMake(self.currentPoint.x - self.firstPoint.x, self.currentPoint.y - self.firstPoint.y);
	    //计算推的角度
	    CGFloat angle = atan2(myPoint.y, myPoint.x);
	    self.myPushBehavior.angle = angle;
	    
	    //激活 瞬时推行为
	    self.myPushBehavior.active = YES;
	
	}
	

