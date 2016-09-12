---
title: iOS notes
permalink: ios-notes-2
id: 31
updated: '2015-04-23 10:21:56'
tags:
---

* In a table view, when we select the selected cell again (tap and release on it), the following processes happen:
	* ~~Cell: setSelect(false, ...)~~
    * ~~Delegate: tableview: didDeselecRowAtIndexPath:~~
    * ~~Delegate: tableview: willSelectRowAtIndexPath:~~
    * Cell: setSelect(true, ...)
    * Delegate: tableview: didSelectRowAtIndexPath:
    
   Be careful that tableview: willDeselectRowAtIndexPath: won't get called!!! The reason from official API doc:
   > This method is only called if there is an existing selection when the user tries to select a different row. The delegate is sent this method for the previously selected row. You can use UITableViewCellSelectionStyleNone to disable the appearance of the cell highlight on touch-down.至少用户一开始看到的就是正在渲染，而且不用call setNeedsLayout了


TableView的Separator 默认高度是1px 而不是1 pt，所以如果需要创建一个和separator一样高度的view，需要使用 `1.0 / UIScreen.mainScreen().scale`。