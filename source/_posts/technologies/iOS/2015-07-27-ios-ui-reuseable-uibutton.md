---
layout: post
category : 移动开发
title: iOS UI系列 (三) ：Reusable Button 
date: 2015-07-27 11:00:00
tags: [iOS]
---
<style>
img {
  max-width: 700px;
  border: solid 2px #ccc;
  padding: 5px;
  border-radius:5px;
}
</style>


有时候我们需要给一些做一些设置，但是这些控件却需要用在多个地方，如果在每一个ViewController都设置一遍，那么代码就不整洁了，而且比较耗时间。

创建一个RoundButton.swift 文件，集成自UIButton



```swift
import UIKit

class RoundButton: UIButton {

    /*
    // Only override drawRect: if you perform custom drawing.
    // An empty implementation adversely affects performance during animation.
    override func drawRect(rect: CGRect) {
        // Drawing code
    }
    */
    
    required init(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
    }
    
    override init(frame: CGRect) {
        super.init(frame: frame)
    }
    
    override func awakeFromNib() {
      
        self.layer.cornerRadius=10
        self.layer.borderColor=UIColor.redColor().CGColor
        self.layer.borderWidth=2
        self.layer.backgroundColor=UIColor.yellowColor().CGColor
        self.contentEdgeInsets=UIEdgeInsets(top: 10,left: 10,bottom: 10,right: 10)
    }
}
```


​    
​    
​    
 设置UIButton的Custom class为 RoundButton


<img class="img-responsive" src="https://cdn.jsdelivr.net/gh/wangdeshui/blogpics@master/ios/UI/3/1.png" />
    
<img class="img-responsive" src="https://cdn.jsdelivr.net/gh/wangdeshui/blogpics@master/ios/UI/3/2.png" />




