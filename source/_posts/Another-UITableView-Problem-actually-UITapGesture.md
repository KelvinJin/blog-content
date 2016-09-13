title: Another UITableView Problem - actually UITapGesture
permalink: another-uitableview-proble
id: 25
updated: '2015-03-20 10:11:41'
tags:
  - Swift
categories:
  - Programming
date: 2015-03-19 22:55:00
---
I've been working with UITableView for a long time...

<!--- more --->

So, here is the background:

- I have a UIViewController which contains a horizontal table view wrapped inside a UIView.
- One day, I want to add a new horizontal table view right below the existing one.
- So I did the same thing as what I had done before except that I used a container view (child controller) to handle the delegate and dataSource responsibilities for the new table.
- For the existing table, everything works fine as expected. I can select the cell with one gentle touch. The new table, however, didn't response my gentle touch at all. 
- I override `touchBegan:` method inside the custom cell class and printed out the message once I touched it, but it turned out that only when I touch it for over 500ms (approximately), the message would be printed.
- What worse, the `didSelectedRowAtIndexPath:` never got called no matter how long I pressed the cell which means I can't select the cell at all!

After about 5 hours struggling for this problem, I decided to have a careful search on Google (I had done some searching before but with no luck. I had thought this shouldn't be a very hard problem so I tried to solve it myself. How stupid I was...) And finally I find [this](http://stackoverflow.com/questions/255927/didselectrowatindexpath-not-being-called), a list of the reasons why `didSelectedRowAtIndexPath:` not being called. Luckily, I found this:
> Another possibility is that a UITapGestureRecognizer could be eating the events, as was the case here: http://stackoverflow.com/a/9248827/214070

And it lead me to the solution (Thanks God!):
> I have found the answer. I had a UITapGestureRecognizer set for myTableView's superView. This overrode the selection call. Credit to whoever suggested that that might be it. Your answer was deleted before I could mark it correct.

> Set the `cancelsTouchesInView` property to `NO` on the gesture recogniser to allow the table view to intercept the event.

The only thing I don't understand is why the existing table view is not affected???