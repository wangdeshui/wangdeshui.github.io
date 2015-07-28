---
layout: post
category : 移动开发
title: iOS UI系列 (四) ：可复用的Xib(1) 静态内容 
date: 2015-07-28 7:00:00
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




有时候页面中的部分内容相同，或者是一些静态的内容组合，这时候我们就可以把这些见面封装到一个XIB里

## 新建Single View Application

## 新建一个View.Xib

1. Command+N-->User Interface-->View
2. 把界面大小改为Freeform
3. 添加一个UILabel, 两个UIView, 并设置对应的背景色
4. 添加对应的约束，让两个UIView等宽，且Space都是10， 高度固定，且与周围的约速为10， 对UILabel也设置对应的约速，细节就不写了，看图

<img src="/assets/images/ios/UI/4/1.png" />


## 使用Xib

在ViewController添加 一个UIView, 并设置对应的约束，连接这个UIView为Controller的 IBOutlet ContainerView


<img src="/assets/images/ios/UI/4/2.png" />

1. 在ViewDidLoad里添加如下代码


		super.viewDidLoad()
		        
		let arr = NSBundle.mainBundle().loadNibNamed("View", owner: nil, options: nil)

		let v = arr[0] as! UIView
		        
		containerView.addSubview(v)


	运行

	<img src="/assets/images/ios/UI/4/3.png" />

	试图添加进去了，但是试图显示的不对，那是因为没有添加约束
	
2. 添加约束



		import UIKit

		class ViewController: UIViewController {
    
	    @IBOutlet weak var containerView: UIView!
	    
	    var v: UIView!
	    
	    override func viewDidLoad() {
	        
	        super.viewDidLoad()
	        
	        let arr = NSBundle.mainBundle().loadNibNamed("View", owner: nil, options: nil)
	        v = arr[0] as! UIView
	        
	        
	        containerView.addSubview(v)
	        
	        setUpConstraint()
	        
	        
	        // Do any additional setup after loading the view, typically from a nib.
	    }
	    
	    override func didReceiveMemoryWarning() {
	        super.didReceiveMemoryWarning()
	        // Dispose of any resources that can be recreated.
	    }
	    
	    
	    func setUpConstraint()
	    {
	        
	        v.setTranslatesAutoresizingMaskIntoConstraints(false)
	        
	        containerView.addConstraint(NSLayoutConstraint(
	            item:v, attribute:.Leading,
	            relatedBy:.Equal, toItem:containerView,
	            attribute:.Left, multiplier:1, constant:0))
	        
	        containerView.addConstraint(NSLayoutConstraint(
	            item:v, attribute:.Trailing,
	            relatedBy:.Equal, toItem:containerView,
	            attribute:.Right, multiplier:1, constant:0))
	        
	        containerView.addConstraint(NSLayoutConstraint(
	            item:v, attribute:.Top,
	            relatedBy:.Equal, toItem:containerView,
	            attribute:.Top, multiplier:1, constant:0))
	        
	        containerView.addConstraint(NSLayoutConstraint(
	            item:v, attribute:.Bottom,
	            relatedBy:.Equal, toItem:containerView,
	            attribute:.Bottom, multiplier:1, constant:0))
	        
	    }	    
	    
		}

	
3. 运行结果, 我们的Xib已经可以自适应容器了

<img src="/assets/images/ios/UI/4/4.png" />
<img src="/assets/images/ios/UI/4/5.png"/>


    
    