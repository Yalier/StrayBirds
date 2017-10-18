---
layout: post
title: UIPickerView
category: 技术
comments: false
---

### UIPickerView的两个代理

* UIPickerViewDelegate
* UIPickerViewDataSource


### UIPickerView的代理方法

---Delegate
‘
每组的宽度
1. - (CGFloat)pickerView:(UIPickerView *)pickerView widthForComponent:(NSInteger)component
{
}

每组的行高
2. - (CGFloat)pickerView:(UIPickerView *)pickerView rowHeightForComponent:(NSInteger)component
{
}

每行的内容返回的是字符串
3. - (nullable NSString *)pickerView:(UIPickerView *)pickerView titleForRow:(NSInteger)row forComponent:(NSInteger)component 
{
}

每行的内容返回的是有属性字符串
4. - (nullable NSAttributedString *)pickerView:(UIPickerView *)pickerView attributedTitleForRow:(NSInteger)row forComponent:(NSInteger)component 
{
}

每行的内容返回的是一个View
5. - (UIView *)pickerView:(UIPickerView *)pickerView viewForRow:(NSInteger)row forComponent:(NSInteger)component reusingView:(nullable UIView *)view 
{
}

选择每行
6. - (void)pickerView:(UIPickerView *)pickerView didSelectRow:(NSInteger)row inComponent:(NSInteger)component
{
}

‘


---DataSource
'
有多少组
1. - (NSInteger)numberOfComponentsInPickerView:(UIPickerView *)pickerView
{
}

每组有多少行
2. - (NSInteger)pickerView:(UIPickerView *)pickerView numberOfRowsInComponent:(NSInteger)component
{
}

'
