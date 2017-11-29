---
title: 多线程之NSOperation
layout: post
category: 技术
comments: fasle

---



	@interface ViewController ()
	
	@property (nonatomic)NSOperationQueue *myQueue;
	
	@end
	
	
	
	@implementation ViewController
	
	
	- (NSOperationQueue *)myQueue
	{
	    if (_myQueue == nil)
	    {
	        _myQueue = [[NSOperationQueue alloc] init];
	        //最大并发数 同时有几个任务在执行
	        _myQueue.maxConcurrentOperationCount = 2;
	    }
	    
	    return _myQueue;
	}
	- (IBAction)cancleBtn:(UIButton *)sender
	{
	    
	    //取消所有操作
	    [self.myQueue cancelAllOperations];
	    
	}
	
	- (IBAction)pauseBtn:(UIButton *)sender
	{
	    //暂停
	    self.myQueue.suspended = YES;
	}
	- (IBAction)continueLp:(UIButton *)sender
	{
	    //继续
	    self.myQueue.suspended = NO;
	}
	
	- (void)viewDidLoad
	{
	    [super viewDidLoad];
	}
	
	
	- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
	{
	    
	//    [self demo1];
	    
	//    [self demo2Block];
	    
	//    [self demo3];
	    
	//    [self demo4];
	//    [self demo5];
	    [self demo6];
	    
	    
	}
	
	- (void)demo6
	{
	    
	    for (int i = 0; i<600; i++)
	    {
	        [self.myQueue addOperationWithBlock:^{
	            
	            NSLog(@"queueLP -- %@, %@", @(i), [NSThread currentThread]);
	        }];
	        
	    }
	    
	}
	
	- (void)demo5
	{
	    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
	    [queue addOperationWithBlock:^{
	       
	        NSLog(@"下载图片, %@", [NSThread currentThread]);
	    
	        [[NSOperationQueue mainQueue] addOperationWithBlock:^{
	            
	            NSLog(@"更新UI, %@", [NSThread currentThread]);
	        }];
	    }];
	    
	}
	
	
	- (void)demo4
	{
	    NSBlockOperation *blockMy = [NSBlockOperation blockOperationWithBlock:^{
	        NSLog(@"嘿嘿嘿");
	    }];
	    
	    [blockMy setCompletionBlock:^{
	        
	        NSLog(@"已经完成了");
	    }];
	    
	    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
	    [queue addOperation:blockMy];
	}
	
	
	- (void)demo3
	{
	    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
	    [queue addOperationWithBlock:^{
	    
	        NSLog(@"添加block ,%@", [NSThread currentThread]);
	    }];
	}
	
	
	- (void)demo2Block
	{
	    NSBlockOperation *myBlock = [NSBlockOperation blockOperationWithBlock:^{
	        
	        
	        NSLog(@"我的block %@", [NSThread currentThread]);
	    }];
	    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
	    [queue addOperation:myBlock];
	}
	
	
	- (void)demo1
	{
	    NSInvocationOperation *invocation = [[NSInvocationOperation alloc] initWithTarget:self selector:@selector(invocation) object:nil];
	//    [invocation setCompletionBlock:^{
	//
	//
	//    }];
	    
	    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
	    [queue addOperation:invocation];
	}
	
	
	- (void)invocation
	{
	    NSLog(@"invocation %@", [NSThread currentThread]);
	}
	
	
	
	
	
	//操作之间的依赖关系  可以跨队列
	
    NSBlockOperation *ll = [NSBlockOperation blockOperationWithBlock:^{
        NSLog(@"1");
    }];
    NSBlockOperation *ll2 = [NSBlockOperation blockOperationWithBlock:^{
        NSLog(@"2");
    }];
    
    NSBlockOperation *ll3 = [NSBlockOperation blockOperationWithBlock:^{
        NSLog(@"3");
    }];
    
    [ll3 addDependency:ll];
    [ll2 addDependency:ll3];
    
    NSOperationQueue *qu = [[NSOperationQueue alloc] init];
    [qu addOperations:@[ll, ll2] waitUntilFinished:NO];
    
    
    NSOperationQueue *qu2 = [[NSOperationQueue alloc] init];
    [qu2 addOperation:ll3];
	
	
	@end
