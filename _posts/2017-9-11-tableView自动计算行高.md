---
title: tableView自动计算行高
layout: post
category: 技术
comments: false

---

	
	//设置tableView的rowHeight属性为UITableViewAutomaticDimension 再在下面的代理方法中返回预估行高,就可以自动计算行高啦
	
	self.myTableView.rowHeight = UITableViewAutomaticDimension;
		 
	 
	 
	 
	 //预估行高
	- (CGFloat)tableView:(UITableView *)tableView estimatedHeightForRowAtIndexPath:(NSIndexPath *)indexPath
	{
	    return 300;
	}