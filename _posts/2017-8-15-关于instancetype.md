---
title: 关于instancetype
layout: post
category: 技术
comments: false

---


	* instancetype是方法中的返回值类型，常见于初始化方法，返回当前的类的对象。
	
	* 用instancetyp作为返回值类型，是为了避免在其他地方重用方法时，返回值类型不相同，而产生问题。
	
	例如：一个父类LPView中有一个返回值为(LPView *)的方法，另一个继承它的子类SubView中，用了这个方法，这个方法的返回值类型是(LPView *)， 而子类需要(SubView *)类型的返回值，就会出现问题，
	将instancetype作为这个方法的返回值后，在父类中的返回值就变成(LPView *) 子类中的返回值类型就变成 （SubView *）类型了
	
	
	* 在苹果早期，是用的id类型来作为返回值的，但是，id类型可以用任何类型接收，可能会调用不存在的方法，让编译检查不出来，运行时就报错了