---
title: TableView的复制，粘贴功能
layout :post
category: 技术
comments: false

---


	tableView中的复制粘贴功能，就在它自带的代理方法中就有
	//copy/cut/paste
	- (BOOL)tableView:(UITableView *)tableView shouldShowMenuForRowAtIndexPath:(NSIndexPath *)indexPath
	{
	    return YES;
	}
	- (BOOL)tableView:(UITableView *)tableView canPerformAction:(SEL)action forRowAtIndexPath:(NSIndexPath *)indexPath withSender:(nullable id)sender
	{
	    return YES;
	}
	- (void)tableView:(UITableView *)tableView performAction:(SEL)action forRowAtIndexPath:(NSIndexPath *)indexPath withSender:(nullable id)sender
	{
	 
	    NSLog(@"%@", NSStringFromSelector(action));
	
	    NSString *str = self.myArray[indexPath.row];
	    //通用剪贴板
	    UIPasteboard *pasteB = [UIPasteboard generalPasteboard];
	    
	    if (action == @selector(copy:))
	    {
	        pasteB.strings = @[str];
	        
	    }else if (action == @selector(paste:))
	    {
	        
	        [self.myArray replaceObjectAtIndex:indexPath.row withObject:pasteB.strings[0]];
	        [self.myTabelView reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationLeft];
	    }
	    
	    
	    
	}

	