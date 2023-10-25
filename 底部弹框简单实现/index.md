# 底部弹框简单实现

<!--more-->
## 前言
底部弹框,类似微信的分享功能,动画效果有两个:<br>
- 从底部弹出内容
- 从底部弹出内容,并且背景变暗
## 效果
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/弹框效果.gif" title="弹框效果" width="40%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b>  </b> 弹框效果 </div>
</center>

## 思路
直接模态一个控制器,自定义转场动画,并且给内容view添加一个改变frame的动画就可以了
## 代码
### 转场动画
```swift
class CustomPresentAnimator: NSObject, UIViewControllerAnimatedTransitioning {
    
    private var duration: TimeInterval = 0.3
    private var maskColor: UIColor = UIColor.black
    private var maskAlpha: CGFloat = 0.6
    private var present = true
    convenience init(_ present: Bool = true,
                     maskColor: UIColor? = nil,
                     maskAlpha: CGFloat? = nil,
                     duration: TimeInterval? = nil) {
        self.init()
        if let maskColor = maskColor {
            self.maskColor = maskColor
        }
        if let maskAlpha = maskAlpha {
            self.maskAlpha = maskAlpha
        }
        if let duration = duration {
            self.duration = duration
        }
        self.present = present
    }
    func transitionDuration(using transitionContext: UIViewControllerContextTransitioning?) -> TimeInterval {
        return duration
    }
    
    func animateTransition(using transitionContext: UIViewControllerContextTransitioning) {
        guard let toVC = transitionContext.viewController(forKey: .to) else {return}
        guard let fromVC = transitionContext.viewController(forKey: .from) else {return}
        let containerView = transitionContext.containerView
        if present {
            containerView.addSubview(toVC.view)
            toVC.view.alpha = 0.0
            toVC.view.backgroundColor = .clear
            UIView.animate(withDuration: duration, animations: {[weak self] in
                guard let wself = self else { return }
                toVC.view.alpha = 1.0
                toVC.view.backgroundColor = wself.maskColor.withAlphaComponent(wself.maskAlpha)
            }, completion: { finished in
                transitionContext.completeTransition(finished)
            })
        } else {
            UIView.animate(withDuration: duration, animations: {
                fromVC.view.alpha = 0.0
                fromVC.view.backgroundColor = .clear
            }, completion: { finished in
                transitionContext.completeTransition(finished)
            })
        }
    }
}
```
### 弹框控制器
```swift
class CommonModalController: UIViewController,UIViewControllerTransitioningDelegate {
    
    let contentView = UIView()
    let pageBottomPadding = -34 // 需要减去底部安全区域的高度,这里省略机型判断,默认是iPhoneX以上机型
    private var contentSize: CGSize = .zero
    private var touchView: UIView = UIView()
    convenience init(contentSize: CGSize) {
        self.init()
        self.modalPresentationStyle = .custom
        self.transitioningDelegate = self
        self.contentSize = contentSize
        self.contentView.frame = CGRectMake(0, self.view.bounds.size.height, self.view.bounds.size.width, self.contentSize.height - pageBottomPadding)
        self.touchView.frame = CGRectMake(0, 0, self.view.bounds.size.width, self.view.bounds.size.height - self.contentSize.height + pageBottomPadding)
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        UIView.animate(withDuration: 0.3,delay: 0) {[weak self] in
            guard let wself = self else { return }
            wself.contentView.frame = CGRectMake(0, wself.view.bounds.size.height - wself.contentSize.height + pageBottomPadding, wself.view.bounds.size.width, wself.contentSize.height - pageBottomPadding)
        }
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        UIView.animate(withDuration: 0.3,delay: 0) {[weak self] in
            guard let wself = self else { return }
            wself.contentView.frame = CGRectMake(0, wself.view.bounds.size.height, wself.view.bounds.size.width, wself.contentSize.height - pageBottomPadding)
        }
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        self.contentView.backgroundColor = .white
        self.contentView.layer.cornerRadius = 18
        self.contentView.layer.maskedCorners = [.layerMinXMinYCorner, .layerMaxXMinYCorner]
        let tap = UITapGestureRecognizer(target: self, action: #selector(dismissView))
        self.touchView.addGestureRecognizer(tap)
        self.view.addSubview(touchView)
        self.view.addSubview(contentView)
    }

    @objc func dismissView() {
        self.dismiss(animated: true)
    }
    
    func animationController(forPresented presented: UIViewController, presenting: UIViewController, source: UIViewController) -> UIViewControllerAnimatedTransitioning? {
        return CustomPresentAnimator()
    }
    
    func animationController(forDismissed dismissed: UIViewController) -> UIViewControllerAnimatedTransitioning? {
        return CustomPresentAnimator(false)
    }
    
    deinit {
        debugPrint("\(self)\(#function)")
    }
}
```
### 使用
```swift
let vc = CommonModalController(contentSize: CGSizeMake(self.view.bounds.size.width, 200))
self.present(vc, animated: true)
```
这里只是简单的实现了一个底部弹框,如果需要更多的动画效果,可以自己去实现,这里只是提供一个思路

