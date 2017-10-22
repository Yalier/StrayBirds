---

title: transform相关
category: 技术
layout: post
comments: false

---


'''


	//360度 2π
    //180度 π
    //90度 π/2
    //45度 π/4
    //旋转45度，只旋转一次
    self.view.transform = CGAffineTransformMakeRotation(M_PI_4);
    //旋转45度，可以不停的旋转
    self.view.transform = CGAffineTransformRotate(self.view.transform, M_PI_4);
    
    
    //缩放一次
    self.view.transform = CGAffineTransformMakeScale(1.5, 1.5);
    //可以不停缩放
    self.view.transform = CGAffineTransformScale(self.view.transform, 1.5, 1.5);
    
    
    //平移一次
    self.view.transform = CGAffineTransformMakeTranslation(30, 30);
    //可以不停平移
    self.view.transform = CGAffineTransformTranslate(self.view.transform, 30, 30);
    
     //回到最初的状态
    self.view.transform = CGAffineTransformIdentity;
    
 '''