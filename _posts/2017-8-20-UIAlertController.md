---
title: UIAlertController
layout: post
category: 技术
comments: false
---

"

    UIAlertController *alertVC = [UIAlertController alertControllerWithTitle:@"iOS8.0" message:@"亲测" preferredStyle:UIAlertControllerStyleAlert];

        UIAlertAction *ac1 = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {

            NSLog(@"点击了确定");
        }];

        [alertVC addAction:ac1];

        [self presentViewController:alertVC animated:YES completion:^{


        }];

"
