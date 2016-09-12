---
title: Grand Central Dispatch
permalink: grand-central-dispatch
id: 33
updated: '2014-12-24 18:14:12'
tags:
---

- **Serial** and **Concurrent** are the two types of execution queues. Serial queues execute one task at a time while concurrent queues might execute several tasks at the same time.

- **Synchronous** and **Asynchronous** are used to determine whether the dispatch block is returned immediately or after execution finished. This will effect the running of the *current* thread.

- For each queue type (serial or concurrent), there are, by default, five particular queues to choose from:
	+ main queue, this is the queue to put when updating the ui. main queue is serial.
    + global dispatch queues with four priorities, background, low, default, high.

- You can create your own queue.

###### Common usage of GCD
```
- (void)viewDidLoad
{   
    [super viewDidLoad];
    NSAssert(_image, @"Image not set; required to use view controller");
    self.photoImageView.image = _image;
 
    //Resize if neccessary to ensure it's not pixelated
    if (_image.size.height <= self.photoImageView.bounds.size.height &&
        _image.size.width <= self.photoImageView.bounds.size.width) {
        [self.photoImageView setContentMode:UIViewContentModeCenter];
    }
 
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_HIGH, 0), ^{ // 1
        UIImage *overlayImage = [self faceOverlayImageFromImage:_image];
        dispatch_async(dispatch_get_main_queue(), ^{ // 2
            [self fadeInNewImage:overlayImage]; // 3
        });
    });
}
```

```
- (void)showOrHideNavPrompt
{
    NSUInteger count = [[PhotoManager sharedManager] photos].count;
    double delayInSeconds = 1.0;
    dispatch_time_t popTime = dispatch_time(DISPATCH_TIME_NOW, (int64_t)(delayInSeconds * NSEC_PER_SEC)); // 1 
    dispatch_after(popTime, dispatch_get_main_queue(), ^(void){ // 2 
        if (!count) {
            [self.navigationItem setPrompt:@"Add photos with faces to Googlyify them!"];
        } else {
            [self.navigationItem setPrompt:nil];
        }
    });
}
```
