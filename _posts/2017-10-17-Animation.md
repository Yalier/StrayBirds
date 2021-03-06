--- 
layout: post
title: Animation
category: 技术
comments: false
---

Position---位置

Opacity---透明度

Scale-----缩放

Color------颜色

Rotation---翻转



"

//用约束做动画需要添加:

      [self.view layoutIfNeeded];
      

"

"
      
            //UIView基本动画头尾式
            [UIView beginAnimations:nil context:nil];
            [UIView setAnimationDuration:2.0];

            //执行动画的代码块

            [UIView commitAnimations];

"
                

"

//UIView动画 Block:

       [UIView animateWithDuration:1 delay:1 options:UIViewAnimationOptionTransitionNone animations:^{
        CGPoint p = self.right.center;
        p.x = self.view.bounds.size.width - self.right.center.x;
        p.y = self.view.bounds.size.height - self.right.center.y;
        self.right.center = p;
        
        
    } completion:^(BOOL finished) {
        
        
    }];
    
"


    
"

//Bezier曲线动画:

* 初始化

      +(instancetype)bezierPath;   //初始化贝塞尔曲线(无形状)

      +(instancetype)bezierPathWithRect:(CGRect)rect;  //绘制矩形贝塞尔曲线

      +(instancetype)bezierPathWithOvalInRect:(CGRect)rect;  //绘制椭圆（圆形）曲线

      +(instancetype)bezierPathWithRoundedRect:(CGRect)rect cornerRadius:(CGFloat)cornerRadius; // 绘制含有圆角的贝塞尔曲线

      +(instancetype)bezierPathWithRoundedRect:(CGRect)rect byRoundingCorners:(UIRectCorner)corners cornerRadii:(CGSize)cornerRadii;  //绘制可选择圆角方位的贝塞尔曲线

      +(instancetype)bezierPathWithArcCenter:(CGPoint)center radius:(CGFloat)radius startAngle:(CGFloat)startAngle endAngle:(CGFloat)endAngle clockwise:(BOOL)clockwise;   //绘制圆弧曲线

      +(instancetype)bezierPathWithCGPath:(CGPathRef)CGPath; //根据CGPathRef绘制贝塞尔曲线


* 添加线的方法

      -(void)moveToPoint:(CGPoint)point;  //贝塞尔曲线开始的点

      -(void)addLineToPoint:(CGPoint)point;  //添加直线到该点

      -(void)addCurveToPoint:(CGPoint)endPoint controlPoint1:(CGPoint)controlPoint1 controlPoint2:(CGPoint)controlPoint2;  //添加二次曲线到该点

      -(void)addQuadCurveToPoint:(CGPoint)endPoint controlPoint:(CGPoint)controlPoint; //添加曲线到该点

      -(void)addArcWithCenter:(CGPoint)center radius:(CGFloat)radius startAngle:(CGFloat)startAngle endAngle:(CGFloat)endAngle clockwise:(BOOL)clockwise NS_AVAILABLE_IOS(4_0);  //添加圆弧 注:上一个点会以直线的形式连接到圆弧的起点

      -(void)closePath;  //闭合曲线

      -(void)removeAllPoints; //去掉所有曲线点


* 相关属性

      @property(nonatomic) CGFloat lineWidth;  //边框宽度

      @property(nonatomic) CGLineCap lineCapStyle;  //端点类型

      @property(nonatomic) CGLineJoin lineJoinStyle;  //线条连接类型

      @property(nonatomic) CGFloat miterLimit;  //线条最大宽度最大限制

      -(void)setLineDash:(nullable const CGFloat *)pattern count:(NSInteger)count phase:(CGFloat)phase;  //虚线类型

      -(void)fill;  //填充贝塞尔曲线内部

      -(void)stroke; //绘制贝塞尔曲线边框

"


“

// 添加二次 三次贝塞尔曲线

    UIBezierPath *bezierPath = [UIBezierPath bezierPath];
    bezierPath.lineWidth = 2;
    [bezierPath moveToPoint:CGPointMake(10, 520)];
    [bezierPath addLineToPoint:CGPointMake(50, 530)];
    [bezierPath addQuadCurveToPoint:CGPointMake(100, 510) controlPoint:CGPointMake(80, 650)];
    [bezierPath addCurveToPoint:CGPointMake(200, 530) controlPoint1:CGPointMake(130, 600) controlPoint2:CGPointMake(170, 400)];
    [bezierPath addArcWithCenter:CGPointMake(300, 400) radius:50 startAngle:0 endAngle:M_PI * 2 clockwise:YES];
    
    
    
 // 做动画的layer
    
    CALayer* aniLayer = [CALayer layer];
    aniLayer.backgroundColor = [UIColor redColor].CGColor;
    aniLayer.position = CGPointMake(10, 520);
    aniLayer.bounds = CGRectMake(0, 0, 8, 8);
    aniLayer.cornerRadius = 4;
    [self.view.layer addSublayer:aniLayer];
    
    
    
 //CAKeyframeAnimation动画
    
    CAKeyframeAnimation* keyFrameAni = [CAKeyframeAnimation animationWithKeyPath:@"position"];
    keyFrameAni.repeatCount = NSIntegerMax;
    keyFrameAni.path = bezierPath.CGPath;
    keyFrameAni.duration = 15;
    keyFrameAni.beginTime = CACurrentMediaTime() + 1;
    [aniLayer addAnimation:keyFrameAni forKey:@"keyFrameAnimation"];

”
    
