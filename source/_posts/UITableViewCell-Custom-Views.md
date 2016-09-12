---
title: UITableViewCell Custom Views
permalink: uitableview-cell-custom-views
id: 6
updated: '2015-01-13 23:24:29'
date: 2015-01-13 23:22:11
tags:
---

When using UITableViewCell, you sometime will add subviews to the cell. But directly add the subview using `addSubview` will have some wired bugs. When this happens, try using `contentView.addSubview` or initialize `backgroundView` and then add subview to the `backgroundView` property of the cell.