---
layout: post
title: "excel使用记录"
description: ""
category: note
slug: excel_usage
tags: []
---
{% include JB/setup %}

## 1、宏的使用

- 1、添加宏需要**打开Visual Basic编辑器**。选择：**工具——宏——Visual Basic编辑器**，如下图所示：

![image-20200317103000333](https://tva1.sinaimg.cn/large/00831rSTly1gcwpvnrt4qj317q0na7up.jpg)

- 2、**编辑宏**。进入编辑器以后，选择**插入——模块**，编写宏代码即可。编写完成后保存关闭【保存的话需要另存一份xlsm格式的副本，xlsx格式不支持保存宏，但是似乎不保存也能用，但是关闭文件下次打开就没有宏了】。

![image-20200317103837413](https://tva1.sinaimg.cn/large/00831rSTly1gcwq4krxuvj31ia0u0hdt.jpg)

- 3、**使用宏**。然后返回excel页面，选择：**工具——宏——宏...**，选择刚才添加的宏运行即可，默认名字是cfb。

![image-20200317103334321](https://tva1.sinaimg.cn/large/00831rSTly1gcwpzbdis5j30nu0li40d.jpg)

下面是一些常用的宏代码。

#### 1.1 将一个excel文件按行分隔为多个小的excel文件

使用以下宏，可以将文件按照行数分隔为多个文件，每个文件都含有表头



```visual basic
Sub cfb()
Dim r, c, i, WJhangshu, WJshu, bt As Long
r = Range("A" & Rows.Count).End(xlUp).Row
c = Cells(1, Columns.Count).End(xlToLeft).Column
bt = 1 'title，表头所在的行，行数从1开始
WJhangshu = 1800 'num，每个文件的大小【行数，不包括表头】
WJshu = IIf(r - bt Mod 200000, Int((r - bt) / WJhangshu), Int((r - bt) / WJhangshu) + 1)
For i = 0 To WJshu
    Workbooks.Add
    Application.DisplayAlerts = False
ActiveWorkbook.SaveAs FileName:=ThisWorkbook.Path & "/FileName" & Format(i + 1, String(Len(WJshu), 0)) & ".xlsx"
    Application.DisplayAlerts = True
    ThisWorkbook.ActiveSheet.Range("A1").Resize(bt, c).Copy ActiveSheet.Range("A1")
    ThisWorkbook.ActiveSheet.Range("A" & bt + i * WJhangshu + 1).Resize(WJhangshu, c).Copy _
     ActiveSheet.Range("A" & bt + 1)
    ActiveWorkbook.Close True
Next
End Sub
```



**参考博客**：[https://blog.csdn.net/bohu83/article/details/76273361](https://blog.csdn.net/bohu83/article/details/76273361)