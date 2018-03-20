--
title : tableView编辑
category : 技术
comments : false
layout : post

---



	// cell滑动编辑方法
	- (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath {
	    return YES;
	}
	- (NSArray<UITableViewRowAction *> *)tableView:(UITableView *)tableView editActionsForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    __weak typeof(self) weakSelf = self;
    UITableViewRowAction *morenAction = [UITableViewRowAction rowActionWithStyle:(UITableViewRowActionStyleNormal) title:@"设为默认卡" handler:^(UITableViewRowAction * _Nonnull action, NSIndexPath * _Nonnull indexPath) {
        
        // 提现卡才能设置为默认卡(充值0 提现2)
        NSString *type = STR([self.cardArr[indexPath.section] valueForKey:@"type"]);
        if ([type isEqualToString:@"0"]) {
            
            [weakSelf showAlertViewWithTitle:@"请选择提现卡设置为默认卡"];
            return ;
        }
        // 设置默认卡
        NSString *cardNum = STR([self.cardArr[indexPath.section] valueForKey:@"cardNo"]);
        [weakSelf requestDataForSetMorenCardWithCardNo:cardNum];
    }];
    UITableViewRowAction *deleteAction = [UITableViewRowAction rowActionWithStyle:(UITableViewRowActionStyleDefault) title:@"删除该卡" handler:^(UITableViewRowAction * _Nonnull action, NSIndexPath * _Nonnull indexPath) {
        
        // 删除当前卡
        NSString *cardNo = STR([self.cardArr[indexPath.section] valueForKey:@"cardNo"]);
        NSString *cardType = STR([self.cardArr[indexPath.section] valueForKey:@"type"]);
        [weakSelf requestDataForDeleteCardWithCardNo:cardNo type:cardType];
    }];
    return @[deleteAction,morenAction];
	}